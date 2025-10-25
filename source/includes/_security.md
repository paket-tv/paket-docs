# Security

Securing API requests is critical to ensure data integrity. The Paket API employs the following methods to ensure that requests made to its API are authenticated and secure:

## Security Features

- **Forced HTTPS**: Connections are strictly enforced over HTTPS at `https://api.paket.tv/v1`
- **TLS/SSL**: X.509 server certificates to validate server requests are authenticated as Paket Media, Inc.
- **Basic Auth**: Basic authentication required for all API requests with unique Client username and rollable secret keys
- **HMAC-SHA256 payload signing**: Required for making API requests
- **IP Whitelisting**: Restrict access to API requests to your server IP addresses designated at the Client level
- **Encryption-at-rest**: All data stored is encrypted-at-rest

## Signing API Requests

The Paket API requires **HMAC-SHA256 payload signing** to ensure the integrity and authenticity of the request body. This feature helps protect against tampering or replay attacks at the payload level.

### How It Works

Each API client uses its existing secret key (provided with the client credentials) to compute an HMAC signature of the request payload. 

When a signed request is received, Paket will:

- Verify the signature matches the body and timestamp using the shared secret key.
- Ensure the request timestamp is within a 5-minute tolerance window.
- Log the signature status for audit and observability purposes.

### Headers To Include

To sign your request, add the following headers:

**`X-Paket-Timestamp`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Unix timestamp in milliseconds

**`X-Paket-Signature`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
`sha256=` followed by HMAC of payload

### Signature Format

> Example HMAC-SHA256 payload signing (Node.js)

```javascript
const crypto = require('crypto')

function signPaketRequest(secretKey, body, timestamp) {
  const payload = `${timestamp}.${body}`
  const signature = crypto
    .createHmac('sha256', secretKey)
    .update(payload)
    .digest('hex')

  return `sha256=${signature}`
}

const secretKey = 'your_client_secret_key'
const body = JSON.stringify({ plan_id: 'xyz', session_id: 'abc' })
const timestamp = Date.now().toString()

const headers = {
  'Authorization': 'Basic base64(username:secret)',
  'Content-Type': 'application/json',
  'X-Paket-Timestamp': timestamp,
  'X-Paket-Signature': signPaketRequest(secretKey, body, timestamp),
}
```

The signature is computed as:

`HMAC_SHA256(secret_key, timestamp + '.' + request_body)`

- `secret_key`: your client’s API secret
- `timestamp`: the current UNIX timestamp in milliseconds
- `request_body`: the raw body string (or '' if there is no body)

The result should be hex-encoded and prefixed with `sha256=`.

### DELETE Requests Without a Body

When sending a `DELETE` request (or any request without a body), sign an empty string as the body:

```javascript
const body = ''  // DELETE usually has no body
const payload = `${timestamp}.${body}`
```

## Securing Webhooks

Webhooks play a key role in communicating critical information (in the form of events) among Platform and Publisher Clients and the Paket service. While not technically a part of the Paket API, this section explains how to set up and secure your webhooks endpoints; and serves as a reference as to which notifications are sent and on which triggering events.

### Configure your endpoints

Webhooks are configured discretely under each Client object in the [Paket Developer Portal](https://developer.paket.tv). You may configure multiple endpoints, however, please note that at present all events will be sent to all active endpoints.

Webhook endpoints must be secured using TLS/SSL certificates over HTTPS and are signed using a signing secret, provided on creation of each endpoint. While it is possible to receive events without [verifying the webhook signature](#webhooks-configure-your-endpoints-verify-webhook-signature), it is highly recommended that each event is verified before taking any programmatic action.

![Webhooks Configuration](webhooks.png)

### Verify Webhook Signature

The `Paket-Signature` header included in each signed event contains a timestamp and one or more signatures that you must verify. The timestamp is prefixed by `t=`, and each signature is prefixed by a scheme. Schemes start with `v`, followed by an integer. Currently, the only valid live signature scheme is `v1`. To aid with testing, Paket sends an additional signature with a fake `v0` scheme.

> Example `Paket-Signature` header

```
Paket-Signature:
t=1709156882568,
v1=f03a63e17d5720d7d01974d115059c8ec05b0f425828a2d0d341020f431bbafa,
v0=3e3be55cd51ef0476ddf7fe83e72f10cf7358e54dd8eab17560dfb879b9d485a
```

Paket generates signatures using a hash-based message authentication code (HMAC) with SHA-256. To prevent downgrade attacks, ignore all schemes that aren’t `v1`.

You can have multiple signatures with the same scheme-secret pair when you roll an endpoint’s secret, and keep the previous secret active for up to 24 hours. During this time, your endpoint has multiple active secrets and Paket generates one signature for each secret.

To create a manual solution for verifying signatures, you must complete the following steps:

#### Step 1: Extract the timestamp and signatures from the header

Split the header using the `,` character as the separator to get a list of elements. Then split each element using the `=` character as the separator to get a prefix and value pair.

The value for the prefix `t` corresponds to the timestamp, and `v1` corresponds to the signature (or signatures). You can discard all other elements.

#### Step 2: Prepare the `signed_payload` string

> Example  `signed_payload`

```
1709156882568.{"idempotency_key":"62a0152b-6ac4-49f7-b605-5e18cbf70ed8","data":{"context_id":"6b0af623-2923-4997-92a6-73f94bbe321e","client_id":"286d2a536a5d6b6b","app_id":"417f3625d067cbe3","created_at":"2024-03-05T17:56:26.265Z","session_id":"4c682b9e-65b4-4e43-a5e9-96f58a967240"},"type":"context.session.created","api_version":"2023-12-10","created":1709661388431,"object":"event","id":"req_ad8a67d675a71a89"}
```

The `signed_payload` string is created by concatenating:

- The timestamp (as a string)
- The character `.`
- The actual JSON payload (that is, the request body)

#### Step 3: Determine the expected signature

> Example Hashed result

```
f03a63e17d5720d7d01974d115059c8ec05b0f425828a2d0d341020f431bbafa
```

Compute an HMAC with the SHA256 hash function. Use the endpoint’s signing secret as the key, and use the signed_payload string as the message.

#### Step 4: Compare the signatures

Compare the signature (or signatures) in the header to the expected signature. For an equality match, compute the difference between the current timestamp and the received timestamp, then decide if the difference is within your tolerance.

To protect against timing attacks, use a constant-time-string comparison to compare the expected signature to each of the received signatures.

<!-- ### Monitoring endpoints

Add section when dashboard monitoring goes live. -->

### Paket IP addresses

To further protect your endpoints, it is recommended that you implement IP whitelisting of the Paket servers from which you may receive webhook events.

Paket IP Addresses:

- `54.185.102.16`
- `52.40.235.54`
- `35.83.22.57`

### Retry Behavior

Paket attempts to deliver a given event to your webhook endpoint up to fifteen (15) times over the course of 3 days with an exponential back off. In the Webhooks logs section of the Paket Publisher Portal, you can view when the next retry will occur as well as the event and event data itself.