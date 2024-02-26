---
description: >-
  `GET` request to `/identity/:id/positions` to fetch Identity Positions for an
  Identity that matches the provided `identity_id`.
---

# Fetch Positions on an Identity

{% hint style="info" %}
As we're in our Beta Testnet we're aware of potential inconsistencies with the returned response data. We encourage you to build proofs of concept that utilize this request but be mindful that the response data may be different than expected.
{% endhint %}

`GET` to `/identity/:id/positions`

Retrieves Positions on an Identity that matches the provided `id`.

```bash
GET https://api.intuition.systems/identity/:id
```

{% tabs %}
{% tab title="Fetch" %}
<pre class="language-typescript"><code class="lang-typescript">   await fetch(`${apiUrl}/identity/${id}/positions`, {
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
curl "https://api.intuition.systems/identity/<id>/positions" \
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

<table><thead><tr><th width="309">Parameter</th><th>Description</th></tr></thead><tbody><tr><td><code>id</code></td><td><code>string</code> - Required<br><br>The <code>identity_id</code> for the Identity's Positions you want to fetch.<br>Example: <code>did:intuition:ipfs:Qxxx</code></td></tr></tbody></table>

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

If Positions are found for the Identity's `id` you'll receive back an Identity Positions object.

```json
{
  "id": "00000000-0000-0000-0000-000000000000",
  "user": {
    "wallet": "0x..." // wallet for user who staked
  },
  "direction": "for", // Identities can only be staked 'for'
  "vault_id": "1",
  "amount_staked": "1000000000000000",
  "conviction": "318743917544004046",
  "assets": "2000000000000000",
  "fee": "1000000000000000",
  "delta_assets": "0",
  "total": 1 // can be ignored -- we're aware of this. it's a duplicate of the pagination response
}
```

Here is an example [Zod](https://zod.dev/) schema and TypeScript interface you can use for the Identity Positions:

{% tabs %}
{% tab title="Zod" %}
```typescript
export const IdentityPositionSchema = z.object({
  id: z.string(),
  user: EmbedUserUserAggregatesSchema,
  updated_at: z.string().transform((date) => {
    const d = new Date(Number.parseInt(date))
    return d.toISOString()
  }), // unix timestamp
  assets: z.string(), // bigint
  conviction: z.string(), // bigint
  fee: z.string(), // TBD - bigint
  delta_assets: z.string(), // assets gain/loss
```
{% endtab %}

{% tab title="Interface" %}
```typescript
// if using Zod, you can directly infer:
 
export type IdentityPosition = z.infer<typeof IdentityPositionSchema>

// used in another interface:

interface IdentityPositionResponse {
  data: IdentityPosition[]
}

// if not using Zod, here's an example of the Identity Position type:

type IdentityPosition = {
    assets: string;
    id: string;
    user: {
        wallet: string;
        id: string;
        did?: string | undefined;
        display_name?: string | undefined;
        image?: string | undefined;
    };
    conviction: string;
    updated_at: string;
    fee: string;
    delta_assets: string;
}

```
{% endtab %}
{% endtabs %}

#### Response Codes

<table><thead><tr><th width="314">Code</th><th>Description</th></tr></thead><tbody><tr><td>200</td><td>Identity Positions object for the provided <code>id</code></td></tr><tr><td>404</td><td>No Positions found for the provided Identity <code>id</code></td></tr></tbody></table>
