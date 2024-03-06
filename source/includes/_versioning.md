# Versioning

When backwards-incompatible changes are made to the API, we release a new, dated version. The current version is `2023-12-01`. For information on all API updates, view our API changelog.

By default, requests made to the Paket API use your account’s default API version (controlled in the Developer Portal) unless you override it by setting the Paket-Version header.

Webhook events also use your account’s API version by default, unless you set an API version during endpoint creation.

You can upgrade your API version in the Paket Developer Portal. As a precaution, use API versioning to test a new API version before committing to an upgrade.