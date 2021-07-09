# GOproxy
PoC for learning how work proxy and load balancer like nginx server


## GOproxy command

> Build

```bash
make build
```

> List of command available

```bash

  -backends string
        Load balanced backends, use commas to separate
  -genkey
        Action for generate hash for protected routes
  -portLB int
        Port to serve to run load balancing (default 3030)
  -portProxy int
        Port to serve to run proxy (default 5000)
  -prevkey string
        Action for save a previous hash for protected routes to validate JWT
  -type string
        Main Service default is proxy (default "proxy")
```

> Start Proxy server first time 

`genkey` command in true generate random secretkey and save on badgerdb on `badger.data`

```bash
./goproxy -portProxy 5000 -genkey true
```

`prevkey` command receive a custom secretkey and save this on badgerdb on `badger.data`

```bash
./goproxy -portProxy 5000 -prevkey <secretKey>
```

> Start Proxy server

```bash
./goproxy -portProxy 5001
```

> Start load balancer

```bash
./goproxy -type lb --backends "http://localhost:5000,http://localhost:5001,http://localhost:5002"
```

> Start API PoC service

```bash
go run  services/micro-a/api.go --port <port>
```

Install badger db on window if you don`t use CGO
```bash
CGO_ENABLED=0 go get github.com/dgraph-io/badger/v3
```

BenchMarking
------------

```bash
ab -c 1000 -n 10000 http://localhost:5000/health
```