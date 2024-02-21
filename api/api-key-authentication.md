---
description: Overview for obtaining and using an Intuition API Key.
---

# API Key Authentication

Your Intuition API Key serves as the cornerstone of authentication when interacting with the Intuition API. It plays a crucial role in securing and authorizing your requests, ensuring that they are recognized and permitted. This is required to use our public API.

### Getting Your API Key

Since your API Key is connected to your Ethereum wallet address, we've created an API Key Portal where you can obtain and view your API Key.

The [Dev Quick Start](../getting-started/dev-quick-start.md) page contains a walkthrough of this process. You can also go directly to the API Key Portal and sign the message.&#x20;

{% hint style="warning" %}
#### Remember, your API Key is tied to your Ethereum Address that you signed with in the API Key Portal.  Be sure to keep it safe!
{% endhint %}

### Authenticating With Your API Key

Once you've obtained your Intuition API Key you can then begin interacting with the Intuition API. You'll need to include your valid API Key as a `header` in every API request.

Let's look at key points for authenticating with your API Key.

#### Including Your API Key in Requests

You'll want to pass in your API Key in the `x-api-key` header, such as:

`x-api-key: YOUR_API_KEY`

Here's how you can use your API Key in a cURL request to the Intuition API, specifically when [fetching Identities](identities/fetch-all-identities.md):thumbsup:

{% tabs %}
{% tab title="Curl" %}
```bash
curl "https://dev.intuition-api.com/identities?<Your_Query_Parameters>" \
     -H "Content-Type: application/json" \
     -H "x-api-key: <Your_API_Key>" \
     -X GET
```

Replace the `<Your_API_Key>` with your valid API Key that you obtained from the API Key Portal.
{% endtab %}
{% endtabs %}

### Using Environment Variables

Be sure to follow best practices for protecting your API Key. One way to ensure safety is to use _environment variables_ to prevent it from being leaked client-side on your requests. Be sure to also include this in any `.gitignore` to prevent your `.env` from being included in version control. It's _highly recommended_ to follow this best practice and to avoid hardcoding your API Key value into your codebase.

{% hint style="info" %}
If you're using Remix, Next.js, or any other frontend framework that supports using `.env` we recommend that you include your API Key and access it via **environment variables** to prevent it from any unintentional client-side leaking.
{% endhint %}

Here is a general example of how you could include your `API_KEY` in a `.env` file:

```bash
// .env

API_KEY=your_actual_api_key_here
```

This is a generalized example of how you can then reference your `API_KEY` via environment variables. Each framework will handle this slightly differently, but this high-level approach can be used as a reference:

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
