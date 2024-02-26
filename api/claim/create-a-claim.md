---
description: '`POST` request to `/claims` to create a Claim.'
---

# Create a Claim

`POST` to `/claims`

Creates an offchain Claim. Consider pairing this with [createTriple](../../contract-interactions/createtriple.md) for onchain Claim creation. The Intuition Portal user journey for creating a Claim combines both of these interactions.

```bash
POST https://api.intuition.systems/claims
```

{% tabs %}
{% tab title="Fetch" %}
<pre class="language-typescript"><code class="lang-typescript"><strong>   await fetch(`${apiUrl}/claims`, {
</strong>      method: 'POST',
      "headers": {
<strong>        "Content-Type": "application/json",
</strong>        "x-api-key": &#x3C;API_KEY>
      },
      "body": {
        "subject": "bxxxxxxx-6xxx-4xxx-8xxx-3xxxxxxxxxxx", // id of the subject Identity
        "predicate": "bxxxxxxx-6xxx-4xxx-8xxx-3xxxxxxxxxxx", // id of the predicate Identity
        "object": "bxxxxxxx-6xxx-4xxx-8xxx-3xxxxxxxxxxx", // id of the object Identity
        "creator": "0x...81", // Ethereum address of the Claim creator
      }
    })
</code></pre>
{% endtab %}

{% tab title="Curl" %}
```bash
curl -X POST "https://api.intuition.systems/claims" \
     -H "Content-Type: application/json" \
     -H "x-api-key: <API_KEY>" \
     -d '{
           "subject": "bxxxxxxx-6xxx-4xxx-8xxx-3xxxxxxxxxxx",
           "predicate": "bxxxxxxx-6xxx-4xxx-8xxx-3xxxxxxxxxxx",
           "object": "bxxxxxxx-6xxx-4xxx-8xxx-3xxxxxxxxxxx",
           "creator": "0x...81"
         }'
```
{% endtab %}
{% endtabs %}

### Security

This request requires a valid API Key to be included with each request.

### Headers

* `Content-Type: application/json`
* `x-api-key: <API_Key>`

### Body Parameters

<table><thead><tr><th width="309">Parameter</th><th>Description</th></tr></thead><tbody><tr><td><code>subject</code></td><td><code>string</code> - Required<br><br>The <code>id</code> for the Identity you're using as the subject.<br>Example: <code>bxxxxxxx-6xxx-4xxx-8xxx-3xxxxxxxxxxx</code></td></tr><tr><td><code>predicate</code></td><td><code>string</code> - Required<br><br>The <code>id</code> for the Identity you're using as the predicate.<br>Example: <code>bxxxxxxx-6xxx-4xxx-8xxx-3xxxxxxxxxxx</code></td></tr><tr><td><code>object</code></td><td><code>string</code> - Required<br><br>The <code>id</code> for the Identity you're using as the object.<br>Example: <code>bxxxxxxx-6xxx-4xxx-8xxx-3xxxxxxxxxxx</code></td></tr><tr><td><code>creator</code></td><td><code>string</code> - Required<br><br>The Ethereum address of the Claim creator.<br>Example: <code>0x...81</code></td></tr></tbody></table>

### Response

If a Claim is successfully created you'll receive the Claim object for the new Claim.

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

<table><thead><tr><th width="314">Code</th><th>Description</th></tr></thead><tbody><tr><td>200</td><td><code>OK</code><br><br>Claim successfully created. Response data.</td></tr><tr><td>400</td><td><code>Bad Request</code><br><br>Example: Improperly formatted JSON, such as including a trailing comma.</td></tr><tr><td>404</td><td></td></tr></tbody></table>
