# Lifecycle Operations Reference Guide

**Purpose**: Define and exemplify platform/infrastructure lifecycle operations for reference architecture capability definitions

**Audience**: Enterprise architects, platform engineers, capability promoters

**Key Concept**: A **lifecycle operation** is a concrete, repeatable action phrased as **Verb + Object** that represents how a platform team operates their infrastructure.

---

## What is a Lifecycle Operation?

### Definition

A lifecycle operation is a platform/infrastructure action expressed as **Verb + Object**, with:
- **Clear ownership**: A platform team member or system can execute it
- **Measurable success**: Observable outcome (succeeded or failed)
- **Trackable dependencies**: Prerequisites and impact on other operations
- **Repeatable**: Can be executed multiple times reliably

### Example Lifecycle Operations

✅ **Good** (verb + object):
- Create Kubernetes cluster
- Scale application instances  
- Configure load balancer
- Backup database
- Migrate data to new region
- Monitor application health
- Delete stale snapshots
- Patch security vulnerabilities
- Update resource quotas
- Restore from backup

❌ **Not Lifecycle Operations** (abstract nouns):
- Microservices support
- Database management
- Load balancing (feature)
- Performance optimization (property)
- Cloud infrastructure (entity)
- Auto-scaling capability (feature)
- Security hardening (vague action)
- High availability (goal, not action)

### Why Lifecycle Operations Matter

1. **Actionability**: A capability framed as a lifecycle operation is something a platform team can *do*, not just a feature they have
2. **Accountability**: Each operation has clear ownership and success criteria
3. **Dependencies**: Operations can be tracked, ordered, and managed
4. **Composability**: Complex platform capabilities are built from sequences of lifecycle operations
5. **Governance**: Operations can be validated, audited, and improved over time

---

## Operation Categories

Every lifecycle operation belongs to one of 5 lifecycle domains — the stages describing *when* and *how* a component is operated across its life in production. Reference architectures document, for each Required Component, which capability covers each of these 5 domains (see "Integration with Reference Architectures" below).

### 1. Day 0: Architecture & Infrastructure Provisioning

Initial bootstrapping and deployment of underlying platform foundational resources — the infrastructure a workload will run on.

**Verb Markers**: create, provision, launch, spawn, initialize, setup, bootstrap

**Pattern**: `Create [Foundational Resource Type]` or `Provision [Infrastructure]`

**Examples**:
- Create Kubernetes cluster
- Create VPC (Virtual Private Cloud)
- Create RDS database instance
- Create storage bucket
- Create network security group
- Create EC2 instance
- Provision load balancer
- Create IAM role
- Create subnet
- Bootstrap Terraform state backend

### 2. Day 1: Application & Workload Deployment

How application code and architecture components transition from source artifacts to running workloads on top of Day 0 infrastructure.

**Verb Markers**: deploy, create, configure, initialize, apply, release, roll out

**Pattern**: `Deploy [Workload]` or `Create [Workload Resource]` or `Configure [Workload Setting]`

**Examples**:
- Deploy microservice
- Create Kubernetes deployment
- Create service
- Create namespace
- Create secret
- Create persistent volume
- Configure application environment variables
- Apply Kubernetes manifest
- Create Lambda function
- Roll out application release

### 3. Day 2: Day-to-Day Platform Operations

Continuous operations, scalability, and runtime platform maintenance — the recurring work of keeping a running platform healthy.

**Verb Markers**: scale, configure, backup, restore, monitor, observe, list, get, describe, inspect

**Pattern**: `Scale [Resource Type]`, `Configure [Resource Type] [Setting]`, `Backup [Resource Type]`, `Restore [Resource Type] from [Source]`, or `Monitor [Metric] for [Resource Type]`

**Examples**:
- Scale application instances
- Scale cluster nodes
- Configure load balancer rules
- Configure autoscaling policy
- Backup database
- Restore from backup
- Monitor application health
- Observe request latency
- List all pods in cluster
- Audit access logs

### 4. Maintenance & Upgrade Lifecycles

Manages drift, vulnerabilities, and system evolution over time — keeping the platform current, compliant, and correct.

**Verb Markers**: patch, fix, upgrade, update, migrate, move, validate, verify, remediate

**Pattern**: `Patch [Component]`, `Upgrade [Component] to [Version]`, `Migrate [Resource] to [Destination]`, or `Validate [System Property]`

**Examples**:
- Patch operating system
- Apply security patch
- Upgrade Kubernetes version
- Upgrade database engine
- Migrate data to new region
- Migrate workload to new cluster
- Validate cluster health
- Verify backup integrity
- Remediate misconfiguration
- Test disaster recovery plan

### 5. Day N: Decommissioning & Teardown

