---
description: '`GET` request to `/claim/:id` to fetch a Claim that matches the provided `id`.'
---

# Fetch a Claim

`GET` to `/claim/:id`

Retrieves a Claim that matches the provided `id`.

```bash
GET https://api.intuition.systems/claim/:id
```

{% tabs %}
{% tab title="Fetch" %}
<pre class="language-typescript"><code class="lang-typescript">   await fetch(`${apiUrl}/claim/${id}`, {
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
curl "https://api.intuition.systems/claim/<id>" \
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

<table><thead><tr><th width="309">Parameter</th><th>Description</th></tr></thead><tbody><tr><td><code>id</code></td><td><code>string</code> - Required<br><br>The <code>claim_id</code> for the Identity you want to fetch.<br>Example: <code>bxxxxxxx-6xxx-4xxx-8xxx-3xxxxxxxxxxx</code></td></tr></tbody></table>

### Response

If a Claim is found for the `id` you'll receive back a Claim object.

```json
{
 
},
```

Here is an example [Zod](https://zod.dev/) schema and TypeScript interface you can use for the Identity:

{% tabs %}
{% tab title="Zod" %}
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

<table><thead><tr><th width="314">Code</th><th>Description</th></tr></thead><tbody><tr><td>200</td><td>Claim object that matches the provided <code>id</code></td></tr><tr><td>404</td><td>No Claim found for the provided <code>id</code></td></tr></tbody></table>
