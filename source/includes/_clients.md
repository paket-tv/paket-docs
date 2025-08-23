# Tenants and Clients

To begin working with the Paket API, Partners must first create a Tenant in the [Paket Developer Portal](https://developer.paket.tv). 

- **Platforms** should register as a **Platform Tenant**
- **Publishers** should register as an **App Tenant**

Once a Tenant is created, one or more **API Clients** can be provisioned. API Clients represent application instances that communicate with the Paket API and exchange data between Tenants. Clients are typically scoped at the **application/environment** level (e.g., iOS app, Android app, web app, OTT app, dev vs. prod).

<aside class="notice">
A Platform such as Samsung might create a Tenant for Tizen OS, whereas a Publisher such as Disney Streaming Services might create discrete Apps each for Hulu, Disney+, and ESPN.
</aside>