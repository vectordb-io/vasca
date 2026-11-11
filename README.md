# vobstore

**English | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | [Español](README.es.md) | [Português do Brasil](README.pt-BR.md)**

vobstore is an S3-compatible object storage project based on MinIO, maintained independently in this repository.

The initial code comes from the `master` branch of the local `minio/` repository at commit
[`7aac2a2c5b7c882e68c1ce017d8256be2feea27f`](https://github.com/minio/minio/commit/7aac2a2c5b7c882e68c1ce017d8256be2feea27f).
Only the code snapshot was imported; this repository does not include MinIO's Git commit history.
See [UPSTREAM.md](UPSTREAM.md) (in Simplified Chinese) for provenance and the maintenance workflow.

## Build and run

Requires Git and Go 1.24.8, matching the toolchain version in `go.mod`.

```sh
git clone git@github.com:vectordb-io/vobstore.git
cd vobstore
go build -tags kqueue -trimpath -o vobstore .
./vobstore server ./data --address 127.0.0.1:9000 --console-address 127.0.0.1:9001
```

Once started, the S3 API is available at `http://127.0.0.1:9000` and the console at
`http://127.0.0.1:9001`. The default credentials for local development are `minioadmin:minioadmin`.
Set your own credentials through `MINIO_ROOT_USER` and `MINIO_ROOT_PASSWORD` when deploying.

## Verification

```sh
go build -tags kqueue -trimpath -o vobstore .
go test -tags kqueue,dev ./internal/...
```

The baseline CI runs these checks on pushes and pull requests targeting `main`.
Changes to server behavior should also run the relevant `cmd` tests and upstream integration tests;
see `Makefile` for integration test dependencies and entry points.

## Initialization scope

This initialization preserves the upstream Go module path `github.com/minio/minio`, internal import paths,
`MINIO_*` configuration, data formats, and runtime behavior. The executable can be named `vobstore`,
while runtime help and version information still use MinIO branding. Changes to branding, module paths,
and release workflows will be handled separately.

MinIO's original GitHub configuration is kept in [docs/upstream/github](docs/upstream/github/)
as a maintenance reference; vobstore uses its own [CI workflow](.github/workflows/ci.yml).
The original `Makefile` and release scripts still contain MinIO image registries, download URLs,
and release destinations. Configure this project's destinations before using release targets.

## License and provenance

The code remains licensed under [GNU AGPL v3](LICENSE), preserving the upstream [NOTICE](NOTICE),
[CREDITS](CREDITS), and copyright notices in source files. vobstore is an independently maintained project.

See the [README at the baseline commit](https://github.com/minio/minio/blob/7aac2a2c5b7c882e68c1ce017d8256be2feea27f/README.md)
for the original upstream documentation.
