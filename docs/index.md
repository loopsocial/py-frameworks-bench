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
            labels: ['blacksheep','muffin','sanic','falcon','starlette','baize','fastapi','aiohttp','quart','tornado','django',],
            datasets: [
                {
                    label: '# of requests',
                    data: ['381615','329205','318495','280935','242190','238515','158475','139560','83595','73080','27210',],
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


More details: [Results (2022-11-21)](_posts/2022-11-21-results.md)