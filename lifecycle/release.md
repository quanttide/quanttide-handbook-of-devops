# 发布

## 版本

以Git tag为事实源。统一使用 `v` 前缀（如 `v0.1.0`、`v1.0.0-rc.1`）。

正则表达式规则：

```regex
# 正式版本
^refs/tags/v(?:0|[1-9]\d*)\.(?:0|[1-9]\d*)\.(?:0|[1-9]\d*)$

# 含预发布后缀
^refs/tags/v(?:0|[1-9]\d*)\.(?:0|[1-9]\d*)\.(?:0|[1-9]\d*)(?:-(?:alpha|beta|rc)\.(?:0|[1-9]\d*))?$
```

## 流程

### 检查发布前状态

```bash
qtcloud-devops release status
```

输出包括最新标签、未发布提交数、CHANGELOG 状态、GitHub Release 同步情况、配置文件版本一致性。

### 发布版本

```bash
qtcloud-devops release publish -v cli/v0.6.1 -y
```

发布流程：
1. 自动更新 `Cargo.toml`/`pyproject.toml` 版本号
2. 自动生成 CHANGELOG 条目（基于 git 提交记录，LLM 协助）
3. 提交修改 → 创建标签 → 推送到远端 → 创建 GitHub Release

### 检查发布后状态

```bash
qtcloud-devops release status
```

## 验收

- 配置文件与Git tag的版本号保持一致。
- CHANGELOG与GitHub Release存在并保持一致。
