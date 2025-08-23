# Authentication

The Paket API uses basic access authentication to authenticate all incoming API requests (please note there are additional [Security](#security) protocols used to secure API requests).

## Basic Authentication

> Authenticated Request

```shell
curl https://api.paket.tv/v1 \
  -H "Authorization: Basic [Base64 encoding of username and password]"
```

Authenticating via Basic Auth involves passing a Base64 encode of your Client's `username` and `password`, prefixed with `Basic` within the request's `Authorization` header.

### Obtaining API Credentials

API credentials can be found in the [Paket Developer Portal](https://developer.paket.tv) within the Client's API Credentials tab. The `username` and `password` are itemized as **'API Username'** and **'API Secret Key'**, respectively:

![Paket API Portal: Credentials](api_credentials.png)


<aside class="notice">
Rolled Secret Keys will remain valid for the duration specified before they are permanently expired.
</aside>