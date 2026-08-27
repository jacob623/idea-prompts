# Software Template Standard

## Purpose

Defines the authoritative structure and lifecycle requirements for self-service templates.

## Required Template Metadata

```yaml
template:
  id:
  name:
  version:
  owner:
  description:
  resource_type:
  approval_status:
```

## Versioning

Templates SHALL follow Semantic Versioning.

## Input Parameters

Every parameter SHALL define:

```yaml
parameter:
  name:
  type:
  required:
  default:
  validation:
```

## User Intent Principle

Software Templates SHALL collect business intent and user requirements.

Templates SHALL NOT require users to provide enterprise implementation attributes.

Examples of prohibited implementation attributes:

- VMware Cluster
- VMware Datastore
- NSX Transport Zone
- Palo Alto Device Group
- Palo Alto Rulebase
- ZScaler Policy Container
- Active Directory Organizational Unit

These values SHALL be resolved through the Enterprise Configuration Model.

## Supported Types

- string
- integer
- boolean
- array
- object

## Validation

Parameters SHALL support:

- Regex validation
- Allowed values
- Length limits
- Required field validation

## Approval Status

Allowed Values:

- Draft
- Approved
- Deprecated
- Retired

Only Approved templates MAY be executed through RHDH.