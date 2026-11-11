# 上游来源与维护

## 初始化基线

- 项目：MinIO
- 上游仓库：https://github.com/minio/minio.git
- 来源：工作区相邻的 `minio/` 本地仓库的 `master` 分支
- 基线提交：`7aac2a2c5b7c882e68c1ce017d8256be2feea27f`
- 初始化日期：2026-11-11
- vobstore 主分支：`main`
- vobstore 远程仓库：`git@github.com:vectordb-io/vobstore.git`

基线固定为初始化时本地 `master` 指向的提交，没有将远程的新提交自动纳入。
本项目仅导入该提交的代码快照，不保留 MinIO 的 Git 历史、分支或发行标签。
vobstore 的历史从本仓库自己的初始提交开始，后续变更在本仓库独立提交。

## 后续维护

在工作区干净时从本项目主分支创建开发分支：

```sh
git switch main
git pull --ff-only origin main
git switch -c feature/change-name
# 修改代码并运行相关测试。
git add <已检查的文件>
git commit -m "feat: describe the change"
git push -u origin HEAD
```

需要参考上游修复时，在独立目录查看 MinIO 代码和对应补丁，按需移植到 vobstore，
验证后作为本项目自己的提交记录。不要将上游完整历史合并到本项目。
移植时记录对应上游提交，保留相关版权声明。

## 维护边界

本次初始化没有修改运行时 Go 源码、依赖、存储格式或协议。

- 保留上游许可证、版权声明和作者署名文件。
- 首先维持可编译、可测试的代码基线，再分步调整命名、发布与功能。
- `go.mod` 和内部导入路径暂时保持上游值，便于核对原始代码。
- 上游 GitHub 配置存放于 `docs/upstream/github/`，作为维护参考；
  vobstore 使用自己的 `.github/` 配置与基础 CI。
- 现有发布脚本仍指向 MinIO 的发布基础设施，正式发布前须单独适配。
