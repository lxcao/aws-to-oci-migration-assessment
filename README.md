# AWS to OCI Migration Assessment Skill Guide

中文版本见 [README_CN.md](README_CN.md).

This skill is designed for OCI presales migration scenarios. It helps Cloud Engineers reuse a standard three-stage workflow for migrating customers from AWS to OCI:

1. Create an AWS-to-OCI migration discovery survey workbook.
2. Generate a migration discovery and feasibility assessment report based on customer feedback.
3. Generate a cloud platform migration Statement of Work (SOW) draft based on the assessment report.

The skill supports Chinese, English, and bilingual Chinese/English outputs. All bundled templates are generic and do not contain customer-specific information.

## 1. Confirm Installation Paths

Codex can typically use the skill from:

```bash
~/.codex/skills/aws-to-oci-migration-assessment
```

Other agents that support the Agent Skills standard, such as OpenCode, can typically use the skill from:

```bash
~/.agents/skills/aws-to-oci-migration-assessment
```

The skill directory should contain:

```text
SKILL.md
README.md
README_CN.md
pyproject.toml
uv.lock
assets/
references/
scripts/
agents/openai.yaml
```

## 2. Understand the Directory Structure

Main agent-facing instruction file:

```text
SKILL.md
```

This is the file agents load automatically. In normal usage, you do not need to edit it.

Template files:

```text
assets/aws_to_oci_migration_survey_template.xlsx
assets/aws_to_oci_feasibility_report_template.docx
assets/aws_to_oci_migration_sow_template.docx
```

Reference files:

```text
references/service-mapping.md
references/survey-question-bank.md
references/assessment-framework.md
references/sow-framework.md
references/bilingual-terms.md
```

Helper scripts:

```text
scripts/generate_generic_templates.py
scripts/extract_feedback_summary.py
```

## 3. Prepare the uv Runtime

The helper scripts use `uv` to manage Python dependencies. Before running the scripts for the first time, enter the skill directory and sync dependencies:

```bash
cd ~/.agents/skills/aws-to-oci-migration-assessment
uv sync
```

If you are using the Codex skill directory, enter:

```bash
cd ~/.codex/skills/aws-to-oci-migration-assessment
uv sync
```

Verify that the scripts can run:

```bash
uv run python scripts/generate_generic_templates.py --output-dir ./outputs
```

Dependencies are declared in:

```text
pyproject.toml
```

Resolved dependency versions are locked in:

```text
uv.lock
```

## 4. First Use: Trigger the Skill in an Agent

In Codex, OpenCode, or another agent that supports skills, use a prompt like:

```text
Use the aws-to-oci-migration-assessment skill to create an English AWS-to-OCI migration discovery workbook.
```

Or:

```text
Use the aws-to-oci-migration-assessment skill to create a bilingual AWS-to-OCI migration discovery workbook.
```

If the agent does not trigger the skill automatically, provide the skill path explicitly:

```text
Please use the skill at ~/.agents/skills/aws-to-oci-migration-assessment.
```

## 5. Stage 1: Create the Migration Discovery Survey

Recommended prompt:

```text
Use the aws-to-oci-migration-assessment skill to create an AWS-to-OCI migration discovery survey.
Requirements:
1. Output an Excel workbook.
2. Use English.
3. Cover ECS, networking, S3, RDS MySQL, Redis, ELB, EFS, EC2, OS, and block storage, self-managed NGINX API Gateway, and self-managed Kafka.
4. Put each survey area in a separate sheet.
5. Keep a Feedback column for the customer to complete.
```

The agent should use or copy this template:

```text
assets/aws_to_oci_migration_survey_template.xlsx
```

To regenerate generic templates manually, run:

```bash
uv run python scripts/generate_generic_templates.py --output-dir ./outputs
```

After generation, send the Excel workbook to the customer. The customer should primarily complete the `Feedback / 反馈` column. They may also fill in `AWS Current State / AWS现状`, `Initial Risk / 初步风险`, and `Notes / 备注`.

## 6. Stage 2: Analyze Customer Feedback and Generate the Assessment Report

After the customer returns the completed Excel workbook, use a prompt like:

```text
Use the aws-to-oci-migration-assessment skill to read the completed migration discovery workbook.
Analyze the Feedback column in each sheet and summarize the AWS current state, OCI target options, migration risks, mitigations, and open questions.
Then generate an English Word-format OCI migration discovery and feasibility assessment report.
```

To generate a feedback summary first, run:

```bash
uv run python scripts/extract_feedback_summary.py completed-survey.xlsx --out feedback-summary.md --json feedback-summary.json
```

The assessment report should be based on:

```text
assets/aws_to_oci_feasibility_report_template.docx
```

The report should include at least:

