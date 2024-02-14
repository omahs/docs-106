---
description: >-
  `GET` request to `/identities/search` to search for Identities based on
  specific criteria.
---

# Search Identities

### Overview

`GET` to `/identity/search`

This endpoint allows for searching Identities based on specific _query parameters_. It returns a _paginated list_ of Identities that match the query parameters.

```
GET https://dev.intuition-api.com/identity/search?<Query_Parameters>
```

#### Security

This request requires a valid API Key to be included with each request.

#### Headers

* `Content-Type: application/json`
* `x-api-key: <Your_API_Key>`

#### Query Parameters

(TBD)

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
curl "https://dev.intuition-api.com/identity?<Your_Query_Parameters>" \
     -H "Content-Type: application/json" \
     -H "x-api-key: <Your_API_Key>" \
     -X GET
```

Replace the `<Your_API_Key>` with your valid API Key.

Replace the `<Your_Query_Parameters>` with any _query parameters_ you want to include, such as `displayName`.
{% endtab %}
{% endtabs %}
