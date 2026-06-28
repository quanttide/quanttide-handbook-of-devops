# 开发

使用 `code` 命令管理子模块。每次操作开始和结束执行一次 `code status`，确认操作结果。

## 检查同步前状态

```bash
qtcloud-devops code status
```

输出包括各子模块的同步状态（Synced / PendingPush / PendingPull / Conflict）、领先/落后提交数。

## 同步子模块

```bash
# 同步全部子模块
qtcloud-devops code sync

# 同步指定子模块
qtcloud-devops code sync <name>
```

同步流程：
1. 自动 fetch 远端最新提交
2. rebase 本地未发布提交
3. push 本地提交到远端
4. 更新父仓库指针并推送

## 检查同步后状态

```bash
qtcloud-devops code status
```
