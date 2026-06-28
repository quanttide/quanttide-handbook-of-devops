# 发布管理

`qtcloud-devops` 是量潮 DevOps 命令行工具，提供发布管理和组件同步功能。

## 发布状态

查看当前项目的发布状态：

```bash
qtcloud-devops release status
```

输出包括最新标签、未发布提交数、CHANGELOG 状态、GitHub Release 同步情况、配置文件版本一致性。

## 发布版本

```bash
# 发布正式版（自动更新 Cargo.toml/pyproject.toml 版本号、生成 CHANGELOG）
qtcloud-devops release publish -v cli/v0.6.1 -y

# 发布预发布版（版本号含 -rc、-alpha、-beta 后缀）
qtcloud-devops release publish -v cli/v0.7.0-rc.1 -y
```

发布流程：
1. 自动更新 `Cargo.toml`/`pyproject.toml` 版本号
2. 自动生成 CHANGELOG 条目（基于 git 提交记录，LLM 协助）
3. 提交修改 → 创建标签 → 推送到远端 → 创建 GitHub Release

## 组件同步

```bash
# 查看子模块状态
qtcloud-devops code status

# 同步子模块（fetch + rebase + push）
qtcloud-devops code sync [name]
```

## 发布前检查

每次操作开始和结束时执行 `release status`，确认：
- 配置文件版本与 tag 一致
- CHANGELOG 条目存在
- GitHub Release body 与 CHANGELOG 同步
- 工作区干净
