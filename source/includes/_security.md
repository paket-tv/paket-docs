# Security

Securing API requests is critical to ensure data integrity. The Paket API employs the following methods to ensure that requests made to its API are authenticated and secure:

- **Forced HTTPS**: Connections are strictly enforced over HTTPS at `https://api.paket.tv/v1`.
- **TLS/SSL**: X.509 server certificates to validate server requests are authenticated as Paket Media, Inc.
- **Basic Auth**: Basic authentication required for all API requests w/unique Client username and rollable secret keys. 
- **IP Whitelisting**: Restrict access to API requests to your server IP addresses designated at the Client level.
- **Encryption-at-rest**: All data stored is encrypted-at-rest.