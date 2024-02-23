---
description: '`POST` request to `/identities` to create an Identity.'
---

# Create an Identity

`POST` to `/identity`

Creates an offchain Identity. Consider pairing this with [createAtom](../../contract-interactions/createatom.md) for onchain Identity creation. The Intuition Portal user journey for creating a Identity combines both of these interactions.

```bash
POST https://dev.intuition-api.com/identity
```

{% tabs %}
{% tab title="Fetch" %}
<pre class="language-typescript"><code class="lang-typescript"><strong>   await fetch(`${apiUrl}/identity`, {
</strong>      method: 'POST',
      "headers": {
<strong>        "Content-Type": "application/json",
</strong>        "x-api-key": &#x3C;API_KEY>
      },
      "body": {
         "display_name": "JP", // Required display name for Identity
         "creator": "0x...81", // Ethereum address for Identity creator
         "description": "It is JP.", // Optional description for identity
         "image": "https://my-image.com/me.jpg", // Optional image for Identity
         "external_reference": "https://github.com", // Optional link for Identity
      }
    })
</code></pre>
{% endtab %}

{% tab title="Curl" %}
```bash
curl -X POST "https://dev.intuition-api.com/identity" \
     -H "Content-Type: application/json" \
     -H "x-api-key: <API_KEY>" \
     -d '{
           "display_name": "JP",
           "creator": "0x...81",
           "description": "It is JP.",
           "image": "https://my-image.com/me.jpg",
           "external_reference": "https://github.com",
         }'
```
{% endtab %}
{% endtabs %}

#### Security <a href="#security" id="security"></a>

This request requires a valid API Key to be included with each request.

#### Headers <a href="#headers" id="headers"></a>

* `Content-Type: application/json`
* `x-api-key: <API_Key>`

#### Body Parameters <a href="#body-parameters" id="body-parameters"></a>

<table data-header-hidden><thead><tr><th width="307"></th><th></th></tr></thead><tbody><tr><td>Parameter</td><td>Description</td></tr><tr><td><code>display_name</code></td><td><code>string</code> - Required <br><br>The <code>display_name</code> for the Identity.<br>Example: <code>MyIdentity</code></td></tr><tr><td><code>creator</code></td><td><code>string</code> - Required <br><br>The Ethereum address of the Identity creator. <br>Example: <code>0x...81</code></td></tr><tr><td><code>description</code></td><td><code>string</code> - Optional <br><br>The <code>description</code> for the Identity.<br>Example: <code>This is my Identity.</code></td></tr><tr><td><code>image_url</code></td><td><code>string</code> - Required <br><br>The <code>image_url</code> for the Identity. This should be a valid URL.<br>Example: <code>https://my-image.com/me.jpg</code></td></tr><tr><td><code>external_reference</code></td><td><code>string</code> - Optional<br><br>An <code>external_reference</code> link for the Identity. This should be a valid URL.<br>Example: <code>https://github.com</code></td></tr></tbody></table>

### Response <a href="#response" id="response"></a>

If an Identity is successfully created you'll receive the Identity object for the new Identity.

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
}
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
```
{% endtab %}
{% endtabs %}

#### Response Codes <a href="#response" id="response"></a>

<table><thead><tr><th width="314">Code</th><th>Description</th></tr></thead><tbody><tr><td>200</td><td><code>OK</code><br><br>Identity successfully created. Response data.</td></tr><tr><td>400</td><td><code>Bad Request</code><br><br>Example: Improperly formatted JSON, such as including a trailing comma.</td></tr><tr><td>404</td><td></td></tr></tbody></table>
