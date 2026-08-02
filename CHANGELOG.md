# CHANGELOG

## [Unreleased]

### Added
- 新增发布审计文档，覆盖自动审计、手动审计、发布前后清单和管理员/AI 决策方法。
- 新增发布章节索引，将发布执行与发布审计分开。

### Changed
- 将原 `stage/release.md` 拆分为 `stage/release/publish.md` 和 `stage/release/audit.md`，按 CLI 发布与手动操作重新组织发布流程。

## [0.2.1] - 2026-07-02

### Added
- 在发布文档中添加版本部分和验收标准

### Changed
- 简化发布文档的标题，移除介绍段落

## [0.2.0] - 2026-06-29

### Added
- 新增 `contract/scope.md` 文档，说明 Scope 配置
- 新增 `lifecycle` 章节，将原有流程重组为单一目录结构
- 添加 `code` 命令相关文档，补充操作流程说明

### Changed
- 优化文档组织结构：将 `releasing/` 降级为教程，并将生命周期文件改为动词命名（plan/code/build/test/release/deploy/operate/monitor）
- 统一操作流程风格：`code.md` 与 `release.md` 对齐，采用 status → sync → status 流程
- 合并发布命令为单一示例，精简文档内容
- 将 `lifecycle/README.md` 重命名为 `lifecycle/index.md`

### Fixed
- 修复 `release.md` 中错误的 code status 表述

### Removed
- 移除空的 `general/` 目录
- 移除废弃的 `repo_naming` 文档（归档到 `data/history`）
- 移除空的“工具”章节（`git/README.md` 仅标题）
- 移除空的“流程”章节，待有内容后再补充

## [0.1.0] - 2026-06-29

### Added
- 添加 MyST Markdown 配置（myst.yml）
- 增加仓库命名规则

### Changed
- 升级目录结构
- 重构项目结构（语义化版本相关）
- 修改非公开版本的定义

### Fixed
- 修复 private_versions.md 中分割线重复异常
- 修复 _toc.yml 目录初始化问题

### Removed
- 移除 Jupyter Book 配置（由 myst.yml 替代）
- 移除 GitHub Actions 工作流（移入主仓库管理）
