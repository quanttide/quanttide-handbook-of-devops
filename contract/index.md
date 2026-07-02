# 契约

契约（Contract）是 DevOps 治理的四维建模框架，定义在 `.quanttide/devops/contract.yaml` 中。

## 四维架构

```yaml
stages:      # 时序维度 — 何时检查什么
platforms:   # 载体维度 — 外部治理平台
sources:     # 事实源维度 — 真相的本质
scopes:      # 上下文维度 — 规则的作用范围
```

### Stages（时序维度）

定义价值流的节拍。不规定"怎么做"，只规定"什么时候检查什么"。包含构建、测试、发布三个阶段，每个阶段可配置执行命令和质量门禁。

详见 [Stage 配置](stage.md)。

### Platforms（载体维度）

定义外部治理载体，包括源代码管理平台、CI/CD pipeline、制品库。负责外部合规——平台提供"舞台"并制定入场规则。

详见 [Platform 配置](platform.md)。

### Sources（事实源维度）

定义事实源 Single Source of Truth，主要指版本号的读取来源（`Cargo.toml`、`pyproject.toml` 等）。无论换到哪个 Platform，Sources 里的真理永远不变。

详见 [Source 配置](source.md)。

### Scopes（上下文维度）

通过 scope 为不同组件挂载不同的 Stages、Platforms、Sources 组合，避免"一刀切"。每个 scope 可覆盖全局配置。

详见 [Scope 配置](scope.md)。

## 完整示例

```yaml
stages:
  build:
    command: cargo build --release
  test:
    command: cargo test
    threshold: 80.0
  release:
    changelog: CHANGELOG.md
    pre_publish:
      - cargo publish

platforms:
  source_control: github
  pipeline: github_actions
  artifact_registry: crates

sources:
  version:
    type: auto

scopes:
  cli:
    dir: src/cli
    language: rust
    build_tool: cargo
    registry: crates
    test_threshold: 90.0
```
