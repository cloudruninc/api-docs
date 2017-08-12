---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python

toc_footers:
  - <a href='mailto:accounts@cloudrun.co'>Sign Up for a Developer Key</a>

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

> To authenticate, use this code:

```python
from cloudrun import Cloudrun

api = Cloudrun(token = os.environ['CLOUDRUN_TOKEN'])
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: Bearer $CLOUDRUN_TOKEN"
```

> Make sure to set `CLOUDRUN_TOKEN` environment variable with your API key.

Cloudrun uses API tokens to allow access to the API. 
You can reqest a new Cloudrun API token via e-mail at 
[accounts@cloudrun.co](mailto:accounts@cloudrun.co).

Cloudrun expects for the API token to be included in all API requests to the server 
in a header that looks like the following:

`Authorization: Bearer $CLOUDRUN_API_TOKEN`

<aside class="notice">
<code>cloudrun_api_token</code> environement variable must be set to your personal API token.
</aside>

# WRF

## Get all WRF runs

```python
from cloudrun import Cloudrun

api = Cloudrun(token)
runs = api.get_all_runs()
```

```shell
curl https://api.cloudrun.co/v1/wrf
    -H "Authorization: Bearer $token"
```

> `token` parameter must be set to your personal API token. 

This endpoint retrieves all WRF runs that you own or have access to.

### HTTP Request

`GET https://api.cloudrun.co/v1/wrf`

## Get a specific WRF run

```python
from cloudrun import Cloudrun

api = Cloudrun(token)
run = api.get_run(id)
```

```shell
curl "https://api.cloudrun.co/v1/wrf/$id"
  -H "Authorization: Bearer $token"
```

This endpoint retrieves a specific WRF run that you own or have access to.

### URL parameters

Parameter | Description
--------- | -----------
id        | The id of the WRF run to retrieve

## Create a new run

```python
from cloudrun import Cloudrun

api = Cloudrun(token)
run = api.create_run(model = 'wrf', version = '3.9')
```

```shell
curl -X POST https://api.cloudrun.co/v1/wrf \
     -H "Authorization: Bearer $token" \
     -F "version=3.9"
```

This endpoint creates a new WRF run.

### HTTP Request

`POST https://api.cloudrun.co/v1/wrf`

### Form parameters

Parameter | Description
--------- | -----------
version   | WRF version to use

<aside class="notice">
Only WRF versions 3.8.1 and 3.9 are currently supported.
</aside>

## Upload input files

### HTTP Request

`POST https://api.cloudrun.co/v1/wrf/{id}/upload`

## Setup your WRF run

### HTTP Request

`POST https://api.cloudrun.co/v1/wrf/{id}/setup`

## Start your WRF run

### HTTP Request

`POST https://api.cloudrun.co/v1/wrf/{id}/start`

## Stop your WRF run

### HTTP Request

`POST https://api.cloudrun.co/v1/wrf/{id}/stop`

## Delete your WRF run

### HTTP Request

`DELETE https://api.cloudrun.co/v1/wrf/{id}