Safe removal of deprecated components or ephemeral environment cleanup.

**Verb Markers**: delete, destroy, remove, terminate, drop, decommission, clean, prune

**Pattern**: `Delete [Resource Type]` or `Decommission [Component]`

**Examples**:
- Delete pod
- Destroy cluster
- Decommission Kubernetes cluster
- Remove stale snapshots
- Terminate unused instance
- Drop database user
- Clean up ephemeral environment
- Prune old container images
- Remove unused IAM role
- Tear down staging environment

---

## Domain-Specific Examples

### Kubernetes Domain

**Day 0 (Architecture & Infrastructure Provisioning)**:
- Create Kubernetes cluster
- Create node group
- Provision cluster networking (CNI)
- Create RBAC role
- Bootstrap etcd

**Day 1 (Application & Workload Deployment)**:
- Create namespace
- Create deployment
- Create service
- Create secret
- Create persistent volume
- Create ingress rule
- Apply Kubernetes manifest

**Day 2 (Day-to-Day Platform Operations)**:
- Scale deployment replicas (horizontal scaling)
- Scale stateful set replicas
- Adjust horizontal pod autoscaler min/max
- Backup persistent volume data
- Snapshot cluster etcd database
- Restore pod from backup
- Monitor pod CPU and memory
- Observe deployment rollout status
- Audit RBAC changes
- List all pods in cluster

**Maintenance & Upgrade Lifecycles**:
- Patch Kubernetes master
- Upgrade node OS
- Upgrade Kubernetes version
- Update container image
- Migrate workload to new cluster
- Migrate namespace to different environment
- Validate cluster health

**Day N (Decommissioning & Teardown)**:
- Delete deployment
- Decommission Kubernetes cluster
- Remove namespace
- Drain and remove node
- Prune unused container images

### AWS Domain

**Day 0 (Architecture & Infrastructure Provisioning)**:
- Create VPC
- Create subnet
- Create security group
- Create IAM role
- Provision EC2 instance
- Create EKS/ECS cluster

**Day 1 (Application & Workload Deployment)**:
- Create Lambda function
- Launch Auto Scaling group
- Create RDS database
- Setup S3 bucket
- Deploy application to Elastic Beanstalk
- Create ECS task definition

**Day 2 (Day-to-Day Platform Operations)**:
- Scale Auto Scaling group (increase/decrease capacity)
- Scale RDS read replicas
- Add instances to load balancer pool
- Backup RDS database
- Create AMI (Amazon Machine Image) snapshot
- Restore RDS from snapshot
- Monitor EC2 CPU utilization
- Collect CloudWatch metrics
- Audit IAM policy changes

**Maintenance & Upgrade Lifecycles**:
- Patch security group rules
- Upgrade database engine version
- Update Lambda runtime
- Migrate RDS to new database engine
- Migrate workload to new region
- Validate IAM policy compliance

**Day N (Decommissioning & Teardown)**:
- Terminate EC2 instance
- Decommission RDS database
- Delete unused S3 bucket
- Remove unused IAM role
- Tear down VPC

### Database Domain

**Day 0 (Architecture & Infrastructure Provisioning)**:
- Create database instance
- Create replication slot
- Provision storage volume
- Create backup plan

**Day 1 (Application & Workload Deployment)**:
- Create database
- Create schema
- Create table
- Create index
- Create user account
- Seed initial data

**Day 2 (Day-to-Day Platform Operations)**:
- Scale read replicas
- Expand connection pool
- Backup database
- Create point-in-time backup
- Restore from backup
- Monitor query performance
- Track replication lag
- Monitor disk usage

**Maintenance & Upgrade Lifecycles**:
- Patch database security
- Upgrade database version
- Migrate to new database system
- Migrate to managed database service
- Validate backup integrity
- Update statistics

**Day N (Decommissioning & Teardown)**:
- Drop database
- Drop database user
- Decommission database instance
- Remove replication slot
- Archive and delete transaction logs

### Networking Domain

**Day 0 (Architecture & Infrastructure Provisioning)**:
- Create VPC
- Create subnet
- Create route table
- Create network interface
- Provision load balancer

**Day 1 (Application & Workload Deployment)**:
- Create security group
- Create API gateway
- Create firewall rule
- Configure DNS settings for the workload
- Setup SSL/TLS certificates

**Day 2 (Day-to-Day Platform Operations)**:
- Configure load balancer rules
- Add instances to load balancer
- Scale load balancer capacity
- Enable VPC flow logs
- Monitor network throughput
- Audit firewall changes
- Monitor load balancer health

**Maintenance & Upgrade Lifecycles**:
- Update firewall rules
- Patch network configuration
- Upgrade network security
- Migrate workload to new network
- Migrate DNS to new provider
- Validate network ACL compliance

