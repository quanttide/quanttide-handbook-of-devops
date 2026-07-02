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
