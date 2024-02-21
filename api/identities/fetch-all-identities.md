---
description: '`GET` request to `/identities` to retrieve all Identities in a paginated list.'
---

# Fetch all Identities

`GET` to `/identities`

Retrieves a list of Identities. Returns a _paginated list_ of Identities.

```bash
GET https://dev.intuition-api.com/identities?<Query_Parameters>
```

{% tabs %}
{% tab title="Fetch" %}
```typescript
   await fetch(`${apiUrl}/identities?${queryParams}`, {
      method: 'GET',
      "headers": {
        "Content-Type": "application/json",
        "x-api-key": <API_KEY>
      },
    })
```
{% endtab %}

{% tab title="Curl" %}
```bash
curl "https://dev.intuition-api.com/identities?<Your_Query_Parameters>" \
     -H "Content-Type: application/json" \
     -H "x-api-key: <Your_API_Key>" \
     -X GET
```

Replace the `<Your_API_Key>` with your valid API Key.

Replace the `<Your_Query_Parameters>` with any _query parameters_ you want to include, such as `displayName`.
{% endtab %}

{% tab title="Remix" %}

{% endtab %}
{% endtabs %}

### Security

This request requires a valid API Key to be included with each request.

### Headers

* `Content-Type: application/json`
* `x-api-key: <API_Key>`

### Query Parameters

| Parameter   | Description                                         |
| ----------- | --------------------------------------------------- |
| `page`      | Filter by `displayName`                             |
| `limit`     | Filter by `creator` Ethereum address                |
| `sortBy`    | Filter by `status`, such as `pending` or `complete` |
| `direction` |                                                     |

### Responses

**200 OK**: Successful retrieval of identities.&#x20;

This includes the [Paginated Response](../api-information.md#pagination)&#x20;

```json
{
    "page": 1, // The current page in pagination sequence
    "limit": 100, // The max number of items returned on a single page
    "total": 50, // Total number of items 
    "data": [{}] // Array of response items
}
```

You'll receive back a `data` array that is a paginated list of all available Identities.

<pre class="language-json"><code class="lang-json">{
  "page": 1,
  "limit": 100,
  "total": 79,
  "data": [
<strong>      {
</strong>            "id": "f9xxxxxx...",
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
  ],
}
</code></pre>

Here is an example [Zod](https://zod.dev/) schema and TypeScript interface you can use for the Identity:

{% tabs %}
{% tab title="Zod Schema" %}
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

