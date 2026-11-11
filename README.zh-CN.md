# vobstore

**[English](README.md) | 简体中文 | [日本語](README.ja.md) | [Español](README.es.md) | [Português do Brasil](README.pt-BR.md)**

vobstore 是基于 MinIO 的 S3 兼容对象存储项目，在本仓库独立持续维护。

初始代码来自本地 `minio/` 仓库的 `master` 分支，基线提交为
[`7aac2a2c5b7c882e68c1ce017d8256be2feea27f`](https://github.com/minio/minio/commit/7aac2a2c5b7c882e68c1ce017d8256be2feea27f)。
本仓库仅导入该提交的代码快照，不包含 MinIO 的 Git 提交历史。来源与后续更新方式见 [UPSTREAM.md](UPSTREAM.md)。

## 构建和运行

需要 Git 和 Go 1.24.8（与 `go.mod` 的工具链版本一致）。

```sh
git clone git@github.com:vectordb-io/vobstore.git
cd vobstore
go build -tags kqueue -trimpath -o vobstore .
./vobstore server ./data --address 127.0.0.1:9000 --console-address 127.0.0.1:9001
```

启动后，S3 API 地址为 `http://127.0.0.1:9000`，控制台地址为
`http://127.0.0.1:9001`。本地开发默认凭据为 `minioadmin:minioadmin`；
部署时通过 `MINIO_ROOT_USER` 和 `MINIO_ROOT_PASSWORD` 设置自己的凭据。

## 验证

```sh
go build -tags kqueue -trimpath -o vobstore .
go test -tags kqueue,dev ./internal/...
```

基础 CI 在 `main` 的推送和拉取请求上执行以上检查。涉及服务端行为的修改还应运行
相关 `cmd` 测试及上游集成测试；集成测试的依赖和入口可参考 `Makefile`。

## 初始化范围

本次初始化保留上游 Go module 路径 `github.com/minio/minio`、内部导入路径、
`MINIO_*` 配置、数据格式和原有运行行为。生成的可执行文件可以命名为 `vobstore`，
运行时的命令说明和版本信息仍沿用 MinIO。后续品牌、模块路径和发布流程的调整将在独立变更中进行。

MinIO 原有 GitHub 配置保存在 [docs/upstream/github](docs/upstream/github/)，
作为维护参考；vobstore 使用自己的 [CI 工作流](.github/workflows/ci.yml)。
原有 `Makefile` 和发布脚本仍包含 MinIO 的镜像仓库、下载地址和发布目标，
使用发布相关目标前需配置本项目的目标。

## 许可证与来源

代码继续使用 [GNU AGPL v3](LICENSE)，保留上游 [NOTICE](NOTICE)、[CREDITS](CREDITS)
以及源文件中的版权声明。vobstore 是独立维护项目。

上游项目的原始说明见 [基线提交的 README](https://github.com/minio/minio/blob/7aac2a2c5b7c882e68c1ce017d8256be2feea27f/README.md)。
