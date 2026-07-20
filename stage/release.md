# 发布

## 版本

以 Git tag 为事实源。统一使用 `v` 前缀（如 `v0.1.0`、`v1.0.0-rc.1`）。

版本号格式：

- `vX.Y.Z` — 正式版本
- `vX.Y.Z-prerelease` — 预发布版本（如 `-rc.1`、`-alpha.2`）
- `scope/vX.Y.Z` — 带作用域前缀（如 `cli/v0.7.0`）
- `scope/vX.Y.Z-prerelease` — 带作用域的预发布版本

## 流程

### 发布前检查

```bash
# 构建状态
qtcloud-devops build status

# 测试状态
qtcloud-devops test status

# 发布状态（最新 tag、版本一致性、CHANGELOG）
qtcloud-devops release status
```

确认版本一致、CHANGELOG 已更新、工作区干净后，进行发布。

### 预发布版本（rc）

发布 rc 版本用于 CI 验证：

```bash
# 通过 CI 发布（默认）
qtcloud-devops release publish -v cli/v0.3.2-rc.1 -y

# 本地直接发布
qtcloud-devops release publish -v cli/v0.3.2-rc.1 -y --local
```

CI 验证通过后，检察发布状态确认一致，再发布正式版本。

### 正式发布

```bash
# 通过 CI 发布（默认）：创建 tag → CI build-cli → CI publish-cli
qtcloud-devops release publish -v cli/v0.3.2 -y

# 本地直接发布：本地执行 cargo publish + uv publish
qtcloud-devops release publish -v cli/v0.3.2 -y --local
```

发布流程（`--remote` / 默认）：

1. 校验版本号格式
2. 校验所有配置文件版本号一致
3. 自动更新 `Cargo.toml`/`pyproject.toml` 版本号
4. 自动生成 CHANGELOG 条目（基于 git 提交记录，LLM 协助）
5. 校验 CHANGELOG 包含对应版本记录
6. 创建标签 → 推送到远端 → 创建 GitHub Release
7. CI 自动发布到 crates.io / PyPI

发布流程（`--local`）：

1. 校验版本号格式
2. 校验所有配置文件版本号一致
3. 自动更新 `Cargo.toml`/`pyproject.toml` 版本号
4. 自动生成 CHANGELOG 条目（基于 git 提交记录，LLM 协助）
5. 校验 CHANGELOG 包含对应版本记录
6. 校验本地凭证（`CARGO_REGISTRY_TOKEN` / `UV_PUBLISH_TOKEN`）
7. 创建标签 → 推送到远端 → 创建 GitHub Release
8. 本地执行 `cargo publish` → crates.io
9. 本地执行 `uv publish` → PyPI

### 发布后检查

```bash
qtcloud-devops release status
```

确认最新标签已更新、CHANGELOG 与 GitHub Release 一致。

## 验收

- 配置文件与 Git tag 的版本号保持一致。
- CHANGELOG 与 GitHub Release 存在并保持一致。
- CI 构建全部通过。
