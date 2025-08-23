# Introduction

Welcome to Paket! This documentation explains key concepts and describes how Platforms and Publishers engage with the Paket Watch APIs.

**So, what is it?**

Paket is a universal engagement platform that gives connected TV platforms, operators, and telcos access to:

- **Scalable subscription and bundling orchestration** – streamline third-party subscription management across services using platform's native payments and identity services.
- **AI-powered streaming television** – deliver personalized programming and recommendations that adapt to each user.
- **Cross-device continue watching & Up Next management** – unify playback progress and next-up recommendations across multiple apps and devices.

Our suite of APIs is device- and platform-agnostic, designed to increase engagement through personalization, improve discovery, and provide actionable analytics.

The Paket API is organized around [REST](https://en.wikipedia.org/wiki/REST). Our API has predictable resource-oriented URLs, accepts JSON-encoded request bodies, returns JSON-encoded responses, and uses standard HTTP response codes, authentication, and verbs.

> Base URL

```
https://api.paket.tv/v1
```

There are two primary types of API consumers:

**Platforms**  
Platform partners are typically TV OEMs, platforms, operating systems, and marketplaces.

**Publishers**  
Publisher partners are typically publishers of streaming media or other subscription-based apps.

How a partner engages with the API depends largely on whether they are a Platform or a Publisher. This designation is assigned in the [Paket Developer Portal](https://developer.paket.tv) at the time of the Company's account setup.