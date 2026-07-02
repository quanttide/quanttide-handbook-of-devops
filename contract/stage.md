# Stage 配置

时序维度，定义价值流的节拍。

## 字段

```yaml
stages:
  build:
    command: cargo build --release     # 构建命令（可选）
  test:
    command: cargo test                # 测试命令（可选）
    threshold: 80.0                    # 覆盖率阈值，默认 70.0
  release:
    changelog: CHANGELOG.md           # CHANGELOG 文件路径，默认 CHANGELOG.md
    pre_publish:                       # 发布前执行的命令列表（可选）
      - cargo publish
```

## 默认值

```yaml
stages:
  build:
    command: null
  test:
    command: null
    threshold: 70.0
  release:
    changelog: CHANGELOG.md
    pre_publish: []
```
