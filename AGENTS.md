# AGENTS.md

Guidance for coding agents working in this repository.

## Project overview

etcd is a distributed, strongly-consistent key-value store built on the Raft
consensus algorithm.

This repository is a mirror of [etcd-io/etcd](https://github.com/etcd-io/etcd)
hosted under the `sourcegraph-testing` organization and used to exercise tooling
and automation. Upstream is the source of truth.

- Module path: `go.etcd.io/etcd/v3`
- Go version: `go 1.14` per `go.mod`; CI builds against Go 1.13.3 and tip
- `version/version.go` reports `3.5.0-pre` — a master-series pre-release, not a
  tagged version. This mirror carries no release tags.

`go.mod` contains a `replace` directive pointing `go.uber.org/zap` at
`github.com/sourcegraph-testing/zap v1.14.1`. That is deliberate for this mirror;
do not "clean it up".

## Repository layout

| Directory | Role |
| --- | --- |
| `etcdserver/`, `mvcc/`, `wal/`, `lease/`, `auth/` | Core server, storage, write-ahead log, leases, auth |
| `raft/` | Raft consensus implementation |
| `etcdctl/`, `etcdmain/` | CLI client and server entrypoint |
| `client/`, `clientv3/` | Go client libraries |
| `embed/` | Embeddable etcd server package |
| `proxy/`, `contrib/` | gRPC proxy and contrib integrations |
| `integration/`, `tests/`, `functional/` | Integration, e2e, and fault-injection tests |
| `scripts/`, `hack/`, `tools/` | Build and release scripts, dev helpers |
| `Documentation/` | User and developer documentation |

## Build

```bash
make build
```

Runs `GO_BUILD_FLAGS="-v" ./build`, then sanity-checks `./bin/etcd --version` and
`./bin/etcdctl version`. The `./build` script can also be invoked directly.

## Test

```bash
make test
```

Runs `TEST_OPTS='PASSES=unit' ./test` and fails if the log contains `FAIL`,
`DATA RACE`, a timeout, or a leak.

The `./test` script is the entry point for every test pass. Select passes with
`PASSES`; it also honours `PKG`, `TESTCASE`, `CPU`, `TIMEOUT`, and `COVERDIR`.

```bash
PASSES='unit' ./test
PASSES='integration' ./test
PKG=./mvcc/... PASSES='unit' ./test
```

Available passes: `fmt`, `bom`, `dep`, `build`, `unit`, `integration`,
`functional`, `release`, `grpcproxy`, `build_cov`, `cov`. With `PASSES` unset the
default is `fmt bom dep build unit`.

Docker-based variants exist as `make docker-test`, `make docker-test-coverage`,
and several `docker-*-test` targets covering DNS, static-IP, and gRPC-proxy
scenarios.

## Lint and format

There is no `.golangci.yml`. Lint runs as sub-passes of the `fmt` pass:

```bash
PASSES='fmt' ./test
```

That covers `gofmt -l -s -d`, `go vet`, `revive -config ./tests/revive.toml`,
license-header and receiver-name checks, commit-title validation, `shellcheck`,
and prose checks.

Note two traps:

- `gofmt` is only *checked*, never applied. Run `gofmt -s -w` on files you touch.
- `staticcheck`, `unparam`, `unconvert`, `ineffassign`, `nakedret`, and shadow
  vet run only when their binaries are on `PATH`, and are silently skipped
  otherwise. A clean local run does not imply a clean CI run.

## Conventions

Code style follows the
[Go Code Review Comments](https://github.com/golang/go/wiki/CodeReviewComments)
wiki.

Commit messages are mechanically validated by the `commit_title_pass` check,
which rejects any title without a `<package>:` prefix:

```text
<package>{, <package>}: <what changed>

<why this change was made>

<footer>
```

Subject line 70 characters or fewer, body wrapped at 80. A change spanning
several packages uses a `*:` prefix.

## Gotchas

- CI is Travis (`.travis.yml`). There are no GitHub Actions build workflows —
  `.github/` holds only issue/PR templates and stale-bot config.
- `vendor/` is excluded from lint passes; do not hand-edit it.
- Integration and functional suites are slow and resource-hungry. Prefer
  `PASSES='unit' ./test` narrowed with `PKG=` while iterating.
- `CONTRIBUTING.md` describes the upstream etcd-io contribution flow, including
  two-maintainer LGTM. That process governs upstream, not this mirror.