**Day N (Decommissioning & Teardown)**:
- Delete security group
- Remove firewall rule
- Decommission load balancer
- Tear down VPC peering connection

### Security Domain

**Day 0 (Architecture & Infrastructure Provisioning)**:
- Create encryption key
- Create security policy baseline
- Provision certificate authority

**Day 1 (Application & Workload Deployment)**:
- Create certificate
- Create audit log stream
- Create security group
- Configure authentication for the workload
- Enable encryption

**Day 2 (Day-to-Day Platform Operations)**:
- Configure authorization policies
- Enable audit logging
- Monitor access logs
- Audit authentication events
- Observe failed login attempts
- Collect security metrics

**Maintenance & Upgrade Lifecycles**:
- Patch security vulnerability
- Upgrade encryption algorithm
- Update security certificate
- Apply security policy updates
- Update access control lists
- Validate compliance controls

**Day N (Decommissioning & Teardown)**:
- Revoke and delete encryption key
- Delete expired certificate
- Remove unused security policy
- Decommission certificate authority

---


## Naming Conventions

### General Guidelines

1. **Use active verbs**: Start with action verbs (create, scale, configure, not "creation of", "scaling")
2. **Be specific**: Name the actual resource type (Kubernetes cluster, not "cluster" alone; RDS instance, not "database")
3. **Avoid adjectives**: Don't include size or status (not "big cluster" or "failed deployment", but "scale cluster" or "recover failed deployment")
4. **Avoid abstract concepts**: Focus on concrete operations (not "high availability" but "configure load balancer" or "scale replicas")
5. **Use domain context**: Include domain when ambiguous ("Kubernetes pod" vs "EC2 instance")

### Patterns

**Simple operations**: `Verb Object`
- Create cluster
- Scale instances
- Delete pod

**Scoped operations**: `Verb Object in|of Context`
- Configure firewall rules in VPC
- Scale deployment replicas in namespace
- Monitor CPU of node
- Backup database in region

**Multi-step operations**: `Verb Object with|using Method`
- Migrate data using blue-green deployment
- Restore cluster from backup
- Backup database with incremental snapshots

### Examples of Good Names

| Operation | Why It Works |
|-----------|------------|
| Create Kubernetes cluster | Specific verb, clear resource, domain context |
| Scale application instances | Active verb, countable resource, no adjectives |
| Configure load balancer rules | Concrete action, specific setting, no vagueness |
| Backup database daily | Verb, resource, frequency context |
| Migrate workload to new region | Multi-step captured in name, clear source/dest |
| Monitor pod restart count | Specific metric, clear scope |
| Patch security vulnerability | Action + vulnerability type, specific |

### Examples of Bad Names

| Operation | Why It Fails |
|-----------|------------|
| Auto-scaling | Noun, not verb; abstract feature |
| Database management | Noun, not verb; too vague |
| Performance optimization | Abstract concept, not actionable |
| High availability | Goal, not action; can't execute |
| Microservices support | Feature, not operation |
| Cloud infrastructure | Entity, not action |
| Ensure scalability | Vague; multiple possible actions |

---

## Anti-Patterns: What NOT to Do

### 1. Noun-Only Phrasing

❌ **Bad**:
- Load balancing
- Database management
- Microservices support
- Container orchestration
- Security hardening

✅ **Better**:
- Configure load balancer
- Backup database
- Deploy microservice
- Orchestrate container workload
- Apply security patch

**Why**: Nouns are passive and don't clarify what the platform team actually *does*. Verbs make operations concrete and executable.

### 2. Feature Names as Capabilities

❌ **Bad**:
- Auto-scaling capability
- Multi-region support
- Disaster recovery
- Blue-green deployment
- Zero-downtime deployment

✅ **Better**:
- Scale cluster automatically
- Deploy to multiple regions
- Execute disaster recovery plan
- Perform blue-green deployment
- Deploy without downtime

**Why**: Features are properties of the system. Capabilities are operations the system enables. Naming operations clarifies what the platform team does.

### 3. Overly Broad or Abstract Operations

❌ **Bad**:
- Manage infrastructure
- Operate platform
- Ensure availability
- Maintain system
- Handle operations

✅ **Better**:
- Create Kubernetes cluster
- Scale application instances
- Configure monitoring
- Backup database
- Restore from backup

**Why**: Broad terms are not actionable. Specific operations can be tracked, tested, and automated.

### 4. Passive or Indirect Phrasing

❌ **Bad**:
- Cluster is created
- Instances get scaled
- Security gets hardened
- Backups are performed
- Monitoring is enabled

✅ **Better**:
- Create cluster
- Scale instances
- Patch security vulnerability
- Backup database
- Enable monitoring

