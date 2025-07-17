# Tenants and Clients

To begin working with the Paket API, Partners must first create a Tenant in the [Paket Developer Portal](https://developer.paket.tv). For Platforms, the tenant type will be a Platform. For Publishers, an App.

Once a Tenant is created, one or more API Clients can be created. Clients are used to communicate with the Paket API and to exchange data among Tenants and are typically scoped at the platform level (e.g., iOS, Android, Web, OTT, etc.).

It is at the Client level that API credentials are issued and at the Tenant level where webhooks are delivered and API products - such as Products or Bundles - are configured.

Tenants should correlate broadly to an operating system or platform, in the case of Platform accounts; or to an application, in the case of Publisher accounts.

<aside class="notice">
A Platform such as Samsung might create a Tenant for Tizen OS, whereas a Publisher such as Disney Streaming Services might create discrete Apps each for Hulu, Disney+, and ESPN.
</aside>