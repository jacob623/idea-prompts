# Platform Engineering Architecture

This document defines the authoritative architecture boundaries of the Enterprise Platform Engineering ecosystem. Each platform serves as the system of record for its explicitly assigned responsibilities, capabilities, decisions, and data domains.

When referencing a platform:
- Treat each platform as the authoritative system of record for its assigned responsibilities, capabilities, decisions, and data domains.
- Architecture boundaries are defined by the responsibilities assigned to each platform.
- Explicitly listed responsibilities are authoritative and define each platform's architecture boundaries, ownership domains, and primary capabilities.
- If a responsibility is not explicitly assigned to a platform, determine the most appropriate platform based on the documented responsibilities and architectural role definitions.
- Any inferred ownership must be clearly identified as an inference and not as a documented assignment.
- Any inferred ownership must include a rationale that references the documented responsibilities of the selected platform.
- If both an Architecture Boundary statement and a Responsibility match a requested capability, the Responsibility takes precedence.
- Use this mapping when determining integration points, operational ownership, lifecycle management, security controls, and troubleshooting paths.
- Do not create new platform responsibilities unless explicitly requested.
- If no reasonable ownership mapping exists, identify the responsibility as unassigned rather than asserting ownership.

# Platforms

## Okta

### Architecture Boundary

Okta is the authoritative source for identity, authentication, federation, and role-based access control.

### Authoritative Responsibilities
- User Authentication
- Identity Federation
- Single Sign-On (SSO)
- Role-Based Access Control (RBAC)
- Group Membership Management
- Entitlement Evaluation


## Red Hat Developer Hub (RHDH)

### Architecture Boundary

RHDH is the authoritative self-service interface for request initiation and developer interactions.

### Authoritative Responsibilities
- Developer Portal
- Software Templates
- Self-Service Experience
- Request Intake
- Workflow Initiation
- Trace ID Generation
- Request Parameter Collection


## Red Hat Developer Orchestrator

### Architecture Boundary

The Orchestrator is the authoritative source for workflow coordination, execution control, and platform integration.

### Authoritative Responsibilities
- Workflow Orchestration
- Execution Sequencing
- Workflow State Management
- Platform Integration
- Exception Handling
- Manual Resume Operations
- Evidence Assembly
- Traceability Coordination


## Open Policy Agent (OPA)

### Architecture Boundary

OPA is the authoritative source for governance policy evaluation and compliance decisions.

### Authoritative Responsibilities
- Policy Evaluation
- Compliance Validation
- Governance Enforcement
- Prerequisite Gating
- Policy Decision Processing
- Policy Decision Receipts


## PostgreSQL

### Architecture Boundary

PostgreSQL is the authoritative source for workflow execution metadata and operational state.

### Enterprise Configuration Authority

PostgreSQL is the authoritative source for enterprise configuration data used to enrich workflow requests prior to policy evaluation and provisioning.

Enterprise configuration data includes:

- Environment Configuration
- Platform Configuration
- Resource Configuration
- Automation Platform Mapping
- Terraform Module Mapping
- Naming Standards
- Policy Assignment Metadata

Enterprise configuration data SHALL be used by the Red Hat Developer Orchestrator to construct the Canonical Request.

### Authoritative Responsibilities
- Workflow Execution Records
- Execution Tracking
- Workflow State Persistence
- Metadata Storage
- Entity Relationship Management
- Execution History
- Audit Metadata Repository
- Enterprise Configuration Repository
- Configuration Profile Management
- Resource Mapping Repository
- Environment Mapping Repository
- Automation Platform Mapping
- CMDB Resource CI Repository
- CMDB Provision Event Repository
- CMDB Resource Relationship Repository
- CMDB Operational Event Repository


## HCP Terraform

### Architecture Boundary

HCP Terraform is the authoritative source for declarative infrastructure provisioning and infrastructure state.

### Authoritative Responsibilities
- Infrastructure as Code (IaC)
- Infrastructure Provisioning
- Infrastructure State Management
- Resource Tagging
- Terraform Run Management


## Event Driven Ansible (EDA)

### Architecture Boundary

EDA is the authoritative source for event-driven automation routing and trigger evaluation.

### Authoritative Responsibilities
- Event Processing
- Webhook Intake
- Event Correlation
- Rulebook Evaluation
- Automation Trigger Selection
- Event Routing


## Ansible Automation Platform (AAP)

### Architecture Boundary

AAP is the authoritative source for imperative automation and post-provisioning configuration activities.

### Authoritative Responsibilities
- Configuration Management
- Post-Provisioning Automation
- System Hardening
- Agent Deployment
- Application Onboarding
- Service Registration


## S3

### Architecture Boundary

S3 is the authoritative repository for immutable evidence and audit artifacts.

### Authoritative Responsibilities
- Object Storage
- Immutable Artifact Storage
- Compliance Evidence
- Audit Retention
- Deployment Evidence Repository
- Audit Artifact Retention


## Dynatrace

### Architecture Boundary

Dynatrace is the authoritative source for operational metrics, telemetry, and performance observability.

### Authoritative Responsibilities
- Metrics Collection
- Operational Telemetry
- Performance Monitoring
- Time-to-Delivery (TTD) Reporting
- Execution Metrics Analytics
- Observability Dashboards


## Splunk

### Architecture Boundary

Splunk is the authoritative source for centralized logging, search, and execution traceability.

### Authoritative Responsibilities
- Log Aggregation
- Centralized Audit Logging
- Trace Correlation
- Cross-Platform Log Search
- Execution Observability
- Log Analytics