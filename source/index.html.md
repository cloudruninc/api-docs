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
# With curl, pass the authentication header with each request
curl https://api.cloudrun.co/v1/wrf
    -H "Authorization: Bearer $token"
```

> Make sure to set the `token` parameter to your API token.

Cloudrun uses API tokens to allow access to the API.
You can reqest a new Cloudrun API token via e-mail at
[accounts@cloudrun.co](mailto:accounts@cloudrun.co).

Cloudrun expects for the API token to be included in all API requests to the
server in a header that looks like this:

`Authorization: Bearer $token`

<aside class="notice">
<code>token</code> environment variable must be set to your personal API token.
</aside>

# WRF

## Introduction

TODO: write intro here

## WRF response object

```json
{
  "compute_options": [
    {
      "cores": 1,
      "cost": 1.0,
      "time": 12203
    },
    {
      "cores": 2,
      "cost": 1.14,
      "time": 6560
    },
    {
      "cores": 4,
      "cost": 1.9,
      "time": 3738
    },
    {
      "cores": 8,
      "cost": 1.9,
      "time": 2327
    },
    {
      "cores": 16,
      "cost": 3.16,
      "time": 1621
    },
    {
      "cores": 32,
      "cost": 5.21,
      "time": 1268
    }
  ],
  "cores": null,
  "error": null,
  "id": "1886418d82e7419fbe79cc5e9c96549a",
  "input_files": [
    {
      "fileid": "3d1a47932b2641ff93933c1a22a37cbb",
      "filename": "namelist.input",
      "size": 4438
    }
  ],
  "model": {
    "domain_shape": [
      [
        30,
        240,
        320
      ]
    ],
    "domains": 1,
    "dt": 60,
    "dx": [
      12000
    ],
    "dy": [
      12000
    ],
    "end_time": [
      "2016-12-26_00:00:00"
    ],
    "name": "wrf",
    "output_interval": [
      60
    ],
    "start_time": [
      "2016-12-25_00:00:00"
    ],
    "version": "3.9"
  },
  "output_files": [],
  "output_size": 0,
  "percent_complete": null,
  "remaining_time": null,
  "required_input_files": [
    "namelist.input",
    "wrfbdy_d01",
    "wrfinput_d01"
  ],
  "status": "ready",
  "time_created": "2017-09-17_21:14:27",
  "time_started": null,
  "time_stopped": null
}
```
Most WRF endpoints return a WRF object in the response body.  

* `compute_options`: A list of dictionaries that can contain multiple options
of parallel cores to use, compute cost, and estimated run time. Simulations
using more parallel cores can finish in less time, but also cost more.
 Each dictionary contains the following fields:
  - `cores`: number of parallel cores (CPUs) to use for the simulation.
  - `cost`: compute cost in US dollars. Your account will be charged with this
  amount at when the simulation is finished.
  - `time`: estimated run time in seconds.
* `cores`: Number of cores selected for the simulation. This parameter is set by the user
when making the request at `https://api.cloudrun.co/v1/wrf/{id}/start`.
`null` if run not yet started.
* `error`: A dictionary with error status and message. `null` if no error.
* `id`: A unique run id.
* `input_files`: A list of dictionaries, one for each input file. Each dictionary contains fields:
  - `fileid`: unique file id
  - `filename`: input file name
  - `size`: file size in bytes
* `model`: A dictionary with following fields:
  - `domain_shape` (grid size in z, y, and x)
  - `domains` (number of WRF nests),
  - `dt` (time step in seconds)
  - `dx`
  - `dy`
  - `dz` (grid spacing)
  - `end_time`
  - `name`
  - `output_interval`
  - `start_time`
  - `version`.
* `output_files`: A list of dictionaries, one for each output_file.
* `output_size`: Total output size in bytes
* `percent_complete`: Current percentage of simulation complete. `null` if run not yet started.
* `remaining_time`: Remaining time in seconds until complete. `null` if run not yet started.
* `required_input_files`: A list of required input files that must be uploaded before starting the run.
* `status`: A status string that can have one of the following values:
  - `created`
  - `ready`
  - `starting`
  - `active`
  - `stopped`
  - `done`
  - `error`
* `time_created`: Time in UTC when the run resource was created.
* `time_started`: Time in UTC when the run resource was started.
* `time_stopped`: Time in UTC when the run resource stopped.

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

This endpoint retrieves all WRF runs that you own or have access to.

### HTTP Request

`GET https://api.cloudrun.co/v1/wrf`

### Response

The response body contains the WRF object.

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

> The response body will look like this:

