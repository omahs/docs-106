---
description: Overview for obtaining and using an Intuition API Key.
---

# API Key Authentication

Your Intuition API Key serves as the cornerstone of authentication when interacting with the Intuition API. It plays a crucial role in securing and authorizing your requests, ensuring that they are recognized and permitted.

### Getting Your API Key

(To be added)

### Authenticating With Your API Key

Once you've obtained your Intuition API Key you can then begin interacting with the Intuition API. You'll need to include a valid API Key as a `header` in every API request.

Let's look at key points for authenticating with your API Key.

{% hint style="warning" %}
#### Your API Key is tied to your Ethereum Address that you signed with in the API Key Portal.  Be sure to keep it safe!
{% endhint %}

#### Including Your API Key in Requests

Here's how you can use your API Key in a cURL request to the Intuition API, specifically when [fetching Identities](identities/fetch-all-identities.md):thumbsup:

{% tabs %}
{% tab title="Curl" %}
```bash
curl "https://dev.intuition-api.com/identities?<Your_Query_Parameters>" \
     -H "Content-Type: application/json" \
     -H "x-api-key: <Your_API_Key>" \
     -X GET
```

Replace the `<Your_API_Key>` with your valid API Key.
{% endtab %}
{% endtabs %}

{% hint style="info" %}
If you're using Remix, Next.js, or any other frontend framework that supports using `.env` we recommend that you include your API Key and access it via **environment variables** to prevent it from any unintentional client-side leaking.
{% endhint %}

Here is a general example of how you could include your `API_KEY` in a `.env` file:

```bash
// .env

API_KEY=your_actual_api_key_here
```

Here's a generalized example of how you can then reference your `API_KEY` via environment variables. Each framework will handle this slightly differently, but this high-level approach can be used as a reference:

```typescript
// yourFile.ts

import 'dotenv/config'; // Load .env variables

// Access the API_KEY from .env
const apiKey = process.env.API_KEY;

if (!apiKey) {
  console.error('API key is undefined. Please check your .env file.');
} else {
  // Ready to use apiKey for your API call headers
  console.log('API Key loaded successfully:', apiKey);
}

```

Note that frameworks such as Remix don't require the `import 'dotenv/config';` , and that Vite uses `import.meta.env` instead. Consult the documentation for the framework that you're using:

* [Remix Environment Variables](https://remix.run/docs/en/main/guides/envvars)
* [Next.js Environment Variables](https://nextjs.org/docs/pages/building-your-application/configuring/environment-variables)
* [Vite Env Variables and Modes](https://vitejs.dev/guide/env-and-mode)
