---
name: aws-to-oci-migration-assessment
description: Build reusable bilingual Chinese/English AWS-to-OCI migration presales artifacts. Use when an OCI Cloud Engineer needs to create AWS-to-OCI discovery survey workbooks, analyze completed survey feedback, draft migration feasibility assessment reports, or generate cloud migration Statement of Work (SOW) documents for services such as ECS, EC2, VPC/networking, S3, RDS MySQL, ElastiCache Redis, ELB, EFS, self-managed middleware, NGINX API Gateway, and Kafka/Streaming.
---

# AWS to OCI Migration Assessment

Use this skill to run a three-stage OCI presales migration workflow:

1. Create an AWS-to-OCI migration discovery workbook.
2. Analyze customer feedback and draft a migration feasibility assessment report.
3. Generate a cloud platform migration SOW draft from the assessment.

The skill is intentionally generic. Do not embed customer names, application names, regions, IPs, account IDs, hostnames, commercial terms, or other customer-specific details into reusable skill files or templates.

## Quick Start

Create generic templates:

```bash
uv sync
uv run python scripts/generate_generic_templates.py --output-dir ./outputs
```

Extract feedback from a completed survey workbook:

```bash
uv run python scripts/extract_feedback_summary.py completed-survey.xlsx --out feedback-summary.md --json feedback-summary.json
```

When creating `.xlsx` or `.docx` deliverables in Codex, follow the installed Spreadsheets/Documents skills for rendering and verification. Use the templates in `assets/` as starting points, not as fixed customer deliverables.

## Workflow

### 1. Discovery Workbook

Start from `assets/aws_to_oci_migration_survey_template.xlsx` or generate a fresh copy with `scripts/generate_generic_templates.py`.

Include separate sheets for independent service investigations:

- Application architecture and business requirements.
- AWS networking to OCI VCN/networking.
- AWS Elastic Load Balancing to OCI Load Balancer or Network Load Balancer.
- AWS ECS/container workloads to OCI OKE, Container Instances, or Compute-based runtime.
- AWS S3 to OCI Object Storage.
- AWS RDS MySQL to OCI MySQL HeatWave or customer-managed MySQL.
- AWS ElastiCache for Redis to OCI Cache with Redis or self-managed Redis.
- AWS EFS to OCI File Storage.
- EC2/block-storage self-managed workloads.
- Self-managed NGINX API Gateway to OCI API Gateway.
- Self-managed Kafka to OCI Streaming, OCI Streaming with Apache Kafka, or self-managed Kafka.
- Migration waves, cutover, rollback, security, IAM, monitoring, backup, and operations.

Keep the same core columns across sheets:

- `ID / 编号`
- `Dimension / 调研维度`
- `Question / 调研问题`
- `Customer Input Required / 客户需提供的信息`
- `AWS Current State / AWS现状`
- `OCI Target / OCI目标`
- `Assessment Focus / 评估重点`
- `Feedback / 反馈`
- `Initial Risk / 初步风险`
- `Notes / 备注`

For detailed question sets, read `references/survey-question-bank.md`.

### 2. Feasibility Assessment

Use `scripts/extract_feedback_summary.py` to summarize completed workbook feedback. Then draft a report from `assets/aws_to_oci_feasibility_report_template.docx`.

The report must include:

- Executive summary.
- Discovery scope and assumptions.
- Current AWS state summary by service.
- OCI target options and recommended migration path.
- Service-by-service feasibility assessment.
- Migration tools and approach.
- Risk register with severity, rationale, mitigation, and owner.
- POC or validation matrix.
- Migration waves and cutover approach.
- Open questions and decision log.

Use `references/assessment-framework.md` for scoring and report structure. Use `references/service-mapping.md` for default AWS-to-OCI target mappings.

### 3. SOW Draft

Draft the SOW from the assessment, not directly from the raw survey. Start from `assets/aws_to_oci_migration_sow_template.docx`.

The SOW must define:

- Project background and objectives.
- In-scope and out-of-scope services.
- Delivery workstreams.
- Deliverables and acceptance criteria.
- Customer responsibilities and provider responsibilities.
- Migration methodology, waves, and change control.
- Assumptions, dependencies, risks, and exclusions.
- Security, confidentiality, and compliance boundaries.

Use `references/sow-framework.md` for the section model and language.

## Bilingual Output Rules

Support three output modes:

- `中文`: Chinese-only customer-facing artifacts.
- `English`: English-only customer-facing artifacts.
- `中英双语 / Bilingual`: Chinese heading followed by English heading, or Chinese paragraph followed by English paragraph.

Use consistent terminology from `references/bilingual-terms.md`. Do not mix translated service names inconsistently across survey, assessment report, and SOW.

## Technical Accuracy Rules

For current OCI service capabilities, limits, pricing, regional availability, migration tooling, or product naming, verify against Oracle official documentation before making factual claims. If browsing is unavailable, state that the service mapping is preliminary and requires Oracle documentation validation.

Do not overstate managed-service equivalence. When the OCI target is not a direct like-for-like service, present multiple options and list tradeoffs.

Important examples:

- ECS does not map to only one OCI service. Consider OKE, Container Instances, and Compute-based container runtime depending on orchestration model, networking, deployment process, and operations maturity.
- Self-managed Kafka can map to OCI Streaming Kafka-compatible APIs, OCI Streaming with Apache Kafka, or self-managed Kafka on Compute depending on protocol compatibility, topic/partition design, retention, throughput, and operational constraints.
- RDS MySQL can map to MySQL HeatWave, customer-managed MySQL, or another OCI database pattern depending on version, extensions, replication, maintenance window, HA, and performance requirements.
- S3 can map to OCI Object Storage, but API compatibility, bucket policy, lifecycle, encryption, event integration, storage class, and data transfer method must be assessed.

## Sanitization Rules

When converting a previous customer artifact into a reusable template:

- Remove customer names, project names, application names, domains, hostnames, email addresses, phone numbers, account IDs, tenancy IDs, OCIDs, ARNs, IP ranges, region names, availability zones, capacity numbers, cost figures, incident references, and contract-specific terms.
- Replace examples with neutral placeholders such as `[Customer Name]`, `[Application Name]`, `[Source Region]`, `[Target OCI Region]`, `[CIDR]`, `[RTO/RPO]`, and `[Migration Wave]`.
- Keep only reusable structure, generic wording, blank fields, and non-sensitive service assessment logic.

## Resource Map

- `assets/aws_to_oci_migration_survey_template.xlsx`: Generic survey workbook.
- `assets/aws_to_oci_feasibility_report_template.docx`: Generic assessment report template.
- `assets/aws_to_oci_migration_sow_template.docx`: Generic SOW template.
- `pyproject.toml`: uv-managed Python dependencies for helper scripts.
- `uv.lock`: Resolved dependency lock file for reproducible uv environments.
- `scripts/generate_generic_templates.py`: Regenerate template assets.
- `scripts/extract_feedback_summary.py`: Summarize feedback columns from completed survey workbooks.
- `references/service-mapping.md`: AWS-to-OCI target mapping and tradeoffs.
- `references/survey-question-bank.md`: Reusable bilingual survey question bank.
- `references/assessment-framework.md`: Risk scoring and assessment report method.
- `references/sow-framework.md`: SOW drafting structure and clauses.
- `references/bilingual-terms.md`: Chinese/English terminology.
