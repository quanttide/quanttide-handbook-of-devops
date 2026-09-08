# 发布执行

发布执行有两条路径：CLI 发布和手动操作。默认路径是 CLI 发布；手动操作只用于项目尚未接入工具、工具暂不支持目标平台、异常修复，或负责人明确批准的情况。

## 版本与标签

以 Git tag 为版本事实源。统一使用 `v` 前缀：

- `vX.Y.Z` — 单组件正式版本
- `vX.Y.Z-prerelease` — 单组件预发布版本，如 `v1.0.0-rc.1`
- `scope/vX.Y.Z` — 多组件正式版本，如 `cli/v0.7.0`
- `scope/vX.Y.Z-prerelease` — 多组件预发布版本，如 `cli/v0.7.0-rc.1`

发布前应确认配置文件版本、CHANGELOG 条目和目标 tag 一致。tag 一经推送即视为不可变锚点，不应通过移动 tag 修复普通发布问题。

## CHANGELOG 规范

- Markdown 格式，每个版本独立章节，日期标注 `YYYY-MM-DD`
- 分类标签：Added、Changed、Fixed、Removed；条目动词开头（新增 / 修改 / 修复 / 移除）
- 简洁明了，每条一行，按重要性排序

示例：

```markdown
## [v0.0.3] - 2026-04-08

### Added
- 新增写作格式规范

### Fixed
- 合并冗余文档
```

## GitHub Release 规范

- 正式发布使用附注标签：`git tag -a v1.0.0 -m "Release version 1.0.0"`；轻量标签仅用于临时标记
- GitHub Release 的 Tag Name 与 Git tag 完全一致（CLI 发布由 `release publish` 自动创建）
- Release Notes 从 CHANGELOG 对应版本章节提取，使用 Added / Changed / Fixed / Removed 分类
- 最新稳定版本标记 Latest，预发布版本（alpha / beta / rc）标记 Pre-release

## 发布目标

发布目标由项目契约、scope 配置、CI workflow 和版本计划共同决定，不能只按语言推断。

| 目标 | 常见制品 | 常见入口 |
|------|----------|----------|
| GitHub Release | release notes、源码归档、二进制包、安装包 | `gh release` 或发布 CI |
| crates.io | Rust crate | `cargo publish` 或发布 CI |
| PyPI | Python package | `uv publish` / `twine upload` 或发布 CI |
| npm | JavaScript / TypeScript package | `npm publish` 或发布 CI |
| pub.dev | Dart / Flutter package | `flutter pub publish` 或发布 CI |
| 容器镜像仓库 | Docker / OCI image | `docker push` 或发布 CI |
| 内部制品库 | 内部二进制、数据包、模型、文档包 | 项目约定上传流程 |
| 无外部分发 | 仅版本记录和文档发布 | tag、CHANGELOG、Release 可选 |

## CLI 发布

项目接入 `qtcloud-devops` 时，使用 CLI 作为发布入口。CLI 的职责是把版本校验、CHANGELOG 校验、tag 创建、GitHub Release、CI 触发和制品发布串成一个可审计流程。

发布前查看状态：

```bash
qtcloud-devops build status
qtcloud-devops test status
qtcloud-devops release status
```

发布前审计：

```bash
qtcloud-devops release audit -v cli/v0.3.2 --scope cli
```

预发布版本用于 CI 验证：

```bash
qtcloud-devops release publish -v cli/v0.3.2-rc.1 --registry crates -y
```

正式发布：

```bash
qtcloud-devops release publish -v cli/v0.3.2 --registry crates -y
```

本地直接发布（`--local`），用于本机缺少发布 CI 或需要人工确认发布结果的情况：

```bash
qtcloud-devops release publish -v cli/v0.3.2 -y --local
```

`--local` 路径的发布流程：校验版本号和 tag 格式 → 校验配置文件版本一致 → 自动更新配置文件版本号 → 自动生成 CHANGELOG 条目（基于 git 提交记录，LLM 协助）→ 校验 CHANGELOG 包含对应版本记录 → 校验本地凭证（`CARGO_REGISTRY_TOKEN` / `UV_PUBLISH_TOKEN`）→ 创建并推送 tag、创建 GitHub Release → 本机执行 `cargo publish`（crates.io）与 `uv publish`（PyPI）。

默认 CLI 发布流程：

1. 校验版本号和 tag 格式。
2. 校验 scope、配置文件版本和 CHANGELOG。
3. 确认工作区和目标提交状态。
4. 创建并推送 tag。
5. 触发发布 CI。
6. 由 CI 发布到声明的目标，例如 GitHub Release、crates.io、PyPI、npm、pub.dev、容器镜像仓库或内部制品库。
7. 执行发布后审计，确认所有目标可访问。

> 已接入 CLI/CI 的项目，禁止绕过工具手动执行 `cargo publish`、`uv publish`、`npm publish`、`flutter pub publish`、`docker push` 或手工移动 tag。
>
> 发布 CI（workflow `release-cli`）由推送 `scope/*` tag 触发（如 `cli/v0.3.2`），不依赖手动创建 GitHub Release，tag 即版本事实源。
>
> CI 使用的发布凭证（如 `CRATES_API_TOKEN`、`PYPI_API_TOKEN`）应配置在组织（organization）或仓库（repository）级 secrets，不得写入仓库；本地发布则使用本机凭证（如 `CARGO_REGISTRY_TOKEN`、`UV_PUBLISH_TOKEN`）。

## 手动操作

手动操作不是默认路径。以下命令示例仅适用于未接入 `qtcloud-devops`/CI 发布流程的项目；已接入项目即使负责人批准，也应优先使用 CLI/CI，仅在工具不可用或异常修复时允许手动执行。进入手动路径前，先记录原因：

- 项目没有接入 `qtcloud-devops`。
- 发布工具尚不支持目标制品库或目标平台。
- 需要补建缺失的 GitHub Release、二进制资产或内部制品记录。
- 工具不可用或异常修复场景下，负责人明确批准一次性人工发布。

手动发布的最小流程：

1. 运行发布前审计，确认版本、CHANGELOG、构建、测试和安全检查通过。
2. 从 `main` 或项目约定的发布分支执行发布。
3. 创建 tag 并推送到远端。
4. 创建 GitHub Release，正文从 CHANGELOG 对应版本派生。
5. 按发布目标上传制品。
6. 运行发布后审计并记录证据。

常见手动命令示例：

```bash
git tag cli/v0.3.2
git push origin cli/v0.3.2
gh release create cli/v0.3.2 --title "cli/v0.3.2" --notes-file release-notes.md
```

不同目标的手动发布命令应按项目配置选择：

```bash
cargo publish --locked --registry crates-io
uv publish
npm publish
flutter pub publish
docker push ghcr.io/<org>/<image>:<version>
```

手动命令只表示操作入口，不代表可以跳过审计。执行前后都要保留命令、CI、制品页面或下载地址等证据。

## 发布后处理

发布完成后，不直接用“命令执行完”作为成功标准。应转到 [发布审计](audit.md)，确认：

- tag 已推送且指向正确提交。
- GitHub Release 存在，正文与 CHANGELOG 一致。
- 对应制品库版本可查询。
- 二进制资产、容器镜像或内部制品可下载。
- CI/CD release job 成功。

