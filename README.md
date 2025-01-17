# Async Python Web Frameworks comparison

https://klen.github.io/py-frameworks-bench/
----------
#### Updated: 2021-09-01

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
* [The Results](#the-results-2021-09-01)
    * [Accept a request and return HTML response with a custom dynamic header](#html)
    * [Parse path params, query string, JSON body and return a json response](#api)
    * [Parse uploaded file, store it on disk and return a text response](#upload)
    * [Composite stats ](#composite)



<img src='https://quickchart.io/chart?width=800&height=400&c=%7Btype%3A%22bar%22%2Cdata%3A%7Blabels%3A%5B%22blacksheep%22%2C%22muffin%22%2C%22falcon%22%2C%22starlette%22%2C%22emmett%22%2C%22sanic%22%2C%22fastapi%22%2C%22aiohttp%22%2C%22tornado%22%2C%22quart%22%2C%22django%22%5D%2Cdatasets%3A%5B%7Blabel%3A%22num%20of%20req%22%2Cdata%3A%5B526905%2C469605%2C447090%2C361845%2C333060%2C306615%2C272805%2C213840%2C124755%2C108585%2C66060%5D%7D%5D%7D%7D' />

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


## The Results (2021-09-01)

<h3 id="html"> Accept a request and return HTML response with a custom dynamic header</h3>
<details open>
<summary> The test simulates just a single HTML response. </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.9` | 18871 | 3.05 | 4.31 | 3.36
| [muffin](https://pypi.org/project/muffin/) `0.84.5` | 16612 | 3.49 | 4.93 | 3.81
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 15754 | 4.07 | 5.17 | 4.02
| [starlette](https://pypi.org/project/starlette/) `0.16.0` | 13725 | 4.22 | 6.01 | 4.63
| [emmett](https://pypi.org/project/emmett/) `2.3.1` | 13516 | 4.17 | 6.10 | 4.70
| [fastapi](https://pypi.org/project/fastapi/) `0.68.1` | 9709 | 5.86 | 8.56 | 6.56
| [sanic](https://pypi.org/project/sanic/) `21.6.2` | 8866 | 6.06 | 9.42 | 7.22
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 7457 | 8.58 | 8.84 | 8.58
| [quart](https://pypi.org/project/quart/) `0.15.1` | 3338 | 19.26 | 20.61 | 19.15
| [tornado](https://pypi.org/project/tornado/) `6.1` | 3310 | 19.15 | 19.80 | 19.33
| [django](https://pypi.org/project/django/) `3.2.6` | 1852 | 34.37 | 37.97 | 34.65


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.9` | 10385 | 5.20 | 8.05 | 6.13
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 10309 | 6.09 | 8.03 | 6.17
| [muffin](https://pypi.org/project/muffin/) `0.84.5` | 10212 | 5.24 | 8.21 | 6.23
| [starlette](https://pypi.org/project/starlette/) `0.16.0` | 7966 | 6.66 | 10.55 | 8.00
| [sanic](https://pypi.org/project/sanic/) `21.6.2` | 7432 | 6.97 | 11.28 | 8.60
| [emmett](https://pypi.org/project/emmett/) `2.3.1` | 7211 | 7.22 | 11.54 | 8.93
| [fastapi](https://pypi.org/project/fastapi/) `0.68.1` | 6260 | 8.49 | 13.46 | 10.19
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 4564 | 14.05 | 14.38 | 14.03
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2783 | 22.95 | 23.63 | 23.00
| [quart](https://pypi.org/project/quart/) `0.15.1` | 2095 | 30.16 | 31.36 | 30.53
| [django](https://pypi.org/project/django/) `3.2.6` | 1566 | 41.41 | 44.99 | 40.82

</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.9` | 5871 | 9.10 | 14.32 | 10.91
| [muffin](https://pypi.org/project/muffin/) `0.84.5` | 4483 | 14.01 | 18.75 | 14.28
| [sanic](https://pypi.org/project/sanic/) `21.6.2` | 4143 | 13.95 | 20.19 | 15.44
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 3743 | 17.80 | 22.05 | 17.19
| [starlette](https://pypi.org/project/starlette/) `0.16.0` | 2432 | 29.97 | 33.69 | 26.25
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 2235 | 28.52 | 29.20 | 28.63
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2224 | 28.67 | 29.34 | 28.77
| [fastapi](https://pypi.org/project/fastapi/) `0.68.1` | 2218 | 32.63 | 36.66 | 28.81
| [quart](https://pypi.org/project/quart/) `0.15.1` | 1806 | 35.19 | 36.32 | 35.42
| [emmett](https://pypi.org/project/emmett/) `2.3.1` | 1477 | 40.70 | 48.60 | 43.29
| [django](https://pypi.org/project/django/) `3.2.6` | 986 | 65.19 | 72.05 | 64.80


</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.0.9` | 526905 | 5.78 | 8.89 | 6.8
| [muffin](https://pypi.org/project/muffin/) `0.84.5` | 469605 | 7.58 | 10.63 | 8.11
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 447090 | 9.32 | 11.75 | 9.13
| [starlette](https://pypi.org/project/starlette/) `0.16.0` | 361845 | 13.62 | 16.75 | 12.96
| [emmett](https://pypi.org/project/emmett/) `2.3.1` | 333060 | 17.36 | 22.08 | 18.97
| [sanic](https://pypi.org/project/sanic/) `21.6.2` | 306615 | 8.99 | 13.63 | 10.42
| [fastapi](https://pypi.org/project/fastapi/) `0.68.1` | 272805 | 15.66 | 19.56 | 15.19
| [aiohttp](https://pypi.org/project/aiohttp/) `3.7.4.post0` | 213840 | 17.05 | 17.47 | 17.08
| [tornado](https://pypi.org/project/tornado/) `6.1` | 124755 | 23.59 | 24.26 | 23.7
| [quart](https://pypi.org/project/quart/) `0.15.1` | 108585 | 28.2 | 29.43 | 28.37
| [django](https://pypi.org/project/django/) `3.2.6` | 66060 | 46.99 | 51.67 | 46.76

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)