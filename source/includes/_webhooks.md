# Webhooks

Webhooks play a key role in communicating critical information (in the form of events) among Platform and Publisher Clients and the Paket service. While not technically a part of the Paket API, this section explains how to set up and secure your webhooks endpoints; and serves as a reference as to which notifications are sent and on which triggering events.

## Configure your endpoints

Webhooks are configured discretely under each Client object in the Paket Developer Portal. You may configure multiple endpoints, however, please note that at present all events will be sent to all active endpoints.

Webhook endpoints must be secured using TLS/SSL certificates over HTTPS and are signed using a signing secret, provided on creation of each endpoint. While it is possible to receive events without [verifying the webhook signature](#webhooks-configure-your-endpoints-verify-webhook-signature), it is highly recommended that each event is verified before taking any programmatic action.

![Webhooks Configuration](webhooks.png)

### Verify webhook signature

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

**Step 1: Extract the timestamp and signatures from the header**

Split the header using the `,` character as the separator to get a list of elements. Then split each element using the `=` character as the separator to get a prefix and value pair.

The value for the prefix `t` corresponds to the timestamp, and `v1` corresponds to the signature (or signatures). You can discard all other elements.

**Step 2: Prepare the `signed_payload` string**

> Example  `signed_payload`

```
1709156882568.{"idempotency_key":"62a0152b-6ac4-49f7-b605-5e18cbf70ed8","data":{"participant_id":"6b0af623-2923-4997-92a6-73f94bbe321e","client_id":"286d2a536a5d6b6b","app_id":"417f3625d067cbe3","created_at":"2024-03-05T17:56:26.265Z","session_id":"4c682b9e-65b4-4e43-a5e9-96f58a967240"},"type":"participant.session.created","api_version":"2023-12-10","created":1709661388431,"object":"event","id":"req_ad8a67d675a71a89"}
```

The `signed_payload` string is created by concatenating:

- The timestamp (as a string)
- The character `.`
- The actual JSON payload (that is, the request body)

**Step 3: Determine the expected signature**

> Example Hashed result

```
f03a63e17d5720d7d01974d115059c8ec05b0f425828a2d0d341020f431bbafa
```

Compute an HMAC with the SHA256 hash function. Use the endpoint’s signing secret as the key, and use the signed_payload string as the message.

**Step 4: Compare the signatures**

Compare the signature (or signatures) in the header to the expected signature. For an equality match, compute the difference between the current timestamp and the received timestamp, then decide if the difference is within your tolerance.

To protect against timing attacks, use a constant-time-string comparison to compare the expected signature to each of the received signatures.

<!-- ### Monitoring endpoints

Add section when dashboard monitoring goes live. -->

### Paket IP addresses

To further protect your endpoints, it is recommended that you implement IP whitelisting of the Paket servers from which you will receive webhook events.

Paket IP Addresses:

- `35.83.22.57`

## Events

Events are our way of letting you know when something interesting happens in your account. When an interesting event occurs, we create a new `Event` object. For example, when a Participant is added to a Session, we create a `participant.session.participant_added` event, and when a bundle subscription is canceled, we create an `participant.subscription.canceled` event.

### The Event object

> Example webhook event

```
{
  "id": "evt_a75f6d23be8c17b1",
  "object": "event",
  "type": "participant.session.participant_added",
  "created": 1709679883853,
  "api_version": "2023-12-10",
  "request": {
    "id": "req_2442fd8627bedb77",
    "idempotency_key": null
  },
  "data": {
    "participant_id": "6b0af623-2923-4997-92a6-73f94bbe321e",
    "client_id": "286d2a536a5d6b6b",
    "app_id": "417f3625d067cbe3",
    "created_at": "2024-03-05T23:04:43.684Z",
    "session_id": "4c682b9e-65b4-4e43-a5e9-96f58a967240"
  }
}
```
**Attributes**

**`id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The unique identifier of the webhook event.

**`object`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Description of the object type (e.g., `event`).

**`type`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Description of the event (e.g., `participant.session.participant_added` or `participant.subscription.canceled`)

**`created`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
`UTC` timestamp when the event was created. 

**`api_version`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The Paket API version through which the triggering API request was made.

**`request`** <span style='margin: 0 5px;font-size:.9em'>object</span>  
An object containing details of the event's triggering API request.

**`request.id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The unique identifier of the event's triggering API request.

**`request.idempotency_key`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The `idempotency_key`, if any, of the original request. Otherwise `null`.

**`data`** <span style='margin: 0 5px;font-size:.9em'>object</span>  
An object containing the data associated with the event.

### Types of events

This is a list of all the types of events we currently send. We may add more at any time, so in developing and maintaining your code, you should not assume that only these types exist.

You’ll notice that these events follow a pattern: `resource.event`. Our goal is to design a consistent system that makes things easier to anticipate and code against.

**Event**

**`participant.session.deleted`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is a [session participant](#core-api-sessions-the-session-participant-object)</span>  
Occurs when a Session is removed. Sent to the related Publishers only.

**`participant.session.participant_added`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is a [session participant](#core-api-sessions-the-session-participant-object)</span>  
Occurs when a Participant is added to a Session. Sent to the related Publishers only.

**`participant.session.participant_removed`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is a [session participant](#core-api-sessions-the-session-participant-object)</span>  
Occurs when a Participant is removed from a Session. Sent to the related Publishers only.

**`participant.list.item_hidden'`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is a [list item](#upnext-api-overview-the-list-item-object)</span>  
Occurs when a Participant user removes or hides an item from their List via the Platform. Sent to the related Publisher only.