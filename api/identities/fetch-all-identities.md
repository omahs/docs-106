---
description: '`GET` request to `/identities` to retrieve all Identities in a paginated list.'
---

# Fetch all Identities

### Overview

`GET` to `/identities`

This endpoint fetches all Identities based on the provided _query parameters_. Returns a _paginated list_ of Identities.

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

#### Responses

**200 OK**: Successful retrieval of identities. The response includes:

* `data`: An array of identities matching the query.
* `limit`: The limit on the number of identities returned.
* `page`: The current page number in the pagination sequence.
* `total`: The total number of identities available.

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