**Why**: Active voice is clearer and assigns responsibility. Passive voice obscures who does the work.

### 5. Conditional or Uncertain Operations

❌ **Bad**:
- Maybe scale instances
- Could migrate data
- Might patch vulnerability
- Try to restore from backup
- Attempt recovery

✅ **Better**:
- Scale instances (when load > 80%)
- Migrate data to new region
- Patch critical vulnerability
- Restore from backup
- Recover from node failure

**Why**: Operations are either executed or not; uncertainty belongs in prerequisites or decision logic, not the operation name.

### 6. Mixing Concepts (Command + Process)

❌ **Bad**:
- Implement auto-scaling
- Deploy and verify application
- Backup and verify integrity
- Replicate and monitor data
- Create and configure instance

✅ **Better**:
- Configure auto-scaling
- Deploy application (then: Verify deployment status)
- Backup database (then: Verify backup integrity)
- Replicate data (then: Monitor replication lag)
- Create instance (then: Configure instance)

**Why**: Compound operations should be split into separate, sequenceable operations that can be tested and verified independently.

---

## When to Split Operations

Sometimes an operation seems compound but should be one unit. Use these guidelines:

### Single Operation (Keep Together)
- Components share the same resource and same success criteria
- Examples: "Configure load balancer rules", "Create VPC with subnets"
- Test: Can you verify success with a single assertion?

### Multiple Operations (Split)
- Different resources or different success criteria
- Examples: "Create and configure cluster" → "Create cluster", then "Configure cluster"
- Test: Would you want to execute them at different times?

### Decision Tree

```
Is this operation compound?

├─ YES: Do all sub-steps share the same resource?
│   ├─ YES: Do all succeed/fail together?
│   │   ├─ YES: Keep as one operation (e.g., "Configure load balancer")
│   │   └─ NO: Split into separate operations
│   └─ NO: Split into separate operations (different resources)
└─ NO: Single operation (e.g., "Scale cluster")
```

---

## Integration with Reference Architectures


### Capability Entry (in REFXXXXXX.md)

In a reference architecture Capabilities section, list lifecycle operations:

```markdown
## Capabilities

- Create Kubernetes cluster
- Deploy microservice
- Scale application instances
- Configure load balancer
- Backup database
- Migrate data to new region
- Monitor application health
- Decommission Kubernetes cluster
```

### Capability Artifact (CAPXXXXXX.md)

When a capability is promoted to an artifact, document the operation's execution:

```markdown
# Capability: Create Kubernetes cluster

**Lifecycle Domain**: Day 0: Architecture & Infrastructure Provisioning

## Procedure

### Prerequisites
- [Prerequisite 1]
- [Prerequisite 2]

### Steps
1. [Step 1]
2. [Step 2]
3. [Step 3]

### Validation
- [Check 1]
- [Check 2]

### Rollback
[Rollback procedure if needed]
```

---

## Quick Reference: Operation Verbs by Lifecycle Domain

| Lifecycle Domain | Verbs |
|-------------------|-------|
| **Day 0: Architecture & Infrastructure Provisioning** | create, provision, launch, spawn, initialize, setup, bootstrap |
| **Day 1: Application & Workload Deployment** | deploy, create, configure, initialize, apply, release, roll out |
| **Day 2: Day-to-Day Platform Operations** | scale, configure, backup, restore, monitor, observe, list, get, describe, inspect, audit |
| **Maintenance & Upgrade Lifecycles** | patch, fix, upgrade, update, migrate, move, validate, verify, remediate |
| **Day N: Decommissioning & Teardown** | delete, destroy, remove, terminate, drop, decommission, clean, prune |

---

## Example: Decomposing a Platform into Lifecycle Operations

**Reference Architecture**: "Scalable Microservices Platform on Kubernetes"

**Old (abstract) capabilities**:
- Microservices support
- Scalability
- Monitoring and observability
- Disaster recovery
- Security hardening

**New (lifecycle operations) capabilities**:
- Create Kubernetes cluster
- Deploy microservice
- Scale application instances
- Configure load balancer
- Monitor pod performance
- Backup persistent data
- Restore from backup
- Apply security patch
- Migrate workload to new cluster
- Audit access logs

**Benefits**:
- Platform team can immediately identify what they support
- Operations can be prioritized, estimated, and tracked
- Dependencies between operations become clear
- Training and runbooks can be operation-specific
- Compliance and auditing are more concrete

---

**Last Updated**: 2026-09-02  
**Version**: 2.0 — reorganized around 5 lifecycle domains (Day 0, Day 1, Day 2, Maintenance & Upgrade, Day N)  
**Next Review**: After first 3 reference architectures adopt lifecycle operations pattern
