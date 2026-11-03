# etcd/clientv3

[![Docs](https://img.shields.io/badge/docs-latest-green.svg)](https://etcd.io/docs)
[![Godoc](https://img.shields.io/badge/go-documentation-blue.svg?style=flat-square)](https://godoc.org/go.etcd.io/etcd/clientv3)

`etcd/clientv3` is the official Go etcd client for v3.

## Install

```bash
go get go.etcd.io/etcd/clientv3
```

## Get started

Create client using `clientv3.New`:

```go
cli, err := clientv3.New(clientv3.Config{
	Endpoints:   []string{"localhost:2379", "localhost:22379", "localhost:32379"},
	DialTimeout: 5 * time.Second,
})
if err != nil {
	// handle error!
}
defer cli.Close()
```

etcd v3 uses [`gRPC`](https://www.grpc.io) for remote procedure calls. And `clientv3` uses
[`grpc-go`](https://github.com/grpc/grpc-go) to connect to etcd. Make sure to close the client after using it.
If the client is not closed, the connection will have leaky goroutines. To specify client request timeout,
pass `context.WithTimeout` to APIs:

```go
ctx, cancel := context.WithTimeout(context.Background(), timeout)
resp, err := cli.Put(ctx, "sample_key", "sample_value")
cancel()
if err != nil {
    // handle error!
}
// use the response
```

For full compatibility, it is recommended to vendor builds using etcd's vendored packages, using tools like `golang/dep`, as in [vendor directories](https://golang.org/cmd/go/#hdr-Vendor_Directories).

## Error Handling

etcd client returns 2 types of errors:

1. context error: canceled or deadline exceeded.
2. gRPC error: see [api/v3rpc/rpctypes](https://godoc.org/go.etcd.io/etcd/etcdserver/api/v3rpc/rpctypes).

Here is the example code to handle client errors:

```go
resp, err := cli.Put(ctx, "", "")
if err != nil {
	switch err {
	case context.Canceled:
		log.Fatalf("ctx is canceled by another routine: %v", err)
	case context.DeadlineExceeded:
		log.Fatalf("ctx is attached with a deadline is exceeded: %v", err)
	case rpctypes.ErrEmptyKey:
		log.Fatalf("client-side error: %v", err)
	default:
		log.Fatalf("bad cluster endpoints, which are not etcd servers: %v", err)
	}
}
```

## Metrics

The etcd client optionally exposes RPC metrics through [go-grpc-prometheus](https://github.com/grpc-ecosystem/go-grpc-prometheus). See the [examples](https://github.com/etcd-io/etcd/blob/master/clientv3/example_metrics_test.go).

## Namespacing

The [namespace](https://godoc.org/go.etcd.io/etcd/clientv3/namespace) package provides `clientv3` interface wrappers to transparently isolate client requests to a user-defined prefix.

## Request size limit

Client request size limit is configurable via `clientv3.Config.MaxCallSendMsgSize` and `MaxCallRecvMsgSize` in bytes. If none given, client request send limit defaults to 2 MiB including gRPC overhead bytes. And receive limit defaults to `math.MaxInt32`.

## Examples

More code examples can be found at [GoDoc](https://godoc.org/go.etcd.io/etcd/clientv3).
Hello World 2

---

## 🚲 Bicycle Advocacy 🚲

We believe that cycling is one of the most powerful tools we have for building healthier communities, reducing carbon emissions, and reclaiming our streets.

### Why Bicycles Matter

- **Climate action** — A bicycle produces zero direct emissions. Replacing even one car trip per day with a bike ride can save hundreds of kilograms of CO₂ per year.
- **Healthier cities** — Cycling reduces traffic congestion, improves air quality, and lowers noise pollution, making urban spaces more liveable for everyone.
- **Personal health** — Regular cycling improves cardiovascular fitness, mental well-being, and longevity.
- **Equity & access** — Bicycles are affordable, require no fuel, and open up mobility to people who cannot drive or afford a car.
- **Economic benefits** — Cyclists spend more at local businesses per kilometre travelled than motorists.

### How You Can Help

1. **Ride when you can.** Every trip made by bike instead of car counts.
2. **Advocate locally.** Attend city council meetings and support protected bike lane proposals.
3. **Be visible.** Use lights, wear bright clothing, and ride predictably.
4. **Welcome newcomers.** Offer to do a group ride with someone new to cycling.
5. **Support bike-friendly businesses.** Patronise shops that provide bike parking and commuter facilities.

### Resources

- [PeopleForBikes](https://www.peopleforbikes.org)
- [European Cyclists Federation](https://ecf.com)
- [Cycling UK](https://www.cyclinguk.org)
- [Bike to Work Day](https://bikeleague.org/bikemonth/)

> *"Life is like riding a bicycle. To keep your balance, you must keep moving."* — Albert Einstein
