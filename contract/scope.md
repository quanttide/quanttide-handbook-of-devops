# Scope 配置

上下文维度，将不同配置组合挂载到具体组件上。

## 字段

```yaml
scopes:
  cli:
    dir: src/cli                      # 子目录路径（必填）
    language: rust                    # 编程语言
    framework: axum                   # 框架（可选）
    build_tool: cargo                 # 构建工具
    registry: crates                  # 制品库（覆盖全局 platform.artifact_registry）
    release:                          # 发布配置（覆盖全局 stages.release）
      changelog: CHANGELOG.md
      pre_publish:
        - cargo publish
    test_threshold: 90.0              # 测试阈值（覆盖全局 stages.test.threshold）
    ci_workflow: ci.yml               # CI workflow 文件名（可选）
```

## 枚举值

### Language

| 值 | 说明 |
|----|------|
| `rust` | Rust |
| `python` | Python |
| `go` | Go |
| `dart` | Dart/Flutter |
| `typescript` / `ts` / `node` | TypeScript/Node.js |

### BuildTool

| 值 | 说明 |
|----|------|
| `cargo` | Cargo |
| `uv` | uv（poetry/pdm 也映射为此） |
| `go` | Go |
| `flutter` | Flutter |
| `npm` | npm（pnpm/yarn/bun 也映射为此） |

### Registry

同 [Platform 配置](platform.md#registry)，scope 级别的 `registry` 覆盖全局配置。

## 覆盖规则

scope 的配置可以覆盖全局 Stage / Stage 中的对应字段：

| Scope 字段 | 覆盖目标 |
|-----------|---------|
| `registry` | `platform.artifact_registry` |
| `release` | `stages.release` |
| `test_threshold` | `stages.test.threshold` |
| `ci_workflow` | 不覆盖，仅在构建状态展示 |

## 默认值

YAML 中未声明的字段使用以下默认值：

| 字段 | 默认值 |
|------|--------|
| `language` | `auto`（自动检测） |
| `framework` | `""`（空字符串） |
| `build_tool` | `auto`（自动检测） |
| `registry` | `none` |
| `release` | `stages.release` 的全局默认值 |
| `test_threshold` | `stages.test.threshold` 的全局值 |
| `ci_workflow` | 按 `build-{scope}` 约定推导 |
