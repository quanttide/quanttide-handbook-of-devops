# 构建

遵循持续集成原则。每次推送自动触发 CI 构建。

## 查看构建状态

```bash
qtcloud-devops build status
```

输出包括：

- 编程语言
- CI 最新运行记录（workflow 名称与状态）
- 语法检查结果
- 版本一致性（与最新 tag 比对）
- 制品库配置
- CHANGELOG 路径

## 依赖管理

依赖优先级：**crates.io > GitHub 仓库引用 > 本地 path**。

| 优先级 | 来源 | 场景 |
|--------|------|------|
| 1 | crates.io | 所有外部依赖默认来源 |
| 2 | GitHub (git =) | 官方未发布到 crates.io、需使用未发布版本 |
| 3 | 本地 path | 开发中本地联调 |

规则：
- 对外发布的 crate **禁止**含有 path 依赖
- CI 环境只识别 crates.io 来源，path 依赖会导致编译失败
- GitHub 引用需锁定 rev（commit hash），不跟踪分支
