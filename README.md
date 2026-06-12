# AWS to OCI Migration Assessment Skill 使用指南

这个 skill 用于 OCI 售前迁移场景，帮助 Cloud Engineer 复用从 AWS 迁移到 OCI 的三阶段交付流程：

1. 生成 AWS 到 OCI 迁移调研表。
2. 根据客户填写的调研反馈生成迁移调研与可行性评估报告。
3. 根据评估报告生成云平台迁移服务工作说明书 SOW 初稿。

skill 支持中文、英文和中英双语输出。模板是通用版本，不包含任何客户专用信息。

## 1. 确认安装位置

Codex 通常可使用的位置：

```bash
~/.codex/skills/aws-to-oci-migration-assessment
```

其他支持 Agent Skills 标准的 agent，例如 OpenCode，通常可使用的位置：

```bash
~/.agents/skills/aws-to-oci-migration-assessment
```

skill 目录中应包含：

```text
SKILL.md
README.md
README_EN.md
pyproject.toml
uv.lock
assets/
references/
scripts/
agents/openai.yaml
```

## 2. 了解目录内容

核心文件：

```text
SKILL.md
```

这是 agent 自动加载的主要说明文件。正常使用时不需要手工修改。

模板文件：

```text
assets/aws_to_oci_migration_survey_template.xlsx
assets/aws_to_oci_feasibility_report_template.docx
assets/aws_to_oci_migration_sow_template.docx
```

参考资料：

```text
references/service-mapping.md
references/survey-question-bank.md
references/assessment-framework.md
references/sow-framework.md
references/bilingual-terms.md
```

辅助脚本：

```text
scripts/generate_generic_templates.py
scripts/extract_feedback_summary.py
```

## 3. 准备 uv 运行环境

辅助脚本通过 `uv` 管理 Python 依赖。第一次运行脚本前，进入 skill 目录并同步依赖：

```bash
cd ~/.agents/skills/aws-to-oci-migration-assessment
uv sync
```

如果你在 Codex 的 skill 目录使用，也可以进入：

```bash
cd ~/.codex/skills/aws-to-oci-migration-assessment
uv sync
```

检查脚本能否运行：

```bash
uv run python scripts/generate_generic_templates.py --output-dir ./outputs
```

依赖声明在：

```text
pyproject.toml
```

锁定版本在：

```text
uv.lock
```

## 4. 第一次使用：让 agent 触发 skill

在 Codex、OpenCode 或其他支持 skills 的 agent 中，可以直接提出类似请求：

```text
使用 aws-to-oci-migration-assessment skill，帮我为一个客户生成 AWS 到 OCI 迁移调研表，输出中文 Excel。
```

或：

```text
Use the aws-to-oci-migration-assessment skill to create a bilingual AWS-to-OCI migration discovery workbook.
```

如果 agent 没有自动触发，可以明确给出 skill 路径：

```text
请使用 ~/.agents/skills/aws-to-oci-migration-assessment 这个 skill。
```

## 5. 阶段一：生成迁移调研表

推荐输入：

```text
使用 aws-to-oci-migration-assessment skill，生成一份 AWS 到 OCI 迁移调研表。
要求：
1. 输出 Excel。
2. 使用中文。
3. 覆盖 ECS、网络、S3、RDS MySQL、Redis、ELB、EFS、EC2、OS、块存储、自建 NGINX API Gateway、自建 Kafka。
4. 每类调研内容放在独立 sheet。
5. 保留“反馈”列供客户填写。
```

agent 应使用或复制这个模板：

```text
assets/aws_to_oci_migration_survey_template.xlsx
```

如果需要重新生成通用模板，可以运行：

```bash
uv run python scripts/generate_generic_templates.py --output-dir ./outputs
```

生成后，把 Excel 发给客户填写。客户应重点填写每个 sheet 的 `Feedback / 反馈` 列，也可以补充 `AWS Current State / AWS现状`、`Initial Risk / 初步风险` 和 `Notes / 备注`。

## 6. 阶段二：分析客户反馈并生成评估报告

客户返回填写后的 Excel 后，推荐输入：

```text
使用 aws-to-oci-migration-assessment skill，读取客户填写后的迁移调研表。
请分析每个 sheet 的“反馈”列，整理 AWS 当前状态、OCI 目标方案、迁移风险、缓解措施和待确认问题。
然后生成一份中文 Word 格式的 OCI 迁移调研与可行性评估报告。
```

如果先想生成反馈摘要，可以运行：

```bash
uv run python scripts/extract_feedback_summary.py completed-survey.xlsx --out feedback-summary.md --json feedback-summary.json
```

评估报告应基于这个模板：

```text
assets/aws_to_oci_feasibility_report_template.docx
```

报告应至少包含：

- 摘要。
- 调研范围与假设。
- AWS 当前状态摘要。
- OCI 目标方案。
- 分服务可行性评估。
- 迁移方案与工具。
- 风险清单。
- POC 与验收矩阵。
- 迁移路线图。
- 待确认事项。

