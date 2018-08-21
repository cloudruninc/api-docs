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
from cloudrun import Run

run = Run('https://api.cloudrun.co/v1', token)
```

```shell
# With curl, pass the authentication header with each request
curl https://api.cloudrun.co/v1/runs \
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

# Runs

## Introduction

A _run_ is the main Cloudrun resource that you will be working with.
It allows you to set the parameters of your model run, upload input files,
start and stop a run, download output files, or sample model output.

## Creating a new run

```python
from cloudrun import Run

run = Run('https://api.cloudrun.co/v1', token)
run.create(model='wrf', version='3.9.1')
```

```shell
curl -X POST https://api.cloudrun.co/v1/runs \
     -H "Authorization: Bearer $token" \
     -F "model=wrf" \
     -F "version=3.9.1"
```

This endpoint creates a new WRF run, and will return the `Run` JSON object
in the response.

### HTTP Request

`POST https://api.cloudrun.co/v1/runs`

### Form parameters

Parameter | Description
--------- | -----------
model     | Name of model to use
version   | Model version to use

<aside class="notice">
The only model option currently supported is `wrf`.
</aside>

<aside class="notice">
Only WRF versions 3.8.1, 3.9, and 3.9.1 are currently supported.
</aside>

### Response

The response body contains the Run object.

```json
{
  "compute_config": {},
  "compute_options": [],
  "disk_usage": 0,
  "error": null,
  "id": "cac65d8d-5b91-4fc9-b1b7-8096aa1f3c9e",
  "input_files": [],
  "model": "wrf",
  "output_files": [],
  "percent_complete": 0.0,
  "remaining_time": null,
  "required_input_files": [
    "namelist.input"
  ],
  "run_name": null,
  "selected_compute_option": {},
  "status": "created",
  "time_created": "2018-08-21T02:16:59.707338Z",
  "time_started": null,
  "time_stopped": null,
  "version": "3.9.1"
}
```
Most `runs` endpoints return a `Run` object in JSON format in the response body.
See an example response on the right.  

* **compute_config**: Optional compute settings to use instead of `selected_compute_option`.
* **compute_options**: A list of dictionaries that can contain multiple options
of parallel cores to use, compute cost, and estimated run time. Simulations
using more parallel cores can finish in less time, but also cost more.
 Each dictionary contains the following fields:
  - **cores**: number of parallel cores (CPUs) to use for the simulation.
  - **cost**: compute cost in US dollars. Your account will be charged with this
  amount at when the simulation is finished.
  - **time**: estimated run time in seconds.
* **disk_usage**: Total disk usage by your run.
* **error**: A dictionary with error status and message. `null` if no error.
* **id**: Unique run id.
* **input_files**: A list of dictionaries, one for each input file. Each dictionary contains fields:
  - **fileid**: Unique input file id.
  - **filename**: Input file name.
  - **size**: File size in bytes.
* **model**: A dictionary with the following fields:
* **output_files**: A list of dictionaries, one for each output_file.
Each dictionary contains the following fields:
  - **filename**: Output file name.
  - **size**: File size in bytes.
* **output_size**: Total output size in bytes.
* **percent_complete**: Current percentage of simulation complete.
`null` if run not yet started, and `100.0` if complete.
* **remaining_time**: Remaining time in seconds until complete.
`null` if run not yet started, and `0` if complete.
* **required_input_files**: A list of required input files that must be uploaded
before starting the run.
* **run_name**: Name for your run.
* **selected_compute_option**: An ID used to select one of the available compute options.
* **status**: A status string that can have one of the following values:
  - `created`: A run resource has been successfully created but not started.
  - `ready`: The run resource is configured and waiting for user request to start.
  - `starting`: The run resource received user request to start, and is in the
  process of spinning up compute nodes and uploading files.
  - `active`: The run is currently in progress.
  - `stopped`: The run was stopped by the user before completion.
  - `done`: The run was successfully completed.
  - `error`: The run encountered an error and has stopped. See the *error* field for more information.
* **time_created**: Time when the run resource was created. The time is in UTC and
  using ISO 8601 standard, `%Y-%m-%d_%H:%M:%S`.
* **time_started**: Time when the run resource was started. The time is in UTC and
  using ISO 8601 standard, `%Y-%m-%d_%H:%M:%S`.
* **time_stopped**: Time when the run resource stopped. The time is in UTC and
  using ISO 8601 standard, `%Y-%m-%d_%H:%M:%S`.
* **version**: Model version, e.g. `3.9.1`.

## Getting all runs

<aside class="warning">
This endpoint is not yet available.
Email us at <a href='mailto:help@cloudrun.co'>help@cloudrun.co</a>
if you want this endpoint to be available sooner.
</aside>

## Get a specific run

```python
from cloudrun import Run

run = Run('https://api.cloudrun.co/v1', token, id=id)
```

```shell
curl https://api.cloudrun.co/v1/runs/$id \
    -H "Authorization: Bearer $token"
```

This endpoint retrieves a specific WRF run that you own or have access to.

### URL parameters

Parameter | Description
--------- | -----------
id        | The id of the WRF run to retrieve

### Response

The response body contains the `Run` object.

## Uploading input files

```python
run.upload(file=path_to_file)
```

