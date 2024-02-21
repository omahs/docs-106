---
description: >-
  `GET` request to `/identities/search` to search for Identities based on
  specific criteria.
---

# Search Identities

`GET` to `/identities/search`

Searches all Identities and returns a paginated list of Identities that meet the search criteria.

```bash
GET https://dev.intuition-api.com/identities/search?<Query_Parameters>
```

{% tabs %}
{% tab title="Fetch" %}
```typescript
  await fetch(`${apiUrl}/identities/search?${queryParams}`, {
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
curl "https://dev.intuition-api.com/identities/search?<Query_Parameters>" \
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

<table><thead><tr><th width="241">Parameter</th><th>Description</th></tr></thead><tbody><tr><td><code>page</code></td><td><br>Current page in sequence</td></tr><tr><td><code>limit</code></td><td><code>number</code><br><br>Max number of items returned on single page<br>Example: 1</td></tr><tr><td><code>sortBy</code></td><td><code>string</code> <br><br>Field to sort by. Default is <code>created_at</code><br>Example: "display_name"</td></tr><tr><td><code>direction</code></td><td><code>string</code> - "desc" or "asc"<br><br>Direction to sort.Default is "desc"<br>Example: "desc"</td></tr></tbody></table>

### Response