```json
{
  "id": "f946f8c0d52e45d88952d66d19f727d6",
  "input_files": [
    {
      "fileid": "97755b5e102f4342992621cf949ab71c",
      "filename": "namelist.input",
      "size": 4436
    },
    {
      "fileid": "3c80152b5e2d4ddf9b83a008ef736682",
      "filename": "wrfinput_d01",
      "size": 9605792
    },
    {
      "fileid": "cf7a5c653de64449b8695439905e4759",
      "filename": "wrfbdy_d01",
      "size": 31341856
    }
  ],
  "model": {
    "name": "wrf",
    "version": "3.9"
  },
  "output_files": [
    {
      "filename": "wrfout_d01_2016-12-25_00:00:00",
      "size": 9030712
    },
    {
      "filename": "wrfout_d01_2016-12-25_01:00:00",
      "size": 9030712
    },
    {
      "filename": "wrfout_d01_2016-12-25_02:00:00",
      "size": 9030712
    },
    {
      "filename": "wrfout_d01_2016-12-25_03:00:00",
      "size": 9030712
    },
    {
      "filename": "wrfout_d01_2016-12-25_04:00:00",
      "size": 9030712
    },
    {
      "filename": "wrfout_d01_2016-12-25_05:00:00",
      "size": 9030712
    },
    {
      "filename": "wrfout_d01_2016-12-25_06:00:00",
      "size": 9030712
    },
  ],
  "output_size": 225767800,
  "percent_complete": 100.0,
  "remaining_time": 0,
  "required_input_files": [
    "namelist.input",
    "wrfbdy_d01",
    "wrfinput_d01"
  ],
  "status": "done",
  "time_created": "2017-08-16T17:17:01",
  "time_started": "2017-08-16T17:19:48",
  "time_stopped": "2017-08-16T17:23:58"
}
```

This endpoint retrieves a specific WRF run that you own or have access to.

### URL parameters

Parameter | Description
--------- | -----------
id        | The id of the WRF run to retrieve

### Response

The response body contains the WRF object.

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

> The example response body will look like this:

```json
{
  "id": "c24470f944d8477fac4938c34143cc3c",
  "input_files": [],
  "model": {
    "name": "wrf",
    "version": "3.9"
  },
  "output_files": [],
  "output_size": 0,
  "percent_complete": null,
  "remaining_time": null,
  "required_input_files": [],
  "status": "created",
  "time_created": "2017-08-16T12:53:31",
  "time_started": null,
  "time_stopped": null
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

> The example response body will look like this:

```json
{
  "fileid": "f28764fb966846608b4cb47773d410b7"
}
```

> where `fileid` is the id assigned to the uploaded input file.

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

> The example response body will look like this:

```json
{
  "fileid": "f28764fb966846608b4cb47773d410b7"
}
```

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

> The example response body will look like this:

```json
{
  "id": "c24470f944d8477fac4938c34143cc3c",
  "input_files": [
    {
      "fileid": "f28764fb966846608b4cb47773d410b7",
      "filename": "namelist.input",
      "size": 4436
    },
    {
      "fileid": "e770024687b74f3c8043119c4eb91533",
      "filename": "wrfinput_d01",
      "size": 9605792
    },
    {
      "fileid": "88208d2f83304ac9a60bdbf725afca2e",
      "filename": "wrfbdy_d01",
      "size": 31341856
    }
  ],
  "model": {
    "name": "wrf",
    "version": "3.9"
  },
  "output_files": [],
  "output_size": 0,
  "percent_complete": null,
  "remaining_time": null,
  "required_input_files": [
    "namelist.input",
    "wrfbdy_d01",
    "wrfinput_d01"
  ],
  "status": "starting",
  "time_created": "2017-08-16T12:53:31",
  "time_started": null,
  "time_stopped": null
}
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

> The example response body will look like this:

```json
{
  "id": "f946f8c0d52e45d88952d66d19f727d6",
  "input_files": [
    {
      "fileid": "97755b5e102f4342992621cf949ab71c",
      "filename": "namelist.input",
      "size": 4436
    },
    {
      "fileid": "3c80152b5e2d4ddf9b83a008ef736682",
      "filename": "wrfinput_d01",
      "size": 9605792
    },
    {
      "fileid": "cf7a5c653de64449b8695439905e4759",
      "filename": "wrfbdy_d01",
      "size": 31341856
    }
  ],
  "model": {
    "name": "wrf",
    "version": "3.9"
  },
  "output_files": [],
  "output_size": 0,
  "percent_complete": null,
  "remaining_time": null,
  "required_input_files": [
    "namelist.input",
    "wrfbdy_d01",
    "wrfinput_d01"
  ],
  "status": "stopped",
  "time_created": "2017-08-16T17:17:01",
  "time_started": "2017-08-16T17:19:23",
  "time_stopped": "2017-08-16T18:26:20"
}
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
