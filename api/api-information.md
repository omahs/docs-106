---
description: Overview of the Intuition public API.
---

# API Information

The Intuition public API is a REST API providing interaction points for building on Intuition. Our Portal app combines API interactions provided in this section with client-side contract interactions with our protocol. We're in the process of creating and exposing more resources to builders, so keep checking back for regular updates!

### API Overview

Our public API is a REST API that supports `GET` and `POST` requests for offchain interactions. Our API supports frameworks that can send HTTP requests, as well as requests sent through Postman or `curl`.  The documentation for each request includes examples for both approaches.

Public API URL: `https://api.intuition.systems`

#### Content Type

API requests need to be encoded as JSON and include the `Content-Type: application/json` header. This is included in the examples for each request, but if you're encountering unexpected issues check that you're encoding as JSON.&#x20;

#### Authentication

Our public API supports authentication through API Key. You'll need to include this with the `x-api-key` header. The individual requests include example headers.&#x20;

You can read more about authenticating with API Keys in the [API Key Authentication](api-key-authentication.md) page.

#### Getting Your API Key

We've created a developer API Key Portal for generating your API Key. Our [Dev Quick Start](../getting-started/dev-quick-start.md) includes a walkthrough of this process if you haven't generated your API Key yet. You can always view your current API Key by navigating to the [API Key Portal](https://intuition.sh) and signing a message. Your API Key is connected to your Ethereum wallet, so you can always retrieve your current API Key by following this same process.

<figure><img src="../.gitbook/assets/api-key-5.jpg" alt=""><figcaption><p>Viewing your API Key on the API Key Portal after connecting your Ethereum wallet and signing a message.</p></figcaption></figure>

#### Missing Authentication

If you don't include your API Key in the `x-api-key` header the API returns, a `401 Unauthorized` response with the following information:

```json
{
    "code": 401,
    "error": "`Authorization` header  or Api Key is missing"
}
```

If you're seeing this you're likely missing the `x-api-key` header in your request.

#### Invalid API Key

If you include an invalid API Key in the `x-api-key` header, the API returns a `401 Unauthorized` response with the following information:

```json
{
    "code": 401,
    "error": "`x-api-key` is invalid"
}
```

If you're seeing this be sure to check that your API Key is valid. If you need to check or request a new API Key you can visit the [API Key Portal](https://intuition.sh).

### Pagination

For successful API responses that include an array of items (such as [Fetching all Identities](identities/fetch-all-identities.md)) the API includes pagination.&#x20;

This is the response object structure on any successful request that includes pagination:

```json
{
    "page": 1, // The current page in pagination sequence
    "limit": 100, // The max number of items returned on a single page
    "total": 50, // Total number of items 
    "data": [{}] // Array of response items
}
```

### Getting Started

Check out our documentation for each of our API requests.

<table data-view="cards"><thead><tr><th></th><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td>API Key Authentication</td><td></td><td></td><td><a href="api-key-authentication.md">api-key-authentication.md</a></td></tr><tr><td>API: Identities</td><td></td><td></td><td><a href="identities/">identities</a></td></tr><tr><td>API: Claims</td><td></td><td></td><td><a href="claim/">claim</a></td></tr></tbody></table>

