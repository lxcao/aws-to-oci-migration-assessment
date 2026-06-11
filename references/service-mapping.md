# AWS to OCI Service Mapping

Use this as a starting point only. Verify current service names, limits, regional availability, and migration tooling against Oracle official documentation before finalizing customer-facing artifacts.

## Mapping Matrix

| AWS Source | Common OCI Target Options | Key Assessment Points |
|---|---|---|
| VPC, Subnets, Route Tables, Security Groups, NACLs, NAT Gateway, Transit Gateway | OCI VCN, regional subnets, route tables, security lists, NSGs, NAT Gateway, DRG, LPG, Service Gateway | CIDR overlap, route propagation, segmentation, east-west controls, internet exposure, private service access, hybrid connectivity, DNS |
| Elastic Load Balancing: ALB/NLB/CLB | OCI Load Balancer, OCI Network Load Balancer | L7/L4 protocol, TLS termination, SNI, path/host routing, health checks, session persistence, idle timeout, certificates, WAF integration |
| ECS on EC2 or Fargate | OCI Container Engine for Kubernetes (OKE), OCI Container Instances, OCI Compute with container runtime | Orchestration model, task/service definitions, image registry, secrets, service discovery, autoscaling, deployment strategy, observability, Fargate dependencies |
| EC2 with EBS | OCI Compute, boot volumes, block volumes, instance pools, autoscaling | OS support, image conversion, CPU architecture, shape sizing, block volume performance, boot method, licensing, agents, startup scripts |
| S3 | OCI Object Storage, Archive Storage | API compatibility, bucket policy, IAM model, lifecycle, object lock, versioning, encryption, events, replication, data transfer volume, application SDK behavior |
| RDS MySQL | OCI MySQL HeatWave, customer-managed MySQL on Compute, Oracle Database migration only if replatforming is in scope | MySQL version, storage engine, extensions, parameter groups, HA/backup, replication, maintenance window, performance, SQL compatibility, downtime tolerance |
| ElastiCache for Redis | OCI Cache with Redis, self-managed Redis on Compute | Redis version, cluster mode, persistence, failover, TLS/auth, memory sizing, eviction policy, endpoint change, client compatibility |
| EFS | OCI File Storage | NFS version, mount targets, throughput, file count, permissions, POSIX behavior, backup, replication, mount topology, client OS |
| Self-managed NGINX API Gateway | OCI API Gateway, OCI Load Balancer + NGINX, OKE ingress gateway | Route model, authentication, authorization, plugins/modules, rate limiting, request/response transforms, TLS, custom logic |
| Self-managed Kafka | OCI Streaming, OCI Streaming with Apache Kafka, self-managed Kafka on Compute/OKE | Kafka API compatibility, partitions, retention, consumer groups, throughput, ordering, schema registry, connectors, exactly-once assumptions |
| RabbitMQ, Nacos, Cassandra, other middleware on EC2 | OCI Compute/OKE self-managed, OCI managed alternatives where available | Stateful behavior, data replication, clustering, ports, storage latency, backup, support model, licensing, operational ownership |

## Target Selection Guidance

Prefer a managed OCI PaaS service when:

- The target service supports the required protocol and operational model.
- Customer accepts service-specific limits and IAM/networking changes.
- Reducing operations burden is a migration goal.

Prefer self-managed on OCI Compute or OKE when:

- The workload depends on custom plugins, unsupported protocol features, kernel modules, or exact version pinning.
- The managed service lacks required regional availability or capacity.
- Migration downtime or behavior change risk is lower with lift-and-shift first.

Use phased migration when:

- There are many unknown dependencies.
- The customer needs a low-risk first wave.
- A managed-service migration requires application code or integration changes.

## Migration Tooling Candidates

Use the following as candidates, not promises:

- OCI Cloud Migrations for supported VM migration scenarios.
- OCI Database Migration, MySQL Shell, logical dump/restore, replication, or native MySQL tools for MySQL migration depending on source and target.
- Native object-copy tools, Rclone, application-level rehydration, or offline transfer for object storage.
- Rsync, NFS copy tools, backup/restore, or storage replication patterns for file storage.
- Container image registry replication and CI/CD redeployment for container workloads.
- Terraform or OCI Resource Manager for repeatable landing zone and target resource provisioning.

## Common Anti-Patterns

- Treating AWS Security Groups and OCI Security Lists as identical. OCI NSGs are usually a closer workload-level control.
- Copying AWS CIDR design without checking OCI VCN growth, hybrid connectivity, and overlap.
- Assuming ALB path rules, WAF behavior, TLS policies, and health checks map one-to-one.
- Migrating ECS to Kubernetes without validating team readiness and delivery pipeline changes.
- Moving stateful middleware to a managed service without validating protocol features and client behavior.
- Writing a SOW before risk, dependencies, and out-of-scope items have been made explicit.
