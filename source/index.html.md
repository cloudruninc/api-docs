---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell: curl
  - python: Python

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

api = Cloudrun(token) 
```

```shell
# With shell, you can just pass the correct header with each request
curl https://api.cloudrun.co/v1/wrf
    -H "Authorization: Bearer $token"
```

> Make sure to set the `token` parameter with your API key.

Cloudrun uses API tokens to allow access to the API. 
You can reqest a new Cloudrun API token via e-mail at 
[accounts@cloudrun.co](mailto:accounts@cloudrun.co).

Cloudrun expects for the API token to be included in all API requests to the server 
in a header that looks like the following:

`Authorization: Bearer $token`

<aside class="notice">
<code>token</code> environement variable must be set to your personal API token.
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
run = api.create_run(model='wrf',version='3.9')
```

```shell
curl -X POST https://api.cloudrun.co/v1/wrf \
     -H "Authorization: Bearer $token" \
     -F "version=3.9"
```

This endpoint creates a new WRF run.

<aside class="warning">
This endpoint is currently not open to users.
<a href='mailto:accounts@cloudrun.co'>E-mail us</a> 
if you would like to get access to developer endpoints.
</aside>

### HTTP Request

`POST https://api.cloudrun.co/v1/wrf`

### Form parameters

Parameter | Description
--------- | -----------
version   | WRF version to use

<aside class="notice">
Only WRF versions 3.8.1 and 3.9 are currently supported.
</aside>

## Upload input file

```python
from cloudrun import Cloudrun

api = Cloudrun(token)
run = api.get_run(id)
run.upload(file=path_to_file)
```

```shell
curl -X POST https://api.cloudrun.co/v1/wrf/${id}/upload \
     -H "Authorization: Bearer $token" \
     -F "file=@$path_to_file"
```

> `path_to_file` parameter must point to a valid input file, e.g. `wrfinput_d01`.

This endpoint lets you upload an input file to your run.

<aside class="warning">
This endpoint is currently not open to users.
<a href='mailto:accounts@cloudrun.co'>E-mail us</a> 
if you would like to get access to developer endpoints.
</aside>

### HTTP Request

`POST https://api.cloudrun.co/v1/wrf/{id}/upload`

### Form parameters

Parameter | Description
--------- | -----------
file      | Path to a valid input file

## Upload input file by URL

```python
from cloudrun import Cloudrun

api = Cloudrun(token)
run = api.get_run(id)
run.upload(url=url_to_file)
```

```shell
curl -X POST https://api.cloudrun.co/v1/wrf/${id}/upload_url \
     -H "Authorization: Bearer $token" \
     -F "url=$url_to_file"
```

> `url_to_file` parameter must point to a valid input file
on the web, e.g. `wrfinput_d01`. This could be a file
hosted from an http or ftp server, or an Amazon S3 bucket.

This endpoint lets you upload an input file from a remote URL to your run.

<aside class="warning">
This endpoint is currently not open to users.
<a href='mailto:accounts@cloudrun.co'>E-mail us</a> 
if you would like to get access to developer endpoints.
</aside>

### HTTP Request

`POST https://api.cloudrun.co/v1/wrf/{id}/upload_url`

### Form parameters

Parameter | Description
--------- | -----------
url       | URL to a remote input file

## Setup your WRF run

```python
from cloudrun import Cloudrun

api = Cloudrun(token)
run = api.get_run(id)
run.setup()
```

```shell
curl -X POST https://api.cloudrun.co/v1/wrf/${id}/setup \
     -H "Authorization: Bearer $token"
```

Use this endpoint to receive run-time and price options from Cloudrun.

<aside class="notice">
If setting up a WRF run, this endpoint can be used only after
uploading the `namelist.input` file via the `upload` endpoint.
</aside>

<aside class="warning">
This endpoint is currently not open to users.
<a href='mailto:accounts@cloudrun.co'>E-mail us</a> 
if you would like to get access to developer endpoints.
</aside>

### HTTP Request

`POST https://api.cloudrun.co/v1/wrf/{id}/setup`

## Start your WRF run

```python
from cloudrun import Cloudrun

api = Cloudrun(token)
run = api.get_run(id)
run.start(cores=32)
```

```shell
curl -X POST https://api.cloudrun.co/v1/wrf/${id}/start \
     -H "Authorization: Bearer $token" \
     -F "cores=32"
```

Use this endpoint to start your run. `cores` must be one of 
valid options from the response of `setup` endpoint.

<aside class="notice">
This endpoint can be used only after calling the `setup` endpoint.
</aside>

<aside class="warning">
This endpoint is currently not open to users.
<a href='mailto:accounts@cloudrun.co'>E-mail us</a> 
if you would like to get access to developer endpoints.
</aside>

### HTTP Request

`POST https://api.cloudrun.co/v1/wrf/{id}/start`

### Form parameters

Parameter | Description
--------- | -----------
cores     | Number of compute cores to use

## Stop your WRF run

```python
from cloudrun import Cloudrun

api = Cloudrun(token)
run = api.get_run(id)
run.stop()
```

```shell
curl -X POST https://api.cloudrun.co/v1/wrf/${id}/stop \
     -H "Authorization: Bearer $token"
```

Use this endpoint to stop your run.
This should be necessary only if you want to stop your run
before it completes.

<aside class="notice">
This endpoint can be used only after calling the `start` endpoint.
</aside>

<aside class="warning">
This endpoint is currently not open to users.
<a href='mailto:accounts@cloudrun.co'>E-mail us</a> 
if you would like to get access to developer endpoints.
</aside>

### HTTP Request

`POST https://api.cloudrun.co/v1/wrf/{id}/stop`

## Delete your WRF run

```python
from cloudrun import Cloudrun

api = Cloudrun(token)
run = api.get_run(id)
run.delete()
```

```shell
curl -X DELETE https://api.cloudrun.co/v1/wrf/${id} \
     -H "Authorization: Bearer $token"
```

Use this endpoint to delete the output files from your run.
You will still be able to access the metadata of your run.

<aside class="warning">
This request cannot be undone. 
</aside>

<aside class="warning">
This endpoint is currently not open to users.
<a href='mailto:accounts@cloudrun.co'>E-mail us</a> 
if you would like to get access to developer endpoints.
</aside>

### HTTP Request

`DELETE https://api.cloudrun.co/v1/wrf/{id}`
