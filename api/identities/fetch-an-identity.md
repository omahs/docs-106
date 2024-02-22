---
description: >-
  `GET` request to `/identity/:id` to fetch an Identity that matches the
  provided `identity_id`.
---

# Fetch an Identity

`GET` to `/identity/:id`

Retrieves an Identity that matches the provided `id`.

```bash
GET https://dev.intuition-api.com/identity/:id
```

{% tabs %}
{% tab title="Fetch" %}
<pre class="language-typescript"><code class="lang-typescript">   await fetch(`${apiUrl}/identity/${id}`, {
      method: 'GET',
      "headers": {
<strong>        "Content-Type": "application/json",
</strong>        "x-api-key": &#x3C;API_KEY>
      },
    })
</code></pre>
{% endtab %}

{% tab title="Curl" %}
```bash
curl "https://dev.intuition-api.com/identity/<id>" \
     -H "Content-Type: application/json" \
     -H "x-api-key: <API_Key>" \
     -X GET
```
{% endtab %}
{% endtabs %}

### Security

This request requires a valid API Key to be included with each request.

### Headers

* `Content-Type: application/json`
* `x-api-key: <API_Key>`

### Path Parameters

<table><thead><tr><th width="309">Parameter</th><th>Description</th></tr></thead><tbody><tr><td><code>id</code></td><td><code>string</code> - Required<br><br>The <code>identity_id</code> for the Identity you want to fetch.<br>Example: <code>did:intuition:ipfs:Qxxx</code></td></tr></tbody></table>

### Response

If an Identity is found for the `id` you'll receive back an Identity object.

```json
{
  "id": "f9xxxxxx...",
  "identity_id": "did:intuition:ipfs:Xxx",
  "identity_hash": "x...",
  "display_name": "JP",
  "description": "It's me, JP",
  "image": "image_url",
  "creator": {
      "id": "7xxxxxxx...",
      "created_at": "2024-02-13T14:58:43.809055Z",
      "updated_at": "2024-02-13T14:58:43.809055Z",
      "last_login": "2024-02-21T18:47:26.556035Z",
      "api_key": "",
      "wallet": "0x...",
      "role": "User"
  },
  "status": "complete",
  "created_at": "2024-02-15T05:47:17.289011Z",
  "num_positions": 0,
  "updated_at": "2024-02-15T05:47:17.289011Z",
  "vault_id": "1",
  "assets_sum": "0",
  "conviction_sum": "0",
  "conviction_price": "0"
},
```

Here is an example [Zod](https://zod.dev/) schema and TypeScript interface you can use for the Identity:

{% tabs %}
{% tab title="Zod" %}
```typescript
export const IdentitySchema = z.object({
  identity_id: z.string(),
  vault_id: z.string(),
  display_name: z.string(),
  description: z.string().optional(),
  image: z.string().optional(),
  corpora_id: z.string().nullish(),
  semantic: z.string().nullish(),
  external_reference: z.string().nullish(),
  creator: EmbedUserUserAggregatesSchema.optional(),
  created_at: z.string(),
  num_positions: z.number(),
  assets_sum: z.string(), // bigint
  conviction_sum: z.string(), // bigint
  conviction_price: z.string(), // bigint
  updated_at: z.string(),
  status: z.string().optional(),
  isUser: z.boolean().optional(),
  user_conviction: z.string().optional(), // bigint
  user_conviction_value: z.string().optional(), // bigint
})
```
{% endtab %}

{% tab title="Interface" %}
```typescript
// if using Zod, you can directly infer:
 
export type Identity = z.infer<typeof IdentitySchema>

// used in another interface:

interface ClaimProps {
  subject: Identity | null
  predicate: Identity | null
  object: Identity | null
}

export const UserSchema = z.object({
  did: z.string().optional(),
  wallet: z.string(),
  display_name: z.string().optional(),
  image: z.string().optional(),
  id: z.string().uuid(),
})

// if not using Zod, here's an example of the Identity type:

type Identity = {
    vault_id: string;
    created_at: string;
    updated_at: string;
    display_name: string;
    identity_id: string;
    num_positions: number;
    assets_sum: string;
    conviction_sum: string;
    conviction_price: string;
    updated_at: string;
    num_positions: number;
    assets_sum: string;
    conviction_sum: string;
    conviction_price: string;
    updated_at: string;
    status?: string;
    isUser?: string;
    user_conviction?: string;
    user_conviction_value?: string;
}

```
{% endtab %}
{% endtabs %}

#### Response Codes

<table><thead><tr><th width="314">Code</th><th>Description</th></tr></thead><tbody><tr><td>200</td><td>Identity object that matches the provided <code>id</code></td></tr><tr><td>404</td><td>No identity found for the provided <code>id</code></td></tr></tbody></table>
