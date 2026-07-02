# 测试

遵循持续测试原则。每次提交应通过本地测试。

## 查看测试状态

```bash
qtcloud-devops test status
```

输出包括：

- 测试通过数 / 失败数 / 跳过数
- 覆盖率百分比
- 覆盖率阈值（来自契约配置 `stages.test.threshold`）
