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



<img src='https://quickchart.io/chart?width=800&height=400&c=%7Btype%3A%22bar%22%2Cdata%3A%7Blabels%3A%5B%22blacksheep%22%2C%22muffin%22%2C%22sanic%22%2C%22falcon%22%2C%22starlette%22%2C%22baize%22%2C%22aiohttp%22%2C%22fastapi%22%2C%22tornado%22%2C%22quart%22%2C%22django%22%5D%2Cdatasets%3A%5B%7Blabel%3A%22num%20of%20req%22%2Cdata%3A%5B460680%2C423600%2C413070%2C391725%2C323850%2C307605%2C168270%2C137835%2C108285%2C101010%2C40170%5D%7D%5D%7D%7D' />

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
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.8` | 16691 | 3.20 | 4.94 | 3.80
| [muffin](https://pypi.org/project/muffin/) `0.87.3` | 15197 | 3.44 | 5.47 | 4.17
| [falcon](https://pypi.org/project/falcon/) `3.1.1` | 13958 | 3.73 | 5.98 | 4.55
| [sanic](https://pypi.org/project/sanic/) `22.9.1` | 13898 | 5.26 | 5.68 | 4.56
| [starlette](https://pypi.org/project/starlette/) `0.22.0` | 12286 | 4.20 | 6.80 | 5.17
| [baize](https://pypi.org/project/baize/) `0.18.2` | 12140 | 4.42 | 6.82 | 5.24
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.3` | 6368 | 10.05 | 10.15 | 10.05
| [fastapi](https://pypi.org/project/fastapi/) `0.87.0` | 4631 | 10.84 | 20.10 | 13.79
| [quart](https://pypi.org/project/quart/) `0.18.3` | 3043 | 21.44 | 22.62 | 21.18
| [tornado](https://pypi.org/project/tornado/) `6.2` | 2950 | 21.74 | 21.96 | 21.69
| [django](https://pypi.org/project/django/) `4.1.3` | 1061 | 56.41 | 66.62 | 60.27


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [sanic](https://pypi.org/project/sanic/) `22.9.1` | 9368 | 7.96 | 8.66 | 6.79
| [muffin](https://pypi.org/project/muffin/) `0.87.3` | 9227 | 5.41 | 9.35 | 6.90
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.8` | 9141 | 5.46 | 9.50 | 6.97
| [falcon](https://pypi.org/project/falcon/) `3.1.1` | 9069 | 5.47 | 9.47 | 7.02
| [starlette](https://pypi.org/project/starlette/) `0.22.0` | 7195 | 6.92 | 11.97 | 8.86
| [baize](https://pypi.org/project/baize/) `0.18.2` | 5914 | 11.04 | 11.39 | 10.80
| [fastapi](https://pypi.org/project/fastapi/) `0.87.0` | 3104 | 16.24 | 29.91 | 20.59
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.3` | 2949 | 21.64 | 21.86 | 21.70
| [tornado](https://pypi.org/project/tornado/) `6.2` | 2426 | 26.42 | 26.78 | 26.38
| [quart](https://pypi.org/project/quart/) `0.18.3` | 2126 | 29.73 | 30.75 | 30.08
| [django](https://pypi.org/project/django/) `4.1.3` | 918 | 65.47 | 76.00 | 69.57

</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.8` | 4880 | 10.16 | 17.93 | 13.07
| [sanic](https://pypi.org/project/sanic/) `22.9.1` | 4272 | 17.58 | 19.31 | 14.94
| [muffin](https://pypi.org/project/muffin/) `0.87.3` | 3816 | 13.05 | 23.00 | 16.73
| [falcon](https://pypi.org/project/falcon/) `3.1.1` | 3088 | 16.59 | 27.53 | 20.80
| [baize](https://pypi.org/project/baize/) `0.18.2` | 2453 | 25.36 | 28.68 | 26.06
| [starlette](https://pypi.org/project/starlette/) `0.22.0` | 2109 | 24.41 | 40.38 | 30.29
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.3` | 1901 | 33.63 | 33.84 | 33.64
| [tornado](https://pypi.org/project/tornado/) `6.2` | 1843 | 34.75 | 35.17 | 34.70
| [quart](https://pypi.org/project/quart/) `0.18.3` | 1565 | 40.99 | 42.34 | 40.90
| [fastapi](https://pypi.org/project/fastapi/) `0.87.0` | 1454 | 34.68 | 62.77 | 43.93
| [django](https://pypi.org/project/django/) `4.1.3` | 699 | 86.94 | 92.51 | 91.35


</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.8` | 460680 | 6.27 | 10.79 | 7.95
| [muffin](https://pypi.org/project/muffin/) `0.87.3` | 423600 | 7.3 | 12.61 | 9.27
| [sanic](https://pypi.org/project/sanic/) `22.9.1` | 413070 | 10.27 | 11.22 | 8.76
| [falcon](https://pypi.org/project/falcon/) `3.1.1` | 391725 | 8.6 | 14.33 | 10.79
| [starlette](https://pypi.org/project/starlette/) `0.22.0` | 323850 | 11.84 | 19.72 | 14.77
| [baize](https://pypi.org/project/baize/) `0.18.2` | 307605 | 13.61 | 15.63 | 14.03
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.3` | 168270 | 21.77 | 21.95 | 21.8
| [fastapi](https://pypi.org/project/fastapi/) `0.87.0` | 137835 | 20.59 | 37.59 | 26.1
| [tornado](https://pypi.org/project/tornado/) `6.2` | 108285 | 27.64 | 27.97 | 27.59
| [quart](https://pypi.org/project/quart/) `0.18.3` | 101010 | 30.72 | 31.9 | 30.72
| [django](https://pypi.org/project/django/) `4.1.3` | 40170 | 69.61 | 78.38 | 73.73

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)