# go-oryx

[![Build Status](https://travis-ci.org/ossrs/go-oryx.svg?branch=develop)](https://travis-ci.org/ossrs/go-oryx)
[![codecov](https://codecov.io/gh/ossrs/go-oryx/branch/develop/graph/badge.svg)](https://codecov.io/gh/ossrs/go-oryx)

Oryx is next generation media streaming server, extract service to processes 
which communicates over http with each other, to get more flexible, low latency,
programmable and high maintainable server.

Oryx will implement most of features of [srs](https://github.com/ossrs/srs), 
which is industrial-strength live streaming cluster, for the best conceptual 
integrity and the simplest implementation. In another words, oryx is next-generation 
srs, the srs-ng.

## Architecture

The architecture of oryx is a group of isolate processes:

1. `shell` which exec other processes, the frontend of oryx.
1. `rtmplb` rtmp frontend of oryx, proxy to backend streaming workers.
1. `httplb` http flv/hls+ frontend of oryx, proxy to backend streaming workers.
1. `apilb` http api frontend of oryx, proxy to backend api.
1. `httpx-static` HTTP/HTTPS static server with API proxy([win32](http://ossrs.net/dl/httpx-static-win32.zip)) over [lego/acme](https://github.com/xenolf/lego/tree/master/acme) of [letsencrypt](https://letsencrypt.org/).
1. `srs` the streaming worker, other stream worker is also ok.

```
                         +----------+                   +---------+
                    +----+  API LB  +------HTTP(1985)---+   SRS   +
                    |    +----------+                   +---------+
                    |
                    |    +----------+                   +---------+
                    +----+  HTTP LB +------FLV(8080)----+   SRS   +
                    |    +----------+                   +---------+
                    |
+---------+         |    +----------+                   +---------+
|  Shell  +--exec->-+----+  RTMP LB +------RTMP(1935)---+   SRS   +
+---------+         |    +----------+                   +---------+
                    |
                    |    +------------+
                    +----+ SRS Worker |
                         +------------+
```

Winlin 2016.07.09
