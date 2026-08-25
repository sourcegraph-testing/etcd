# etcd

[![Go Report Card](https://goreportcard.com/badge/github.com/etcd-io/etcd?style=flat-square)](https://goreportcard.com/report/github.com/etcd-io/etcd)
[![Coverage](https://codecov.io/gh/etcd-io/etcd/branch/master/graph/badge.svg)](https://codecov.io/gh/etcd-io/etcd)
[![Go Reference](https://pkg.go.dev/badge/go.etcd.io/etcd/v3.svg)](https://pkg.go.dev/go.etcd.io/etcd/v3)
[![Docs](https://img.shields.io/badge/docs-latest-green.svg)](https://etcd.io/docs)
[![Releases](https://img.shields.io/github/release/etcd-io/etcd/all.svg?style=flat-square)](https://github.com/etcd-io/etcd/releases)
[![LICENSE](https://img.shields.io/github/license/etcd-io/etcd.svg?style=flat-square)](./LICENSE)

![etcd Logo](logos/etcd-horizontal-color.svg)

etcd is a distributed reliable key-value store for the most critical data of a
distributed system, with a focus on being:

* *Simple*: well-defined, user-facing API (gRPC)
* *Secure*: automatic TLS with optional client cert authentication
* *Fast*: benchmarked 10,000 writes/sec
* *Reliable*: properly distributed using Raft

etcd is written in Go and uses the [Raft][raft] consensus algorithm to manage a
highly-available replicated log. It is used [in production by many
companies](./ADOPTERS.md), and is frequently teamed with applications such as
[Kubernetes][k8s] and many others. Reliability is further ensured by
[**rigorous testing**](./functional).

This tree tracks the `3.5.0-pre` development line (see [version/version.go](./version/version.go)).

> **Note**: the development branch may be in an *unstable or even broken state*
> during development. Use a published [release][github-release] to get stable
> binaries.

See [etcdctl](./etcdctl) for a simple command line client.

[raft]: https://raft.github.io/
[k8s]: https://kubernetes.io/
[github-release]: https://github.com/etcd-io/etcd/releases

## Getting started

### Installing etcd

The easiest way to get etcd is to use one of the pre-built release binaries,
which are available for macOS, Linux, Windows, and Docker on the
[release page][github-release]. For more installation guides, see
[operating etcd](https://etcd.io/docs/latest/op-guide).

To build from this source tree instead, install [Go](https://golang.org/) —
`go.mod` declares Go 1.14 as the minimum supported version — and run:

```bash
./build
```

or, equivalently, via the [Makefile](./Makefile), which also prints the
versions of the binaries it produced:

```bash
make build
```

Both write `etcd` and `etcdctl` into `./bin`. Bug fixes land on the development
branch first and are subsequently ported to release branches, as described in
the [branch management](./Documentation/branch-management.md) guide. See
[Documentation/dl-build.md](./Documentation/dl-build.md) for more build details.

### Running etcd

Start a single-member cluster of etcd.

If etcd was installed from the [pre-built release binaries][github-release],
run it from the installation location:

```bash
/tmp/etcd-download-test/etcd
```

The command can be run as just `etcd` once the binary is on the system path:

```bash
mv /tmp/etcd-download-test/etcd /usr/local/bin/
etcd
```

If etcd was built from source as above, run it from `./bin`:

```bash
./bin/etcd
```

This brings up etcd listening on port 2379 for client communication and on port
2380 for server-to-server communication.

Next, set a single key and then retrieve it:

```console
$ etcdctl put mykey "this is awesome"
OK
$ etcdctl get mykey
mykey
this is awesome
```

etcd is now running and serving client requests. For more, check out the
[animated quick demo](https://etcd.io/docs/latest/demo).

### etcd TCP ports

The [official etcd ports][iana-ports] are 2379 for client requests and 2380 for
peer communication.

[iana-ports]: https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.txt

### Running a local etcd cluster

First install [goreman](https://github.com/mattn/goreman), which manages
Procfile-based applications.

The [Procfile](./Procfile) in this repository sets up a local example cluster.
Build the binaries first (the Procfile refers to `bin/etcd`), then start it:

```bash
make build
goreman start
```

This brings up three etcd members, `infra1`, `infra2`, and `infra3`, which
together compose a cluster. Every member accepts key value reads and writes.
The Procfile also contains a commented-out `grpc-proxy` entry that can be
enabled if a proxy is wanted.

Follow the steps in [Procfile.learner](./Procfile.learner) to add a learner
node to the cluster. Start the learner node with:

```bash
goreman -f ./Procfile.learner start
```

## Documentation

The rendered documentation lives at [etcd.io/docs](https://etcd.io/docs); its
source files are in [Documentation/](./Documentation).

- Read the full [documentation](https://etcd.io/docs/latest).
- Explore the gRPC [API reference](./Documentation/dev-guide/api_reference_v3.md).
- Set up a [multi-machine cluster](./Documentation/op-guide/clustering.md).
- Learn the [config format, env variables and flags](./Documentation/op-guide/configuration.md).
- Find [language bindings and tools](./Documentation/integrations.md).
- Use TLS to [secure an etcd cluster](./Documentation/op-guide/security.md).
- [Tune etcd](./Documentation/tuning.md).
- Understand the design in [Documentation/learning](./Documentation/learning).

## Contact

- Mailing list: [etcd-dev](https://groups.google.com/g/etcd-dev)
- Questions and discussion: [GitHub Discussions](https://github.com/etcd-io/etcd/discussions)
- Bugs and feature requests: [GitHub issues](https://github.com/etcd-io/etcd/issues)
- Planning: [milestones](https://github.com/etcd-io/etcd/milestones), [roadmap](./ROADMAP.md)

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for details on submitting patches and
the contribution workflow, and [code-of-conduct.md](./code-of-conduct.md) for
the community expectations. Project roles and decision making are described in
[GOVERNANCE.md](./GOVERNANCE.md); current maintainers are listed in
[MAINTAINERS](./MAINTAINERS).

Issues and pull requests are managed according to the
[issue triage guidelines](./Documentation/triage/issues.md) and
[PR management guidelines](./Documentation/triage/PRs.md).

## Reporting bugs

See [reporting bugs](./Documentation/reporting-bugs.md) for details about
reporting any issues.

## Reporting a security vulnerability

See the [security disclosure and release process](./security/README.md) for
details on how to report a security vulnerability and how the etcd team manages
it.

## etcd emeritus maintainers

These emeritus maintainers dedicated a part of their career to etcd and reviewed
code, triaged bugs, and pushed the project forward over a substantial period of
time. Their contribution is greatly appreciated.

* Fanmin Shi
* Anthony Romano

## License

etcd is under the Apache 2.0 license. See the [LICENSE](./LICENSE) file for
details.
