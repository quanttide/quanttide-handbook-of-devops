# Git 使用规范

本文档定义 Git 提交和版本控制的使用规范，与 `code` 命令的子模块操作流程（见[开发](code.md)）配合使用。

## 提交规范

遵循 [Conventional Commits](https://www.conventionalcommits.org/)，格式：

```
<type>: <description>
```

### 提交类型

| 类型 | 说明 | 示例 |
|:-----|:-----|:-----|
| feat | 新功能 | feat: add user authentication |
| fix | 修复 bug | fix: resolve null pointer exception |
| docs | 文档更新 | docs: update README |
| test | 测试相关 | test: add unit tests for api |
| refactor | 代码重构 | refactor: simplify logic |
| chore | 构建/工具 | chore: update dependencies |

## 分支规范

- `main`：主分支，稳定版本
- `feature/xxx`：功能分支
- `fix/xxx`：修复分支
- `release/xxx`：发布分支

## 子模块维护

日常同步使用 `qtcloud-devops code sync`（见[开发](code.md)）；手动操作流程：

1. 进入子模块提交并推送
2. 返回主仓库更新子模块引用
3. 提交并推送主仓库

## 最佳实践

1. 原子提交：每次提交独立完整
2. 规范格式：遵循 Conventional Commits
3. 及时推送：本地提交后尽快推送
4. 拉取更新：推送前先拉取远程
5. 子模块同步：更新后立即同步父仓库
6. 版本标记：发布时使用语义化版本号（见[发布](release/index.md)）
7. CHANGELOG 维护：每次发布更新 CHANGELOG.md

## 参考文档

- [Conventional Commits](https://www.conventionalcommits.org/)
- [Git 官方文档](https://git-scm.com/doc)
