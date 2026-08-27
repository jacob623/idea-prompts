# Automation Catalog Standard

## Purpose

The Automation Catalog Standard defines authoritative mappings between resource types and post-provisioning automation activities.

The Automation Catalog SHALL determine which EDA rulebooks and AAP job templates are executed.

## Architectural Principles

### ACS-1 Catalog Authority

The Automation Catalog SHALL be the authoritative source for automation selection.

### ACS-2 Deterministic Selection

The same:

- Resource Type
- Environment
- Catalog Version

SHALL result in the same set of automation activities.

### ACS-3 Catalog Governance

Only approved automation definitions MAY be executed.

## Automation Definition

automation_catalog_entry:
  automation_id:
  resource_type:
  environment:
  eda_rulebook:
  aap_job_templates:
  version:
  owner:
  approval_status:

## Supported Activities

Allowed Values:

- os_hardening
- agent_installation
- application_onboarding
- service_registration
- compliance_validation

## Job Template Definition

job_template:
  template_id:
  template_name:
  execution_order:
  required:
  version:

## Execution Ordering

Automation activities SHALL define execution order.

Example:

1. OS Hardening
2. Agent Installation
3. Service Registration
4. Application Onboarding

## Resource Mapping Matrix

| Resource Type | Activity |
|---------------|----------|
| VM | OS Hardening |
| VM | Agent Installation |
| VM | Service Registration |
| Kubernetes | Agent Installation |
| Kubernetes | Service Registration |
| Database | Agent Installation |
| Database | Service Registration |

## Environment Rules

Example:

environment_rules:
  prod:
    compliance_validation: required

  dev:
    compliance_validation: optional

## Approval Status

Allowed Values:

- Draft
- Approved
- Deprecated
- Retired

Only Approved catalog entries MAY be executed.

## Audit Requirements

automation_execution:
  trace_id:
  automation_id:
  eda_rulebook:
  executed_templates:
  timestamp:

# Automation Catalog Job Templates

## SailPoint AD Group Creation

automation_catalog_entry:
  automation_id: ad-group-create

job_templates:
  - validate_request
  - create_ad_group
  - update_sailpoint
  - audit_record

## SailPoint AD User Creation

automation_catalog_entry:
  automation_id: ad-user-create

job_templates:
  - validate_hr_attributes
  - create_ad_account
  - update_sailpoint
  - notification

## SailPoint AD Group Membership

automation_catalog_entry:
  automation_id: ad-group-membership

job_templates:
  - validate_membership
  - update_group
  - update_sailpoint

## CyberArk Safe Creation

automation_catalog_entry:
  automation_id: cyberark-safe-create

job_templates:
  - validate_request
  - create_safe
  - assign_owners
  - evidence_collection
