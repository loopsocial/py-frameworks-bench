# Async Python Web Frameworks comparison

https://klen.github.io/py-frameworks-bench/
----------
#### Updated: 2022-11-21

[![benchmarks](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml)
[![tests](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml)

----------

This is a simple benchmark for python async frameworks. Almost all of the
frameworks are ASGI-compatible (aiohttp and tornado are exceptions on the
moment).

The objective of the benchmark is not testing deployment (like uvicorn vs
hypercorn and etc) or database (ORM, drivers) but instead test the frameworks
itself. The benchmark checks request parsing (body, headers, formdata,
queries), routing, responses.

## Table of contents

* [The Methodic](#the-methodic)
* [The Results](#the-results-2022-11-21)
    * [Accept a request and return HTML response with a custom dynamic header](#html)
    * [Parse path params, query string, JSON body and return a json response](#api)
    * [Parse uploaded file, store it on disk and return a text response](#upload)
    * [Composite stats ](#composite)



<img src='https://quickchart.io/chart?width=800&height=400&c=%7Btype%3A%22bar%22%2Cdata%3A%7Blabels%3A%5B%22blacksheep%22%2C%22muffin%22%2C%22sanic%22%2C%22falcon%22%2C%22starlette%22%2C%22baize%22%2C%22fastapi%22%2C%22aiohttp%22%2C%22tornado%22%2C%22quart%22%2C%22django%22%5D%2Cdatasets%3A%5B%7Blabel%3A%22num%20of%20req%22%2Cdata%3A%5B526320%2C475020%2C468810%2C440055%2C359700%2C353775%2C248430%2C193440%2C124425%2C113880%2C45615%5D%7D%5D%7D%7D' />

## The Methodic

The benchmark runs as a [Github Action](https://github.com/features/actions).
According to the [github
documentation](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners)
the hardware specification for the runs is:

* 2-core vCPU (Intel® Xeon® Platinum 8272CL (Cascade Lake), Intel® Xeon® 8171M 2.1GHz (Skylake))
* 7 GB of RAM memory
* 14 GB of SSD disk space
* OS Ubuntu 20.04

[ASGI](https://asgi.readthedocs.io/en/latest/) apps are running from docker using the gunicorn/uvicorn command:

    gunicorn -k uvicorn.workers.UvicornWorker -b 0.0.0.0:8080 app:app

Applications' source code can be found
[here](https://github.com/klen/py-frameworks-bench/tree/develop/frameworks).

Results received with WRK utility using the params:

    wrk -d15s -t4 -c64 [URL]

The benchmark has a three kind of tests:

1. "Simple" test: accept a request and return HTML response with custom dynamic
   header. The test simulates just a single HTML response.

2. "API" test: Check headers, parse path params, query string, JSON body and return a json
   response. The test simulates an JSON REST API.

3. "Upload" test: accept an uploaded file and store it on disk. The test
   simulates multipart formdata processing and work with files.


## The Results (2022-11-21)

<h3 id="html"> Accept a request and return HTML response with a custom dynamic header</h3>
<details open>
<summary> The test simulates just a single HTML response. </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.8` | 19048 | 2.79 | 4.34 | 3.32
| [muffin](https://pypi.org/project/muffin/) `0.87.3` | 16838 | 3.11 | 4.93 | 3.77
| [sanic](https://pypi.org/project/sanic/) `22.9.1` | 15744 | 4.75 | 5.08 | 4.02
| [falcon](https://pypi.org/project/falcon/) `3.1.1` | 15646 | 3.31 | 5.36 | 4.06
| [baize](https://pypi.org/project/baize/) `0.18.2` | 14057 | 3.65 | 6.00 | 4.52
| [starlette](https://pypi.org/project/starlette/) `0.22.0` | 13610 | 3.87 | 6.18 | 4.67
| [fastapi](https://pypi.org/project/fastapi/) `0.87.0` | 8786 | 5.63 | 9.87 | 7.26
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.3` | 7279 | 8.74 | 8.81 | 8.79
| [quart](https://pypi.org/project/quart/) `0.18.3` | 3473 | 18.98 | 19.86 | 18.44
| [tornado](https://pypi.org/project/tornado/) `6.2` | 3360 | 18.96 | 19.04 | 19.06
| [django](https://pypi.org/project/django/) `4.1.3` | 1196 | 50.25 | 54.10 | 53.38


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [sanic](https://pypi.org/project/sanic/) `22.9.1` | 10667 | 7.13 | 7.59 | 5.96
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.8` | 10561 | 4.70 | 8.17 | 6.04
| [muffin](https://pypi.org/project/muffin/) `0.87.3` | 10432 | 4.78 | 8.29 | 6.10
| [falcon](https://pypi.org/project/falcon/) `3.1.1` | 10244 | 4.86 | 8.51 | 6.22
| [starlette](https://pypi.org/project/starlette/) `0.22.0` | 7994 | 6.21 | 10.93 | 7.97
| [baize](https://pypi.org/project/baize/) `0.18.2` | 6706 | 9.71 | 10.09 | 9.53
| [fastapi](https://pypi.org/project/fastapi/) `0.87.0` | 5713 | 8.58 | 15.62 | 11.17
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.3` | 3377 | 18.89 | 19.05 | 18.94
| [tornado](https://pypi.org/project/tornado/) `6.2` | 2851 | 22.43 | 22.53 | 22.45
| [quart](https://pypi.org/project/quart/) `0.18.3` | 2376 | 26.54 | 27.19 | 26.93
| [django](https://pypi.org/project/django/) `4.1.3` | 1052 | 57.45 | 61.50 | 60.68

</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.8` | 5479 | 8.96 | 16.04 | 11.64
| [sanic](https://pypi.org/project/sanic/) `22.9.1` | 4843 | 11.28 | 17.28 | 13.19
| [muffin](https://pypi.org/project/muffin/) `0.87.3` | 4398 | 11.34 | 20.15 | 14.51
| [falcon](https://pypi.org/project/falcon/) `3.1.1` | 3447 | 14.46 | 25.12 | 18.65
| [baize](https://pypi.org/project/baize/) `0.18.2` | 2822 | 22.10 | 24.78 | 22.67
| [starlette](https://pypi.org/project/starlette/) `0.22.0` | 2376 | 21.29 | 37.27 | 26.88
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.3` | 2240 | 28.57 | 28.73 | 28.60
| [tornado](https://pypi.org/project/tornado/) `6.2` | 2084 | 30.67 | 30.84 | 30.70
| [fastapi](https://pypi.org/project/fastapi/) `0.87.0` | 2063 | 24.06 | 42.72 | 30.97
| [quart](https://pypi.org/project/quart/) `0.18.3` | 1743 | 36.66 | 37.51 | 36.68
| [django](https://pypi.org/project/django/) `4.1.3` | 793 | 76.47 | 78.93 | 80.42


</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.8` | 526320 | 5.48 | 9.52 | 7.0
| [muffin](https://pypi.org/project/muffin/) `0.87.3` | 475020 | 6.41 | 11.12 | 8.13
| [sanic](https://pypi.org/project/sanic/) `22.9.1` | 468810 | 7.72 | 9.98 | 7.72
| [falcon](https://pypi.org/project/falcon/) `3.1.1` | 440055 | 7.54 | 13.0 | 9.64
| [starlette](https://pypi.org/project/starlette/) `0.22.0` | 359700 | 10.46 | 18.13 | 13.17
| [baize](https://pypi.org/project/baize/) `0.18.2` | 353775 | 11.82 | 13.62 | 12.24
| [fastapi](https://pypi.org/project/fastapi/) `0.87.0` | 248430 | 12.76 | 22.74 | 16.47
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.3` | 193440 | 18.73 | 18.86 | 18.78
| [tornado](https://pypi.org/project/tornado/) `6.2` | 124425 | 24.02 | 24.14 | 24.07
| [quart](https://pypi.org/project/quart/) `0.18.3` | 113880 | 27.39 | 28.19 | 27.35
| [django](https://pypi.org/project/django/) `4.1.3` | 45615 | 61.39 | 64.84 | 64.83

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)