# 计划

## 规划文件

ROADMAP.md 位于每个 scope 的根目录，使用 Keep a Changelog 风格 + checkbox 任务清单管理 scope 级规划。

格式约定：

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

## 流程

### 查看进度

```bash
# 查看当前 scope 的规划进度
qtcloud-devops plan status

# 查看指定 scope 的规划进度
qtcloud-devops plan status <scope>
```

`scope` 省略时自动检测当前目录所属 scope。

输出示例：

```text
[cli] 规划进度
────────────────────────────────────────
  [0.2.0]      1/ 4 完成 (25%)
  [0.1.0]      8/ 8 完成 (100%)
────────────────────────────────────────
  总计:  9/12 完成 (75%)
```

### 验证格式

```bash
qtcloud-devops plan doctor [scope]
```

只读检查，修复由 LLM 完成。检出的问题包括 v 前缀、分类大小写、checkbox 格式等。

### 清理条目

```bash
qtcloud-devops plan clean [scope]
```

提交前清理已完成条目，保持 ROADMAP 整洁。

## 验收

- scope 根目录存在 `ROADMAP.md`
- `plan status` 正确反映 checkbox 完成比例
- `plan clean` 正确删除 `[x]` 条目及空分类
- `plan doctor` 正确检出 v 前缀/分类大小写/checkbox 格式问题
