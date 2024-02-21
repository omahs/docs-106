---
description: '`GET` request to `/claims` to retrieve all Claims in a paginated list.'
---

# Fetch all Claims

`GET` to `/claims`

Retrieves a list of Claims. Returns a _paginated list_ of Claims.

```bash
GET https://dev.intuition-api.com/claims?<Query_Parameters>
```

{% tabs %}
{% tab title="Fetch" %}
```typescript
   await fetch(`${apiUrl}/claims?${queryParams}`, {
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
curl "https://dev.intuition-api.com/claims?<Query_Parameters>" \
     -H "Content-Type: application/json" \
     -H "x-api-key: <API_Key>" \
     -X GET
```

Replace the `<API_Key>` with your valid API Key.

Replace the `<Query_Parameters>` with _query parameters_ you want to include.
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

<table><thead><tr><th width="241">Parameter</th><th>Description</th></tr></thead><tbody><tr><td><code>page</code></td><td><br>Current page in sequence</td></tr><tr><td><code>limit</code></td><td><code>number</code><br><br>Max number of items returned on single page<br>Example: 1</td></tr><tr><td><code>sortBy</code></td><td><code>string</code> <br><br>Field to sort by. Default is <code>created_at</code><br>Example: "creator"</td></tr><tr><td><code>direction</code></td><td><code>string</code> - "desc" or "asc"<br><br>Direction to sort. Default is <code>desc</code><br>Example: "desc"</td></tr></tbody></table>

### Response

This includes the [Paginated Response](../api-information.md#pagination)&#x20;

```json
{
    "page": 1, // The current page in pagination sequence
    "limit": 100, // The max number of items returned on a single page
    "total": 50, // Total number of items 
    "data": [{}] // Array of response items
}
```

You'll receive back a `data` array that is a paginated list of all available Claims.

```json
{
  "page": 1,
  "limit": 100,
  "total": 79,
  "data": [{}],
  ]
}
```

Here is an example [Zod](https://zod.dev/) schema and TypeScript interface you can use for the Claim:

{% tabs %}
{% tab title="Zod Schema" %}
```typescript
export const ClaimSchema = z.object({
  claim_id: z.string(),
  vault_id: z.string(),
  counter_vault_id: z.string(),
  created_at: z.string().transform((date) => {
    const d = new Date(Number.parseInt(date))
    return d.toISOString()
  }), // unix timestamp
  updated_at: z.string(), // unix timestamp (last claim position update)
  creator: EmbedUserUserAggregatesSchema.optional(),
  subject: IdentitySchema,
  predicate: IdentitySchema,
  object: IdentitySchema,
  status: z.enum(['pending', 'complete']),
  for_num_positions: z.number(),
  for_assets_sum: z.string(), // bigint
  for_conviction_sum: z.string(), // bigint
  for_conviction_price: z.string(), // bigint
  against_num_positions: z.number(),
  against_assets_sum: z.string(), // bigint
  against_conviction_sum: z.string(), // bigint
  against_conviction_price: z.string(), // bigint
})
```
{% endtab %}

{% tab title="Interface" %}
```typescript
 // if using Zod, you can directly infer:
 
export type Claim = z.infer<typeof ClaimSchema>

// used in another interface:

interface OffChainClaimFetcherData {
  success: 'success' | 'error'
  claim: Claim
}

// if not using Zod, here's an example of the Claim type:

type Claim = {
        claim_id: string;
        vault_id: string;
        created_at: string;
        updated_at: string;
    };
}

```
{% endtab %}
{% endtabs %}

#### Response Codes

<table><thead><tr><th width="260">Code</th><th>Description</th></tr></thead><tbody><tr><td>200</td><td>Paginated list of Claims</td></tr><tr><td>400</td><td>Bad Request. <br><br>Example: Unsupported value for <code>sortBy</code> query parameter.</td></tr><tr><td></td><td></td></tr></tbody></table>
