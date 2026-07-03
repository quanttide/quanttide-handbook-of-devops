# 计划

## 定位

`plan` 阶段承接 lifecycle 的起点：**确定要做什么、做了多少、还有什么没做**。

由 `qtcloud-devops plan` 命令管理 scope 级规划文件（`<scope-dir>/ROADMAP.md`），通过 AI 在工作流中持续读写维护。

## 命令

```bash
# 查看当前 scope 的规划进度
qtcloud-devops plan status

# 查看指定 scope 的规划进度
qtcloud-devops plan status <scope>

# 删除 scope 中已完成的条目
qtcloud-devops plan clean [scope]

# 验证 scope 格式问题（只读，修复由 LLM 完成）
qtcloud-devops plan doctor [scope]
```

`scope` 省略时自动检测当前目录所属 scope。

## 工作流

```
plan status    → 了解当前版本还差什么
plan doctor    → 验证 ROADMAP 格式是否合规
plan clean     → 提交前清理已完成条目，保持 ROADMAP 整洁
     ↓
code / build / test / release     ← 进入后续生命周期
```

### 规划文件格式

ROADMAP.md 位于每个 scope 的根目录，格式约定：

```markdown
# ROADMAP

> 格式：Keep a Changelog + checkbox 任务清单。

## [0.2.0]

### Added
- [ ] feature a
- [ ] feature b

### Fixed
- [x] bug fix

## [0.1.0]

### Added
- [x] release plan
- [x] test coverage
```

语法规则：

- `## [X.Y.Z]` — 版本标题，可选后缀如 `— 已发布`
- `### Added / Changed / Fixed / Removed / Deprecated / Security` — 分类标题
- `- [x] xxx` — 已完成条目
- `- [ ] xxx` — 未完成条目

### plan status 输出示例

```text
[cli] 规划进度
────────────────────────────────────────
  [0.2.0]      1/ 4 完成 (25%)
  [0.1.0]      8/ 8 完成 (100%)
────────────────────────────────────────
  总计:  9/12 完成 (75%)
```

## 验收

- scope 根目录存在 `ROADMAP.md`
- `plan status` 正确反映 checkbox 完成比例
- `plan clean` 正确删除 `[x]` 条目及空分类
- `plan doctor` 正确检出 v 前缀/分类大小写/checkbox 格式问题
