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



<img src='https://quickchart.io/chart?width=800&height=400&c=%7Btype%3A%22bar%22%2Cdata%3A%7Blabels%3A%5B%22blacksheep%22%2C%22muffin%22%2C%22sanic%22%2C%22falcon%22%2C%22starlette%22%2C%22baize%22%2C%22fastapi%22%2C%22aiohttp%22%2C%22quart%22%2C%22tornado%22%2C%22django%22%5D%2Cdatasets%3A%5B%7Blabel%3A%22num%20of%20req%22%2Cdata%3A%5B381615%2C329205%2C318495%2C280935%2C242190%2C238515%2C158475%2C139560%2C83595%2C73080%2C27210%5D%7D%5D%7D%7D' />

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
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.8` | 14114 | 3.75 | 6.05 | 4.51
| [muffin](https://pypi.org/project/muffin/) `0.87.3` | 11898 | 4.46 | 7.13 | 5.36
| [sanic](https://pypi.org/project/sanic/) `22.9.1` | 10530 | 4.88 | 8.16 | 6.05
| [falcon](https://pypi.org/project/falcon/) `3.1.1` | 10195 | 5.16 | 8.54 | 6.27
| [starlette](https://pypi.org/project/starlette/) `0.22.0` | 9725 | 5.41 | 8.83 | 6.55
| [baize](https://pypi.org/project/baize/) `0.18.2` | 9274 | 5.72 | 9.21 | 6.87
| [fastapi](https://pypi.org/project/fastapi/) `0.87.0` | 5638 | 9.51 | 12.80 | 11.33
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.3` | 4929 | 12.80 | 13.59 | 12.99
| [quart](https://pypi.org/project/quart/) `0.18.3` | 2550 | 23.67 | 28.13 | 25.19
| [tornado](https://pypi.org/project/tornado/) `6.2` | 1944 | 32.52 | 33.40 | 32.92
| [django](https://pypi.org/project/django/) `4.1.3` | 683 | 86.85 | 102.57 | 93.63


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.8` | 7269 | 7.23 | 11.88 | 8.80
| [sanic](https://pypi.org/project/sanic/) `22.9.1` | 7182 | 7.27 | 11.85 | 8.89
| [muffin](https://pypi.org/project/muffin/) `0.87.3` | 7020 | 7.49 | 12.45 | 9.09
| [falcon](https://pypi.org/project/falcon/) `3.1.1` | 6251 | 8.63 | 11.57 | 10.22
| [starlette](https://pypi.org/project/starlette/) `0.22.0` | 4760 | 11.68 | 12.68 | 13.42
| [baize](https://pypi.org/project/baize/) `0.18.2` | 4745 | 13.37 | 14.63 | 13.48
| [fastapi](https://pypi.org/project/fastapi/) `0.87.0` | 3431 | 16.13 | 19.16 | 18.63
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.3` | 2625 | 24.21 | 25.29 | 24.38
| [quart](https://pypi.org/project/quart/) `0.18.3` | 1714 | 36.68 | 39.33 | 37.32
| [tornado](https://pypi.org/project/tornado/) `6.2` | 1643 | 38.24 | 39.66 | 38.94
| [django](https://pypi.org/project/django/) `4.1.3` | 646 | 91.81 | 110.16 | 98.88

</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.8` | 4058 | 12.78 | 21.91 | 15.75
| [sanic](https://pypi.org/project/sanic/) `22.9.1` | 3521 | 14.46 | 24.48 | 18.14
| [muffin](https://pypi.org/project/muffin/) `0.87.3` | 3029 | 17.20 | 29.55 | 21.10
| [falcon](https://pypi.org/project/falcon/) `3.1.1` | 2283 | 23.31 | 30.58 | 28.11
| [baize](https://pypi.org/project/baize/) `0.18.2` | 1882 | 32.93 | 37.52 | 33.99
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.3` | 1750 | 36.38 | 37.73 | 36.53
| [starlette](https://pypi.org/project/starlette/) `0.22.0` | 1661 | 32.68 | 39.08 | 38.49
| [fastapi](https://pypi.org/project/fastapi/) `0.87.0` | 1496 | 35.63 | 44.38 | 42.69
| [quart](https://pypi.org/project/quart/) `0.18.3` | 1309 | 48.73 | 51.49 | 48.87
| [tornado](https://pypi.org/project/tornado/) `6.2` | 1285 | 49.01 | 50.45 | 49.77
| [django](https://pypi.org/project/django/) `4.1.3` | 485 | 121.99 | 143.72 | 131.45


</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.8` | 381615 | 7.92 | 13.28 | 9.69
| [muffin](https://pypi.org/project/muffin/) `0.87.3` | 329205 | 9.72 | 16.38 | 11.85
| [sanic](https://pypi.org/project/sanic/) `22.9.1` | 318495 | 8.87 | 14.83 | 11.03
| [falcon](https://pypi.org/project/falcon/) `3.1.1` | 280935 | 12.37 | 16.9 | 14.87
| [starlette](https://pypi.org/project/starlette/) `0.22.0` | 242190 | 16.59 | 20.2 | 19.49
| [baize](https://pypi.org/project/baize/) `0.18.2` | 238515 | 17.34 | 20.45 | 18.11
| [fastapi](https://pypi.org/project/fastapi/) `0.87.0` | 158475 | 20.42 | 25.45 | 24.22
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.3` | 139560 | 24.46 | 25.54 | 24.63
| [quart](https://pypi.org/project/quart/) `0.18.3` | 83595 | 36.36 | 39.65 | 37.13
| [tornado](https://pypi.org/project/tornado/) `6.2` | 73080 | 39.92 | 41.17 | 40.54
| [django](https://pypi.org/project/django/) `4.1.3` | 27210 | 100.22 | 118.82 | 107.99

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)