风险评分规则见：

```text
references/assessment-framework.md
```

AWS 到 OCI 服务映射参考见：

```text
references/service-mapping.md
```

## 7. 阶段三：生成 SOW 初稿

在评估报告完成后，推荐输入：

```text
使用 aws-to-oci-migration-assessment skill，基于这份迁移调研与可行性评估报告，生成一份云平台迁移服务工作说明书 SOW 初稿。
要求：
1. Word 格式。
2. 中文。
3. 明确服务范围、非服务范围、交付物、验收标准、双方职责、假设依赖、风险与变更管理。
```

SOW 应基于这个模板：

```text
assets/aws_to_oci_migration_sow_template.docx
```

SOW 框架参考：

```text
references/sow-framework.md
```

注意：SOW 应从评估报告生成，不建议直接从原始调研表生成。这样可以避免遗漏风险、前置条件和非服务范围。

## 8. 选择输出语言

中文输出：

```text
请输出中文版本。
```

英文输出：

```text
Please generate the English version.
```

中英双语输出：

```text
请输出中英双语版本，中文段落后跟英文段落。
```

术语表参考：

```text
references/bilingual-terms.md
```

## 9. 客户信息脱敏要求

不要把客户专用信息写回 skill 的模板或参考文件中，包括：

- 客户名称。
- 应用系统名称。
- AWS account ID、OCI tenancy ID、OCID、ARN。
- region、availability zone、IP、CIDR、域名、主机名。
- 成本、容量、性能、业务数据。
- 合同条款、报价、人员联系方式。

如果需要在文档中保留位置，使用占位符：

```text
[Customer Name]
[Project Name]
[Source Region]
[Target OCI Region]
[CIDR]
[Application Name]
[RTO/RPO]
[Migration Wave]
```

## 10. 技术准确性要求

在生成客户正式交付材料前，涉及以下内容时应核对 Oracle 官方文档：

- OCI 服务能力。
- 服务限制。
- 区域可用性。
- 价格。
- 迁移工具。
- 产品名称。
- AWS 服务到 OCI 服务的等价关系。

不要假设一一对应。例如：

- ECS 可能迁移到 OKE、Container Instances 或 Compute 自建容器运行环境。
- RDS MySQL 可能迁移到 MySQL HeatWave，也可能采用自建 MySQL。
- 自建 Kafka 可能迁移到 OCI Streaming、OCI Streaming with Apache Kafka，或继续自建 Kafka。
- 自建 NGINX API Gateway 不一定能完全等价迁移到 OCI API Gateway，需做 fit-gap 分析。

## 11. 常见问题

### Q1：这个 skill 会自动生成最终客户文档吗？

可以辅助生成，但最终交付前仍需要 CE 审阅、补充客户具体信息、核对 OCI 官方文档，并确认风险、范围和商务边界。

### Q2：客户只填写了部分反馈怎么办？

先使用反馈摘要脚本或让 agent 汇总缺失项，再把缺失内容整理成 `待确认事项 / Open Questions`。不要在评估报告中假设未知信息。

### Q3：能不能直接复用以前客户的报告？

可以参考结构，但必须脱敏。不要把客户名、应用、region、IP、容量、成本、合同信息写入通用 skill。

### Q4：OpenCode 能不能使用？

如果 OpenCode 已启用 Agent Skills，并扫描 `~/.agents/skills`，则可以识别这个 skill。若没有自动识别，请在 prompt 中显式提供 skill 路径：

```text
~/.agents/skills/aws-to-oci-migration-assessment
```

### Q5：脚本运行失败怎么办？

先在 skill 目录运行：

```bash
uv sync
```

然后用 `uv run` 执行脚本：

```bash
uv run python scripts/generate_generic_templates.py --output-dir ./outputs
```

脚本依赖由 `pyproject.toml` 管理。如果当前 agent 无法运行 `uv`，可以让 agent 使用它自己的 Office 文档能力，或在该环境中安装 `pyproject.toml` 声明的依赖后再运行脚本。

## 12. 推荐完整工作流

第一次客户迁移项目可以按这个顺序执行：

1. 让 agent 使用 skill 生成迁移调研表。
2. CE 审阅调研表，按客户实际范围删减 sheet 或问题。
3. 发给客户填写 `反馈` 列。
4. 收回 Excel 后，生成反馈摘要。
5. 基于反馈摘要和调研表生成可行性评估报告。
6. CE 核对服务映射、风险和 OCI 官方文档。
7. 基于评估报告生成 SOW 初稿。
8. CE、项目经理、商务或法务按公司流程审阅 SOW。
9. 输出正式客户版本。

## 13. 注意事项

所有生成的文档都必须经过 CE 审核后才递交给客户。agent 生成的调研表、评估报告和 SOW 初稿只能作为工作草稿，不能绕过 CE 对技术准确性、客户信息、风险判断、服务范围和商务边界的最终确认。