```shell
curl -X POST https://api.cloudrun.co/v1/runs/${id}/input \
     -H "Authorization: Bearer $token" \
     -F "file=@$path_to_file"
```

> `path_to_file` parameter must point to a valid input file, e.g. `wrfinput_d01`.

> The response body will look like this, for example when uploading the WRF
> namelist file namelist.input:

```json
{
  "file": {
    "id": "a3d32942-b172-435d-9354-d49f4b1cfef8",
    "name": "namelist.input",
    "size": 4438
  }
}
```

> where `id` is the id assigned to the uploaded input file.

This endpoint lets you upload an input file to your run.

### HTTP Request

`POST https://api.cloudrun.co/v1/runs/{id}/input`

### Form parameters

Parameter | Description
--------- | -----------
file      | Path to a valid input file

### Response

Parameter | Description
--------- | -----------
id        | Unique ID of your input file
name      | File name
size      | File size in bytes

## Uploading input files from URL

```python
run.upload(url=url_to_file)
```

```shell
curl -X POST https://api.cloudrun.co/v1/runs/${id}/input \
     -H "Authorization: Bearer $token" \
     -F "url=$url_to_file"
```

> `url_to_file` parameter must point to a valid input file
on the web, e.g. `wrfinput_d01`. This could be a file
hosted from an http or ftp server, or an AWS S3 bucket.

> The response body will look like this, for example when uploading the WRF
> namelist file namelist.input from url:

```json
{
  "file": {
    "id": "a3d32942-b172-435d-9354-d49f4b1cfef8",
    "name": "namelist.input",
    "size": 4438
  }
}
```

This endpoint lets you upload an input file from a remote URL to your run.

### HTTP Request

`POST https://api.cloudrun.co/v1/runs/{id}/input`

### Form parameters

Parameter | Description
--------- | -----------
url       | URL to a remote input file

### Response

Parameter | Description
--------- | -----------
id        | Unique ID of your input file
name      | File name
size      | File size in bytes

## Selecting your compute option

```shell
curl -X PATCH ${CLOUDRUN_API_URL}/runs/${id} \
     -H "Authorization: Bearer $CLOUDRUN_API_TOKEN" \
```

After you have uploaded a valid WRF namelist file (`namelist.input`),
Cloudrun server will parse the namelist and return a number of available
compute options in the response. Each option uses a different number of
parallel cores, and has different estimated time to completion and run price.

```json
"compute_options": [
  {
    "compute_option_id": "ee418969-e1de-4f44-b053-d8349baf01aa",
    "cores": 2,
    "cost": "1.0",
    "time": 23
  },
  {
    "compute_option_id": "d3762d0d-5a83-4b56-bd48-7b6f2467efea",
    "cores": 4,
    "cost": "1.14",
    "time": 16
  },
  {
    "compute_option_id": "bb20dc53-50ee-4178-ad4a-c14518b8420d",
    "cores": 8,
    "cost": "1.9",
    "time": 12
  },
  {
    "compute_option_id": "06f75013-84db-42c3-80b4-e4fa733df6ea",
    "cores": 16,
    "cost": "3.16",
    "time": 10
  },
  {
    "compute_option_id": "7a3ecb91-a0c7-4f51-85c1-b62e7a7bf69d",
    "cores": 32,
    "cost": "5.21",
    "time": 10
  }
]
```

## Starting your WRF run

```python
run.start(cores=32)
```

```shell
curl -X POST https://api.cloudrun.co/v1/runs/${id}/start \
     -H "Authorization: Bearer $token" \
     -F "cores=32"

# or

curl -X POST https://api.cloudrun.co/v1/runs/${id}/start \
     -H "Authorization: Bearer $token" \
     -F "selected_compute_option=$compute_option_id"
```

Use this endpoint to start your run once it has been properly configured
and all the input files have been uploaded.

There are two different ways you can start your run:

1. By passing a number of parallel cores (`cores`) in the request body.
2. By passing a valid compute option ID in the request body.

<aside class="notice">
This endpoint can be used only after uploading all the required input files.
</aside>

### HTTP Request

`POST https://api.cloudrun.co/v1/wrf/{id}/start`

### Form parameters

Parameter | Description
--------- | -----------
cores     | Number of compute cores to use

### Response

The response body contains the `Run` JSON object.

## Stopping your WRF run

```python
run.stop()
```

```shell
curl -X POST https://api.cloudrun.co/v1/runs/${id}/stop \
     -H "Authorization: Bearer $token"
```

Use this endpoint to stop a currently active run.
This should be necessary only if you want to stop your run before it completes.

<aside class="notice">
This endpoint can be used only after calling the `start` endpoint.
</aside>

### HTTP Request

`POST https://api.cloudrun.co/v1/runs/{id}/stop`

### Response

The response body contains the `Run` JSON object.

## Deleting your WRF run

```python
run.delete()
```

```shell
curl -X DELETE https://api.cloudrun.co/v1/runs/${id} \
     -H "Authorization: Bearer $token"
```

Use this endpoint to delete the output files from your run.
You will still be able to access the metadata of your run.

<aside class="warning">
This request cannot be undone.
</aside>

### HTTP Request

`DELETE https://api.cloudrun.co/v1/runs/{id}`

### Response

The response body contains the `Run` JSON object.
