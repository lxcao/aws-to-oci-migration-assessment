# SOW Framework

Draft the SOW from the feasibility assessment, not from raw survey answers alone. Keep the SOW specific enough for delivery governance while avoiding customer-specific facts in reusable templates.

## Required Sections

1. Introduction / 简介
   - Purpose of the SOW.
   - Relationship to the assessment report.

2. Project Background / 项目背景
   - Source cloud and migration intent.
   - Business and technical drivers.

3. Project Objectives / 项目目标
   - Establish OCI target foundation.
   - Migrate agreed workloads.
   - Validate function, performance, security, and operability.

4. Scope of Services / 服务范围
   - Discovery validation.
   - Target OCI architecture design.
   - Landing zone or baseline environment support.
   - Network and security configuration guidance.
   - Workload migration execution or advisory.
   - Data migration support.
   - Cutover planning and stabilization.

5. Out of Scope / 非服务范围
   - Application code refactoring unless explicitly included.
   - Long-term managed operations unless explicitly included.
   - Third-party license procurement.
   - Production data correction or application bug fixing.
   - Security penetration testing unless explicitly included.

6. Workstreams / 工作流
   - Governance and project management.
   - OCI foundation and networking.
   - Compute/container migration.
   - Database and data migration.
   - Storage migration.
   - Middleware and integration migration.
   - Security, monitoring, backup, and operations.
   - Cutover and rollback.

7. Deliverables / 交付物
   - Migration design document.
   - Migration runbook.
   - OCI configuration checklist.
   - Test and validation report.
   - Cutover plan.
   - Handover document.

8. Acceptance Criteria / 验收标准
   - Resource provisioning completed for agreed scope.
   - Connectivity validated.
   - Migration runbook reviewed.
   - Workload tests passed by customer.
   - Cutover or pilot migration completed according to agreed criteria.

9. Roles and Responsibilities / 双方职责
   - Use a RACI table.
   - Make customer responsibilities explicit: access, AWS data, test cases, business validation, DNS/certificate approvals, downtime approval.

10. Project Timeline and Milestones / 项目计划与里程碑
   - Discovery validation.
   - Design.
   - Build.
   - Pilot.
   - Migration waves.
   - Stabilization and handover.

11. Assumptions and Dependencies / 假设与依赖
   - Customer provides timely access and feedback.
   - OCI services are available in target region.
   - Required network connectivity is approved and available.
   - Required maintenance window is approved.

12. Change Management / 变更管理
   - Scope changes require written approval.
   - Material changes to workload count, migration method, timeline, or acceptance criteria must be treated as change requests.

13. Risks and Mitigations / 风险与缓解
   - Link to assessment report risk register.

14. Confidentiality, Security, and Compliance / 保密、安全与合规
   - Define handling of credentials and customer data.
   - Avoid storing secrets in migration artifacts.

15. Exclusions and Commercial Notes / 除外事项与商务说明
   - Keep legal/commercial language aligned with the provider's standard contract process.

## SOW Drafting Rules

- Use clear commitment language only where delivery scope is confirmed.
- Use "support", "assist", or "provide guidance" for advisory tasks.
- Use "customer is responsible for" for activities requiring customer ownership.
- Avoid promising SLA, performance, cost savings, zero downtime, or feature equivalence unless those items are explicitly validated and contractually approved.
- Keep acceptance criteria testable.
- Include placeholders for customer/project metadata rather than embedding previous customer data.
