# Data

Instantly deploy APIs for small data. Create schema-defined APIs on demand, backed by SQLite. Perfect for prototypes and small data. An HTML interface for the API is running at [https://data.foundry.sh/](https://data.foundry.sh/)

# API

Data endpoints support both HTTP and HTTPS access, although HTTPS is encouraged. All data is sent and received as JSON. All timestamps stored in UTC and return in ISO 8601 format:

```
YYYY-MM-DDTHH:MM:SSZ
```

### Authentication

Key is passed in the `Authorization` header. Each key corresponds to a HTTP verb. This allows endpoints to be deployed as read-only, write-only, or a combination of both. The admin key has full permissions on all methods, including two admin methods for metadata and database dump. Do not share this key publicly.

### CORS

Endpoints are designed to be used from the web, and respond with `Access-Control-Allow-Origin: *`

### Default Values

When no value is passed to a field of type date or datetime, `datetime('now')` is inserted.

### Rate Limiting

Writes \(POST/PATCH/DELETE\) are rate limited to 5 requests per second per IP address. Reads \(GET\) are limited to 30 requests per second per IP address. For remainaing requests, see the `X-RateLimit-Remaining-second` header.

API deployment is limited to 2 deploys per minute.

## Methods

**Get All**

List all objects of :model. Pagination and sorting is supported with query parameters.

```
GET /:model
```

Query Parameters:

| Name | Type | Default |
| :--- | :--- | :--- |
| limit | integer | 10 |
| offset | integer | 0 |
| sort | string |  |
| order | string "ASC" or "DESC" | "ASC" |

**Get One**

Get an object of :model by :id. Multiple objects can be retrieved by passing comma separated ids.

```
GET /:model/:id
```

```
GET /:model/:id,:id,:id
```

**Create**

Create an object of :model. POST body is a JSON document matching the model schema. Returns JSON with the id of the new object.

```
POST /:model
```

**Update**

Update an object of :model by :id. PATCH body is a partial JSON document matching the model schema.

```
PATCH /:model/:id
```

**Delete**

Delete an object of :model by :id.

```
DELETE /:model/:id
```

### Admin Methods

**Metadata &Keys**

Get a list of model schemas and endpoint keys.

```
GET /
```

**Database Dump**

Download full SQLite database.

```
GET /sqlite.db
```

### API Methods

Create a new API. POST a JSON object of model schemas to /. Supported field types are:

```
'boolean', 'integer', 'float', 'text', 'date', 'datetime'
```

```
POST https://data.foundry.sh/
{
  "<model>": {
    "<field>": "<type>"
  }
}
```

Returns the new API endpoint and authorization keys for each method.

```
HTTP/1.1 201 Created
Location: https://data.foundry.sh/<id>/
{
    "authorization": {
        "admin": "tgeyzqryuuoqwppwaoafxxiyvfszileq",
        "delete": "wjatkpzagpjqkmmuqrqgtqngmslbnwbd",
        "get": "hwulumjzorciqhknhqahzjkakamiyqfh",
        "getone": "rsatmnbffumpcfaumohkmclnrdpivgjo",
        "patch": "yptquwnnzzfdicaxotphxxgyerarieya",
        "post": "jppvzjvxracamqzevzfnltyhfztsodbk"
    },
    "endpoint": "https://data.foundry.sh/<id>/"
}
```



