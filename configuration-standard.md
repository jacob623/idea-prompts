# Enterprise Configuration Standard

## Purpose

The Enterprise Configuration Standard defines the authoritative enterprise configuration model used throughout the Platform Engineering ecosystem.

This standard establishes the management, ownership, versioning, and governance requirements for enterprise configuration data used during workflow execution, policy evaluation, and automation selection.

Enterprise configuration data SHALL be maintained separately from workflow code and SHALL be resolved dynamically during workflow execution.

## Scope

This standard applies to:

- Red Hat Developer Hub (RHDH)
- Red Hat Developer Orchestrator
- Open Policy Agent (OPA)
- HCP Terraform
- Event Driven Ansible (EDA)
- Ansible Automation Platform (AAP)
- PostgreSQL

This standard also applies to all self-service offerings, resource catalogs, automation catalogs, and provisioning workflows.

## Architectural Principles

### ECS-1 Enterprise Configuration Authority

PostgreSQL SHALL be the authoritative source for enterprise configuration data.

### ECS-2 Configuration As Data

Enterprise implementation decisions SHALL be maintained as configuration data rather than workflow code whenever possible.

### ECS-3 Deterministic Resolution

The same:

- Resource Type
- Environment
- Template
- Configuration Profile Version

SHALL resolve to the same enterprise configuration values.

### ECS-4 Separation of Intent and Configuration

User intent SHALL remain separate from enterprise-resolved configuration.

### ECS-5 Versioned Configuration

All enterprise configuration SHALL be version controlled.

### ECS-6 Traceability

Configuration versions used during execution SHALL be recorded within:

- Workflow Metadata
- Audit Records
- Evidence Packages

### ECS-7 Reproducibility

Authorized auditors SHALL be able to reconstruct the configuration context associated with a workflow execution.

## Enterprise Configuration Domains

The Enterprise Configuration repository SHALL support:

- Environment Configuration
- Platform Configuration
- Resource Configuration
- Naming Standards
- Policy Assignment
- Module Assignment
- Automation Assignment
- Integration Routing
- Organizational Mapping

## Core Entity Model

### Configuration Profile

Defines a deployment context.

configuration_profile:
  profile_id:
  profile_name:
  resource_type:
  environment:
  version:
  status:

### Configuration Attribute

Defines configuration values associated with a profile.

configuration_attribute:
  profile_id:
  attribute_name:
  attribute_value:

### Resolution Definition

Defines how enterprise attributes are resolved.

resolution_definition:
  resolution_id:
  resource_type:
  attribute_name:
  source_entity:
  source_attribute:
  lookup_keys:
  required:

### Canonical Mapping

Defines how resolved attributes populate the Canonical Request.

canonical_mapping:
  mapping_id:
  resource_type:
  source_attribute:
  target_path:
  required:

## Configuration Versioning

### ECS-V1 Semantic Versioning

Configuration Profiles SHALL follow Semantic Versioning.

Example:

1.0.0
1.1.0
2.0.0

### ECS-V2 Historical Preservation

Historical configuration SHALL remain associated with completed workflow executions.

Configuration updates SHALL NOT modify historical workflow records.

## Configuration Resolution Lifecycle

Workflow executions SHALL resolve enterprise configuration using the following sequence:

Request
    ↓
Configuration Profile Selection
    ↓
Configuration Resolution
    ↓
Canonical Request Assembly
    ↓
Policy Evaluation
    ↓
Provisioning

## Metadata Requirements

Every workflow execution SHALL record:

configuration_context:
  profile_id:
  profile_version:
  resolution_timestamp:

## Audit Requirements

### ECS-A1 Traceability

Configuration context SHALL remain associated with the originating Trace_ID.

### ECS-A2 Reconstructability

Authorized auditors SHALL be able to reconstruct the effective configuration used during workflow execution.

### ECS-A3 Historical Retention

Historical configuration versions SHALL remain available for audit review.

## Compliance Requirements

Workflow implementations SHALL NOT:

- Hardcode enterprise configuration values.
- Perform undocumented configuration resolution.
- Modify resolved configuration during execution.
- Bypass enterprise configuration resolution.

All participating platforms SHALL consume enterprise configuration through the Canonical Request.