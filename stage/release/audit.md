# 发布审计

发布审计回答两个问题：现在能不能发布，以及发布以后是否真的成功。

审计分为自动审计和手动审计。自动审计优先使用 `qtcloud-devops`、CI 和平台 API；手动审计用于补足工具暂时覆盖不到的判断，例如业务范围、发布目标是否完整、Release 说明是否可读。

## 审计对象

| 对象 | 检查重点 |
|------|----------|
| 发布范围 | scope、目录、目标版本、目标 tag |
| 版本事实 | 配置文件、锁文件、CHANGELOG、Git tag |
| 代码状态 | 工作区、目标提交、主分支可达性 |
| 构建测试 | build、test、coverage、lint、平台构建 |
| 安全 | 密钥、token、私有数据、发布凭证 |
| 发布目标 | GitHub Release、制品库、镜像仓库、二进制资产、内部制品 |
| 证据 | CI run、release URL、registry URL、安装或下载验证 |

## 自动审计

已接入 `qtcloud-devops` 的项目优先运行：

```bash
qtcloud-devops release status
qtcloud-devops release audit -v cli/v0.3.2 --scope cli
```

按项目需要补充构建和测试状态：

```bash
qtcloud-devops build status
qtcloud-devops test status
```

发布后检查：

```bash
qtcloud-devops release status
gh release view cli/v0.3.2 --json tagName,url,assets
gh run view <run-id> --json status,conclusion,url
```

制品库检查按目标选择：

```bash
cargo info <crate> --registry crates-io
python -m pip index versions <package>
npm view <package> version
docker manifest inspect ghcr.io/<org>/<image>:<version>
```

自动审计失败时，不进入发布步骤。应先修复版本、文档、CI、凭证或制品配置，再重新审计。

## 手动审计

手动审计适合以下情况：

- 项目还没有接入 `qtcloud-devops release audit`。
- 发布目标是内部制品库、对象存储、客户交付包或其他非标准平台。
- 需要判断 CHANGELOG 是否准确表达用户可感知变化。
- 需要确认负责人是否允许手动备用路径。

发布前手动清单：

- 目标版本和 tag 格式正确。
- scope 与目录一致。
- 配置文件版本、CHANGELOG 条目、目标 tag 一致。
- CHANGELOG 使用 Keep a Changelog 分类，内容不是 commit 罗列。
- 工作区干净，无临时文件混入。
- 构建、测试、lint 或平台构建已通过。
- 发布凭证在组织、仓库或受控环境中配置，没有写入仓库。
- 发布目标已列全，明确哪些需要 GitHub Release、registry、镜像或二进制资产。
- `main` 或发布分支包含本次发布提交。

发布后手动清单：

- tag 已存在并指向本次发布提交。
- GitHub Release 已创建，正文来自 CHANGELOG。
- 每个 registry 都能查到目标版本。
- 二进制资产、安装包、镜像或内部制品能访问。
- CI/CD release job 成功，失败 job 已处理。
- 交付说明中没有把未完成目标写成已完成。

## 管理员与 AI 决策方法

管理员判断“该不该发”：确认业务范围、版本名称、发布目标、是否接受风险，以及是否允许手动操作。

AI 判断“能不能发”：检查事实源是否一致、自动化命令是否通过、文档是否闭环、是否存在安全风险、发布目标是否遗漏。

协作时按以下规则处理分歧：

- 管理员说要发布，但事实源缺失：先补事实源。
- 自动审计通过，但发布目标未声明清楚：先确认目标。
- 手动操作更快，但项目已有 CLI/CI 发布流程：仍使用 CLI/CI。
- 发布结果部分成功：只报告已成功目标，失败目标进入异常处理。

## 审计结论格式

审计结论应包含：

- 发布对象：仓库、scope、版本、tag。
- 发布路径：CLI 发布或手动操作。
- 发布目标：GitHub Release、registry、镜像、二进制资产或内部制品。
- 发布前结果：通过项、失败项、未覆盖项。
- 发布后结果：每个目标的 URL、CI run 或验证命令。
- 后续处理：需要修复、补建、重跑或搁置的事项。

只有当声明的发布目标全部通过审计，才可以表述为“版本已发布完成”。