- Executive summary.
- Discovery scope and assumptions.
- AWS current state summary.
- OCI target options.
- Service-by-service feasibility assessment.
- Migration approach and tools.
- Risk register.
- POC and acceptance matrix.
- Migration roadmap.
- Open questions.

Risk scoring rules are documented in:

```text
references/assessment-framework.md
```

AWS-to-OCI service mapping guidance is documented in:

```text
references/service-mapping.md
```

## 7. Stage 3: Generate the SOW Draft

After the assessment report is complete, use a prompt like:

```text
Use the aws-to-oci-migration-assessment skill to generate a cloud platform migration Statement of Work draft based on this migration discovery and feasibility assessment report.
Requirements:
1. Word format.
2. English.
3. Clearly define service scope, out-of-scope items, deliverables, acceptance criteria, roles and responsibilities, assumptions and dependencies, risks, and change management.
```

The SOW should be based on:

```text
assets/aws_to_oci_migration_sow_template.docx
```

SOW structure guidance is documented in:

```text
references/sow-framework.md
```

Note: generate the SOW from the assessment report, not directly from the raw survey workbook. This helps avoid missing risks, prerequisites, assumptions, and exclusions.

## 8. Choose the Output Language

Chinese output:

```text
Please generate the Chinese version.
```

English output:

```text
Please generate the English version.
```

Bilingual output:

```text
Please generate a bilingual Chinese/English version, with each Chinese paragraph followed by the English paragraph.
```

Terminology reference:

```text
references/bilingual-terms.md
```

## 9. Customer Data Sanitization Requirements

Do not write customer-specific information back into the skill templates or reference files, including:

- Customer names.
- Application or system names.
- AWS account IDs, OCI tenancy IDs, OCIDs, or ARNs.
- Regions, availability zones, IP addresses, CIDRs, domains, and hostnames.
- Cost, capacity, performance, or business data.
- Contract terms, pricing, or personal contact information.

Use placeholders when the document needs these fields:

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

## 10. Technical Accuracy Requirements

Before producing customer-final deliverables, validate the following against Oracle official documentation:

- OCI service capabilities.
- Service limits.
- Regional availability.
- Pricing.
- Migration tools.
- Product names.
- AWS-to-OCI service equivalence.

Do not assume one-to-one equivalence. For example:

- ECS may migrate to OKE, Container Instances, or a Compute-based container runtime.
- RDS MySQL may migrate to MySQL HeatWave or to self-managed MySQL.
- Self-managed Kafka may migrate to OCI Streaming, OCI Streaming with Apache Kafka, or remain self-managed on OCI.
- Self-managed NGINX API Gateway may not map completely to OCI API Gateway and requires fit-gap analysis.

## 11. FAQ

### Q1: Does this skill automatically create final customer-ready documents?

It can help create drafts, but the CE must still review, add customer-specific information, validate OCI documentation, and confirm risks, scope, and commercial boundaries.

### Q2: What if the customer only completed part of the survey?

Generate a feedback summary first, then convert missing or unclear items into `Open Questions`. Do not assume unknown information in the assessment report.

### Q3: Can I reuse a previous customer's report?

You may reuse the structure, but you must sanitize it. Do not place customer names, applications, regions, IPs, capacity, cost, or contract details into the generic skill.

### Q4: Can OpenCode use this skill?

Yes, if OpenCode has Agent Skills enabled and scans `~/.agents/skills`. If it does not auto-detect the skill, provide the path explicitly:

```text
~/.agents/skills/aws-to-oci-migration-assessment
```

### Q5: What if the helper scripts fail?

First run this from the skill directory:

```bash
uv sync
```

Then run scripts with `uv run`:

```bash
uv run python scripts/generate_generic_templates.py --output-dir ./outputs
```

Script dependencies are managed in `pyproject.toml`. If the current agent environment cannot run `uv`, ask the agent to use its own Office document tooling or install the dependencies declared in `pyproject.toml` before running the scripts.

## 12. Recommended End-to-End Workflow

For a first customer migration project, use this sequence:

1. Ask the agent to use this skill to generate the migration discovery workbook.
2. Review the workbook and remove sheets or questions that are not relevant to the customer scope.
3. Send the workbook to the customer and ask them to complete the `Feedback` column.
4. After receiving the completed Excel workbook, generate a feedback summary.
5. Generate the feasibility assessment report from the feedback summary and survey workbook.
6. Validate service mappings, risks, and OCI official documentation.
7. Generate the SOW draft from the assessment report.
8. Have the CE, project manager, commercial team, or legal team review the SOW according to the company process.
9. Produce the final customer-facing version.

## 13. Important Note

All generated documents must be reviewed by a CE before they are delivered to the customer. Agent-generated discovery workbooks, assessment reports, and SOW drafts are working drafts only and must not bypass CE validation of technical accuracy, customer information, risk judgment, service scope, and commercial boundaries.
