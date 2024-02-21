---
description: '`GET` request to `/claims` to retrieve all Claims in a paginated list.'
---

# Fetch all Claims

### Overview

`GET` to `/claims`

This endpoint fetches all Claims based on the provided _query parameters_. Returns a _paginated list_ of Claims.

```bash
GET https://dev.intuition-api.com/claims
```

#### Security

This request requires a valid API Key to be included with each request. Include your valid API Key as the value for the `x-api-key` header.

#### Headers

* `Content-Type: application/json`
* `x-api-key: <Your_API_Key>`

{% swagger src="../../.gitbook/assets/swagger.json" path="/claims" method="get" %}
[swagger.json](../../.gitbook/assets/swagger.json)
{% endswagger %}

### Examples

{% tabs %}
{% tab title="Curl" %}
```bash
curl "https://dev.intuition-api.com/claims" \
     -H "Content-Type: application/json" \
     -H "x-api-key: <Your_API_Key>" \
     -X GET
```

Replace the `<Your_API_Key>` with your valid API Key.
{% endtab %}

{% tab title="TypeScript" %}

{% endtab %}
{% endtabs %}
