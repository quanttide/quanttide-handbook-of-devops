# Source 配置

事实源维度，定义版本号的读取来源。

## 字段

```yaml
sources:
  version:
    type: auto        # 版本号来源类型
    path: null        # 自定义路径（可选）
```

## 枚举值

### SourceType

| 值 | 说明 |
|----|------|
| `auto` | 自动检测（默认） |
| `cargo` | 从 `Cargo.toml` 读取 |
| `pyproject` | 从 `pyproject.toml` 读取 |
| `pubspec` | 从 `pubspec.yaml` 读取 |
| `package.json` | 从 `package.json` 读取 |
| `tagOnly` | 只从 git tag 读取，不读配置文件 |

## 默认值

```yaml
sources:
  version:
    type: auto
    path: null
```
