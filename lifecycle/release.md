# 发布管理

使用 `release` 命令管理版本发布。每次操作开始和结束执行一次 `release status`，确认操作结果。

## 检查发布前状态

```bash
qtcloud-devops release status
```

输出包括最新标签、未发布提交数、CHANGELOG 状态、GitHub Release 同步情况、配置文件版本一致性。

## 发布版本

```bash
qtcloud-devops release publish -v cli/v0.6.1 -y
```

发布流程：
1. 自动更新 `Cargo.toml`/`pyproject.toml` 版本号
2. 自动生成 CHANGELOG 条目（基于 git 提交记录，LLM 协助）
3. 提交修改 → 创建标签 → 推送到远端 → 创建 GitHub Release

## 检查发布后状态

```bash
qtcloud-devops release status
```
