# Introduction

Welcome to Paket! This documentation explains key concepts and describes how Platforms and Publishers engage with the Paket API.

**So, what is it?**

Paket is a universal engagement platform for Connected TVs and subscription marketplaces. We facilitate, through our suite of APIs, device and platform-agnostic features that help drive engagement through personalization, enhanced discovery, and advanced analytics.

The Paket API is organized around [REST](https://en.wikipedia.org/wiki/REST). Our API has predictable resource-oriented URLs, accepts JSON-encoded request bodies, returns JSON-encoded responses, and uses standard HTTP response codes, authentication, and verbs.

> Base URL

```
https://api.paket.tv/v1
```

There are two primary types of API consumers:

**Platforms**  
Platform partners are typically TV OEMs, platforms, operating systems, and marketplaces.

**Publishers**  
Publisher partners are typically publishers of streaming media apps.

How a partner engages with the API depends largely on whether they are a Platform or a Publisher. This designation is assigned in the [Paket Developer Portal](https://developer.paket.tv) at the time of the Company's account setup.