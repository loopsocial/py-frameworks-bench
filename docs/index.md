---
layout: home
list_title: Archive
description: Python frameworks benchmarks
---

<script src="https://cdn.jsdelivr.net/npm/chart.js@3.2.1/dist/chart.min.js"></script>

This is a simple benchmark for python async frameworks. Almost all of the
frameworks are ASGI-compatible (aiohttp and tornado are exceptions on the
moment).

The objective of the benchmark is not testing deployment (like uvicorn vs
hypercorn and etc) or database (ORM, drivers) but instead test the frameworks
itself. The benchmark checks request parsing (body, headers, formdata,
queries), routing, responses.

* Read about the benchmark: [The Methodic](methodic.md)
* Check complete results for the latest benchmark here: [Results (2022-11-21)](_posts/2022-11-21-results.md)

[![benchmarks](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml)
[![tests](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml)

## Combined results

<canvas id="chart" style="margin-bottom: 2em"></canvas>
<script>
    var ctx = document.getElementById('chart').getContext('2d');
    var myChart = new Chart(ctx, {
        type: 'bar',
        data: {
            labels: ['blacksheep','muffin','sanic','falcon','starlette','baize','aiohttp','fastapi','tornado','quart','django',],
            datasets: [
                {
                    label: '# of requests',
                    data: ['460680','423600','413070','391725','323850','307605','168270','137835','108285','101010','40170',],
                    backgroundColor: [
                        '#4E79A7', '#A0CBE8', '#F28E2B', '#FFBE7D', '#59A14F', '#8CD17D', '#B6992D', '#F1CE63', '#499894', '#86BCB6', '#E15759', '#FF9D9A', '#79706E', '#BAB0AC', '#D37295', '#FABFD2', '#B07AA1', '#D4A6C8', '#9D7660', '#D7B5A6',
                    ]
                },
            ]
        }
    });
</script>

Sorted by sum of completed requests

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


More details: [Results (2022-11-21)](_posts/2022-11-21-results.md)