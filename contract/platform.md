# Platform 配置

载体维度，定义外部治理平台。

## 字段

```yaml
platforms:
  source_control: github            # 源代码管理平台
  pipeline: github_actions          # CI/CD pipeline
  artifact_registry: crates         # 制品库
```

## 枚举值

### SourceControl

| 值 | 说明 |
|----|------|
| `github` | GitHub（默认） |
| `gitlab` | GitLab |
| `gitee` | Gitee |

### Pipeline

| 值 | 说明 |
|----|------|
| `github_actions` | GitHub Actions（默认） |
| `gitlab_ci` | GitLab CI |
| `jenkins` | Jenkins |

### Registry

| 值 | 说明 |
|----|------|
| `crates` | crates.io |
| `pypi` | PyPI |
| `pubdev` | pub.dev |
| `npm` | npm |
| `github_releases` | GitHub Releases |
| `docker` | Docker |
| `none` | 无制品库（默认） |

## 默认值

```yaml
platforms:
  source_control: github
  pipeline: github_actions
  artifact_registry: none
```
