---
description: '`GET` request to `/identities` to retrieve all Identities in a paginated list.'
---

# Fetch all Identities

### Overview

`GET` to `/identities`

Fetches a list of Identities. Returns a _paginated list_ of Identities.

```bash
GET https://dev.intuition-api.com/identities?<Query_Parameters>
```

#### Security

This request requires a valid API Key to be included with each request.

#### Headers

* `Content-Type: application/json`
* `x-api-key: <Your_API_Key>`

#### Query Parameters

| Parameter     | Purpose                                             |
| ------------- | --------------------------------------------------- |
| `displayName` | Filter by `displayName`                             |
| `creator`     | Filter by `creator` Ethereum address                |
| `status`      | Filter by `status`, such as `pending` or `complete` |
| `paging`      |                                                     |
| `sort`        |                                                     |

{% swagger src="../../.gitbook/assets/swagger.json" path="/identities" method="get" %}
[swagger.json](../../.gitbook/assets/swagger.json)
{% endswagger %}

#### Responses

**200 OK**: Successful retrieval of identities. The response includes:

* `data`: An array of identities matching the query.
* `limit`: The limit on the number of identities returned.
* `page`: The current page number in the pagination sequence.
* `total`: The total number of identities available.

#### Successful Response

You'll receive back a `data` object that is a paginated list of all available Identities.

```typescript
{
  "data": [
    {
      "corpora_id": "string",
      "created_at": "2024-02-21T16:38:23.165Z",
      "creator": {
        "api_key": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "created_at": "2024-02-21T16:38:23.165Z",
        "did": "string",
        "display_name": "string",
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "image": "string",
        "last_login": "2024-02-21T16:38:23.165Z",
        "role": "User",
        "updated_at": "2024-02-21T16:38:23.165Z",
        "wallet": "string"
      },
      "description": "string",
      "display_name": "string",
      "external_reference": "string",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "identity_hash": "string",
      "identity_id": "string",
      "image": "string",
      "num_positions": 0,
      "semantic": "string",
      "status": "pending",
      "updated_at": "2024-02-21T16:38:23.165Z"
    }
  ],
  "limit": 0,
  "page": 0,
  "total": 0
}
```

### Examples

{% tabs %}
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

{% tab title="TypeScript" %}
```typescript
```
{% endtab %}

{% tab title="Remix" %}

{% endtab %}
{% endtabs %}
