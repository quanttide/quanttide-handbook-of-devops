# Scope 配置

Scope 将 git tag 的 scope 前缀映射到项目中的子目录，供 `release status` 和 `release publish` 使用。

## 配置文件

`.quanttide/devops/contract.yaml`

```yaml
scopes:
  cli: src/cli
  studio: src/studio
  provider: src/provider
```

## 作用

- **`release status`**：按 scope 分组展示发布状态，检查对应子目录的版本一致性和 CHANGELOG
- **`release publish`**：自动更新 scope 子目录下的配置文件和 CHANGELOG

## 示例

### 单 scope 项目（CLI 工具）

```yaml
scopes:
  cli: .
```

### 多 scope 项目（monorepo）

```yaml
scopes:
  studio: src/studio
  provider: src/provider
  cli: src/cli
```

### 无前缀 tag

不带 scope 前缀的 tag（如 `v0.1.0`）默认属于 `(root)` scope，对应项目根目录，无需在配置中声明。
