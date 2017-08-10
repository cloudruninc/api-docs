---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Cloudrun API!

We have language bindings in for shell (using curl) and Python! 
You can view code examples in the dark area to the right, 
and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

> To authorize, use this code:

```python
import cloudrun

api = cloudrun.authorize('cloudrun_token')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: cloudrun_token"
```

> Make sure to replace `cloudrun_token` with your API key.

Cloudrun uses API keys to allow access to the API. You can register a new Cloudrun API key at our [developer portal](http://cloudrun.co/v1/developers).

Cloudrun expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: cloudrun_token`

<aside class="notice">
You must replace <code>cloudrun_token</code> with your personal API key.
</aside>

# WRF

## Get All WRF runs

```python
import cloudrun

api = cloudrun.authorize('cloudrun_token')
api.wrf.get()
```

```shell
curl "http://cloudrun.co/v1/api/wrf"
  -H "Authorization: cloudrun_token"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all wrf.

### HTTP Request

`GET http://cloudrun.co/v1/api/wrf`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include wrf that have already been adopted.

<aside class="success">
Remember — a happy WRF run is an authenticated WRF run!
</aside>

## Get a Specific WRF run

```python
import cloudrun

api = cloudrun.authorize('cloudrun_token')
api.wrf.get(2)
```

```shell
curl "http://cloudrun.co/v1/api/wrf/2"
  -H "Authorization: cloudrun_token"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific WRF run.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://cloudrun.co/v1/wrf/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the WRF run to retrieve

## Delete a Specific WRF run

```python
import cloudrun

api = cloudrun.authorize('cloudrun_token')
api.wrf.delete(2)
```

```shell
curl "http://cloudrun.co/v1/api/wrf/2"
  -X DELETE
  -H "Authorization: cloudrun_token"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint retrieves a specific WRF run.

### HTTP Request

`DELETE http://cloudrun.co/v1/wrf/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the WRF run to delete

