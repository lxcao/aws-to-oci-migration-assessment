# Feasibility Assessment Framework

Use this framework to turn completed survey feedback into an OCI migration feasibility report.

## Risk Dimensions

Score each service or workload from 1 to 5 for each dimension.

| Score | Meaning |
|---|---|
| 1 | Low risk; standard migration pattern; few unknowns |
| 2 | Low-to-medium risk; minor configuration or validation needed |
| 3 | Medium risk; dependency, compatibility, or operational change requires planning |
| 4 | High risk; migration path is feasible but requires POC, design decisions, or downtime negotiation |
| 5 | Critical risk; feasibility cannot be confirmed without major unknowns being resolved |

Assess these dimensions:

- Compatibility: API, protocol, OS, database, middleware, version, and feature compatibility.
- Data migration: data volume, replication, consistency, backup, validation, and rollback.
- Cutover: DNS, endpoint, certificate, downtime window, rollback, and stakeholder coordination.
- Performance and capacity: throughput, latency, IOPS, CPU/memory, connection count, autoscaling, and quotas.
- Operations and security: IAM, encryption, logging, monitoring, backup, incident response, patching, and ownership.

## Overall Risk

Use the maximum score as the starting overall risk, then adjust one level up when:

- Multiple dimensions score 4 or above.
- Customer feedback is missing for a critical dependency.
- The target OCI service choice is unresolved.
- The migration requires application code changes.

Use these labels:

- Low / 低
- Medium / 中
- High / 高
- Critical / 关键

## Report Structure

1. Executive Summary / 摘要
   - State whether migration appears feasible.
   - List the top risks.
   - Identify recommended next actions.

2. Scope and Assumptions / 范围与假设
   - List source AWS services assessed.
   - List target OCI region as placeholder unless customer-specific data is explicitly provided for this engagement.
   - State what was not assessed.

3. Current State Summary / AWS现状摘要
   - Summarize by service.
   - Highlight missing feedback.

4. OCI Target Options / OCI目标方案
   - Provide recommended target service.
   - Include alternatives when the mapping is not direct.

5. Feasibility Assessment / 可行性评估
   - Use a table with service, current state, OCI target, feasibility, risk, key concerns, and mitigation.

6. Migration Approach and Tools / 迁移方案与工具
   - Describe network foundation, landing zone, data migration, workload migration, validation, cutover, and rollback.

7. Risk Register / 风险清单
   - Include risk ID, description, severity, impact, mitigation, owner, and status.

8. POC and Acceptance Matrix / POC与验收矩阵
   - Define what must be proven before production migration.

9. Migration Roadmap / 迁移路线图
   - Group into assessment, design, build, pilot, migration waves, and stabilization.

10. Open Questions / 待确认事项
   - Convert blank or ambiguous feedback into concrete questions.

## Recommended Report Tables

Service assessment table:

| Area | AWS Current State | OCI Target | Feasibility | Risk | Key Issues | Mitigation |
|---|---|---|---|---|---|---|

Risk register:

| ID | Risk | Severity | Impact | Mitigation | Owner | Status |
|---|---|---|---|---|---|---|

POC matrix:

| POC Item | Objective | Success Criteria | Test Method | Owner |
|---|---|---|---|---|

Open question table:

| Area | Question | Why It Matters | Required Owner |
|---|---|---|---|
