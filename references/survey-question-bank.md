# Survey Question Bank

Use these bilingual question sets when creating or extending the migration discovery workbook. Keep each service in its own sheet unless the customer explicitly wants a consolidated workbook.

## Common Columns

| Column | Purpose |
|---|---|
| ID / 编号 | Stable row identifier |
| Dimension / 调研维度 | Topic grouping |
| Question / 调研问题 | What to ask the customer |
| Customer Input Required / 客户需提供的信息 | Concrete evidence or data requested |
| AWS Current State / AWS现状 | Customer-provided source-state facts |
| OCI Target / OCI目标 | Proposed or candidate OCI target |
| Assessment Focus / 评估重点 | What the CE needs to evaluate |
| Feedback / 反馈 | Customer response |
| Initial Risk / 初步风险 | Low / Medium / High / Critical |
| Notes / 备注 | Follow-ups and assumptions |

## Application Architecture

- Business system name, environment, owner, and criticality.
- Application dependency map, upstream/downstream systems, and integration protocols.
- RTO, RPO, business blackout period, and allowed downtime window.
- Current deployment topology and release process.
- Peak traffic, seasonality, and performance baseline.
- Compliance, data residency, audit, and security requirements.

## Networking

- AWS VPC CIDR, subnet CIDR, route tables, NAT, internet gateway, transit gateway, and peering.
- Public/private subnet design and workload placement.
- Security groups, NACLs, firewall appliances, and allow/deny policy model.
- VPN, Direct Connect, on-premises connectivity, DNS, and resolver forwarding.
- Egress dependencies, private endpoints, service endpoints, and third-party integrations.
- Required OCI VCN, subnet, NSG, route, DRG, NAT Gateway, Service Gateway, and DNS design.

## Load Balancing

- ALB/NLB/CLB type, listener protocols, ports, TLS termination, certificates, and policies.
- Host/path routing, target groups, health checks, stickiness, redirects, and header rules.
- Backend instance/container registration method.
- Traffic volume, connection count, idle timeout, and peak behavior.
- WAF, logging, access logs, and monitoring dependencies.
- OCI Load Balancer or Network Load Balancer fit and required configuration changes.

## ECS and Containers

- ECS launch type, cluster count, service count, task definitions, CPU/memory, and autoscaling.
- Container images, registry, image scanning, secrets, environment variables, and config injection.
- Network mode, service discovery, internal/external exposure, and dependency endpoints.
- Deployment strategy, blue/green or rolling behavior, CI/CD pipeline, and rollback.
- Logs, metrics, tracing, and operational ownership.
- Target decision among OKE, Container Instances, or OCI Compute with container runtime.

## S3 Object Storage

- Bucket list, purpose, storage class, object count, total size, growth rate, and object size distribution.
- Versioning, lifecycle, retention, object lock, encryption, replication, and access logs.
- IAM policies, bucket policies, pre-signed URLs, public access, and cross-account access.
- SDK/API usage, event notifications, Lambda/integration triggers, and static website usage.
- Data migration method, bandwidth, cutover consistency, validation, and rollback.
- OCI Object Storage API, namespace, bucket policy, lifecycle, events, and archive requirements.

## RDS MySQL

- MySQL version, engine settings, parameter groups, option groups, timezone, charset, and collation.
- Database size, table count, top tables, storage growth, IOPS, CPU, memory, connections, and slow SQL.
- HA, Multi-AZ, backup retention, PITR, maintenance window, and replication.
- Extensions, stored procedures, events, triggers, users, privileges, and external integrations.
- Application connection strings, DNS, secrets, certificates, and connection pool behavior.
- Target choice among MySQL HeatWave, self-managed MySQL, or phased migration.

## ElastiCache for Redis

- Redis version, cluster mode, node type, shard/replica count, memory usage, and peak QPS.
- Persistence, snapshot, backup, failover, encryption, auth token, TLS, and parameter group.
- Keyspace pattern, eviction policy, TTL usage, pub/sub, streams, Lua scripts, and client libraries.
- Endpoint behavior, DNS cutover, application retry behavior, and connection pool settings.
- Target choice between OCI Cache with Redis and self-managed Redis.

## EFS File Storage

- File system count, total size, file count, directory depth, growth rate, throughput, and IOPS.
- Mount targets, security groups, NFS version, POSIX permissions, UID/GID, and client OS.
- Access points, backup, lifecycle, replication, and performance mode.
- Application mount paths, lock behavior, latency sensitivity, and cutover requirements.
- OCI File Storage mount target, export path, security rules, migration copy method, and validation.

## Self-managed VM, OS, and Block Storage

- EC2 instance inventory, AMI/OS version, CPU, memory, disk layout, and attached EBS volumes.
- Boot mode, kernel modules, agents, cron jobs, systemd services, and startup scripts.
- Block volume type, size, IOPS, throughput, filesystem, encryption, snapshots, and backup.
- Middleware installed on VMs such as NGINX, Nacos, RabbitMQ, Kafka, Cassandra, or custom services.
- Required ports, clustering, data directories, certificates, logs, and operational runbooks.
- OCI Compute shape, image migration, block volume performance, and licensing considerations.

## NGINX API Gateway to OCI API Gateway

- Route list, upstream services, host/path matching, rewrite rules, and TLS/certificate handling.
- Authentication and authorization model, JWT/OAuth/API keys, mTLS, and custom plugins.
- Rate limits, request/response transforms, headers, CORS, logging, and error handling.
- Dependencies on NGINX modules, Lua scripts, custom filters, or sidecar services.
- Target API Gateway route/deployment design and gaps requiring custom implementation.

## Kafka to OCI Streaming or Managed Kafka

- Kafka version, broker count, topic count, partition count, replication factor, and retention.
- Producer/consumer list, consumer groups, throughput, message size, ordering, and lag.
- Security model: SASL, TLS, ACLs, certificates, and network exposure.
- Connectors, schema registry, transactions, exactly-once assumptions, compaction, and quotas.
- Migration approach: mirror, dual-write, replay, topic recreation, or phased cutover.
- Target choice among OCI Streaming, OCI Streaming with Apache Kafka, and self-managed Kafka.

## Migration Waves and Cutover

- Workload grouping and dependency order.
- Pilot scope and success criteria.
- Cutover window, DNS TTL, endpoint switching, certificate changes, and rollback criteria.
- Data freeze, replication lag threshold, validation checklist, and business signoff.
- Communication plan and incident bridge.

## Security, IAM, Monitoring, Backup, and Operations

- AWS IAM users/roles/policies, secrets, KMS keys, and access patterns.
- Required OCI IAM groups, policies, dynamic groups, vault, and key management.
- Log sources, metrics, dashboards, alarms, tracing, and SIEM integrations.
- Backup policy, retention, restore test, DR requirements, and operational ownership.
- Patch management, vulnerability scanning, incident response, and handover requirements.
