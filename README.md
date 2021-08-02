# Async Python Web Frameworks comparison

https://klen.github.io/py-frameworks-bench/
----------
#### Updated: 2021-08-02

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
* [The Results](#the-results-2021-08-02)
    * [Accept a request and return HTML response with a custom dynamic header](#html)
    * [Parse path params, query string, JSON body and return a json response](#api)
    * [Parse uploaded file, store it on disk and return a text response](#upload)
    * [Composite stats ](#composite)



<img src='https://quickchart.io/chart?width=800&height=400&c=%7Btype%3A%22bar%22%2Cdata%3A%7Blabels%3A%5B%22blacksheep%22%2C%22muffin%22%2C%22falcon%22%2C%22starlette%22%2C%22emmett%22%2C%22sanic%22%2C%22fastapi%22%2C%22aiohttp%22%2C%22tornado%22%2C%22quart%22%2C%22django%22%5D%2Cdatasets%3A%5B%7Blabel%3A%22num%20of%20req%22%2Cdata%3A%5B545790%2C486870%2C455655%2C373365%2C344445%2C323880%2C279675%2C227055%2C128880%2C113910%2C70200%5D%7D%5D%7D%7D' />

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


## The Results (2021-08-02)

<h3 id="html"> Accept a request and return HTML response with a custom dynamic header</h3>
<details open>
<summary> The test simulates just a single HTML response. </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.9` | 19432 | 2.70 | 4.27 | 3.26
| [muffin](https://pypi.org/project/muffin/) `0.84.1` | 17250 | 3.08 | 4.78 | 3.67
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 16127 | 3.24 | 5.15 | 3.93
| [starlette](https://pypi.org/project/starlette/) `0.16.0` | 14000 | 3.61 | 6.00 | 4.54
| [emmett](https://pypi.org/project/emmett/) `2.2.3` | 13977 | 3.63 | 6.04 | 4.54
| [fastapi](https://pypi.org/project/fastapi/) `0.67.0` | 9929 | 5.06 | 8.49 | 6.41
| [sanic](https://pypi.org/project/sanic/) `21.6.0` | 9308 | 5.41 | 8.96 | 6.89
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 7815 | 8.14 | 8.23 | 8.19
| [quart](https://pypi.org/project/quart/) `0.15.1` | 3522 | 18.70 | 19.41 | 18.15
| [tornado](https://pypi.org/project/tornado/) `6.1` | 3422 | 18.68 | 18.80 | 18.71
| [django](https://pypi.org/project/django/) `3.2.5` | 1977 | 32.21 | 35.93 | 32.38


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.9` | 10836 | 4.67 | 7.84 | 5.87
| [muffin](https://pypi.org/project/muffin/) `0.84.1` | 10574 | 4.66 | 8.10 | 6.02
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 10470 | 4.76 | 8.11 | 6.08
| [starlette](https://pypi.org/project/starlette/) `0.16.0` | 8298 | 5.92 | 10.28 | 7.68
| [sanic](https://pypi.org/project/sanic/) `21.6.0` | 7850 | 6.23 | 10.67 | 8.16
| [emmett](https://pypi.org/project/emmett/) `2.2.3` | 7383 | 6.53 | 11.44 | 8.76
| [fastapi](https://pypi.org/project/fastapi/) `0.67.0` | 6441 | 7.59 | 13.31 | 9.90
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 4884 | 13.11 | 13.26 | 13.10
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2919 | 21.96 | 22.07 | 21.93
| [quart](https://pypi.org/project/quart/) `0.15.1` | 2208 | 28.58 | 29.09 | 28.97
| [django](https://pypi.org/project/django/) `3.2.5` | 1656 | 39.50 | 42.58 | 38.60

</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.9` | 6118 | 8.03 | 14.20 | 10.45
| [muffin](https://pypi.org/project/muffin/) `0.84.1` | 4634 | 10.75 | 18.89 | 13.78
| [sanic](https://pypi.org/project/sanic/) `21.6.0` | 4434 | 11.05 | 19.37 | 14.44
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 3780 | 13.30 | 22.46 | 17.01
| [starlette](https://pypi.org/project/starlette/) `0.16.0` | 2593 | 19.15 | 33.15 | 24.64
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 2438 | 26.23 | 26.37 | 26.25
| [fastapi](https://pypi.org/project/fastapi/) `0.67.0` | 2275 | 22.18 | 37.71 | 28.09
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2251 | 28.38 | 28.49 | 28.43
| [quart](https://pypi.org/project/quart/) `0.15.1` | 1864 | 34.31 | 35.01 | 34.31
| [emmett](https://pypi.org/project/emmett/) `2.2.3` | 1603 | 36.49 | 45.73 | 39.90
| [django](https://pypi.org/project/django/) `3.2.5` | 1047 | 60.93 | 67.76 | 61.00


</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.9` | 545790 | 5.13 | 8.77 | 6.53
| [muffin](https://pypi.org/project/muffin/) `0.84.1` | 486870 | 6.16 | 10.59 | 7.82
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 455655 | 7.1 | 11.91 | 9.01
| [starlette](https://pypi.org/project/starlette/) `0.16.0` | 373365 | 9.56 | 16.48 | 12.29
| [emmett](https://pypi.org/project/emmett/) `2.2.3` | 344445 | 15.55 | 21.07 | 17.73
| [sanic](https://pypi.org/project/sanic/) `21.6.0` | 323880 | 7.56 | 13.0 | 9.83
| [fastapi](https://pypi.org/project/fastapi/) `0.67.0` | 279675 | 11.61 | 19.84 | 14.8
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 227055 | 15.83 | 15.95 | 15.85
| [tornado](https://pypi.org/project/tornado/) `6.1` | 128880 | 23.01 | 23.12 | 23.02
| [quart](https://pypi.org/project/quart/) `0.15.1` | 113910 | 27.2 | 27.84 | 27.14
| [django](https://pypi.org/project/django/) `3.2.5` | 70200 | 44.21 | 48.76 | 43.99

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)