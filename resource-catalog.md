# Resource Catalog Standard

## Purpose

The Resource Catalog Standard defines the authoritative inventory of approved resource offerings available through Platform Engineering self-service workflows.

Only catalog-approved resources MAY be provisioned automatically.

## Architectural Principles

### RCS-1 Catalog Authority

The Resource Catalog SHALL be the authoritative source for approved resource offerings.

### RCS-2 Standardization

Infrastructure requests SHALL be fulfilled using catalog-defined offerings whenever available.

### RCS-3 Deterministic Selection

The same:

- Template
- Resource Type
- Environment

SHALL resolve to the same catalog entry.

## Resource Categories

Allowed Values:

- vm
- kubernetes-cluster
- database
- object-storage
- network

## Catalog Entry Definition

resource_catalog_entry:
  resource_id:
  resource_type:
  offering_name:
  version:
  owner:
  approval_status:
  terraform_module:
  supported_environments:
  supported_regions:
  configuration_schema:

## Virtual Machine Catalog

vm_offering:
  cpu:
  memory:
  operating_system:
  backup_profile:
  patch_profile:

## Database Catalog

database_offering:
  engine:
  version:
  high_availability:
  backup_profile:
  encryption:

## Kubernetes Catalog

cluster_offering:
  kubernetes_version:
  worker_profile:
  networking_profile:
  monitoring_profile:

## Environment Eligibility

Allowed Values:

- dev
- test
- stage
- prod

## Approval Status

Allowed Values:

- Draft
- Approved
- Deprecated
- Retired

Only Approved entries MAY be provisioned automatically.

## Catalog Lifecycle

Catalog entries SHALL follow Semantic Versioning.

## Unsupported Requests

If no approved catalog entry exists:

- Automatic provisioning SHALL stop.
- Manual review SHALL be required.
- Approval SHALL be recorded.

## Audit Requirements

Every provisioning request SHALL record:

catalog_selection:
  trace_id:
  resource_id:
  version:
  timestamp:

# Resource Catalog Entries

## VMware VM

resource_catalog_entry:
  resource_id: vmware-vm
  resource_type: virtual-machine
  provider: vmware
  automation_platform: terraform
  terraform_module: vmware-vsphere-vm
  approval_status: Approved

required_attributes:
  - vm_name
  - environment
  - cpu
  - memory
  - datastore
  - network
  - operating_system

## NSX Rule

resource_catalog_entry:
resource_id: nsx-security-rule
resource_type: network-security-rule
provider: nsx
automation_platform: terraform
terraform_module: nsx-distributed-firewall-rule

required_attributes:
- source_groups
- destination_groups
- services
- action

## Palo Alto Firewall Rule

resource_catalog_entry:
  resource_id: palo-alto-firewall-rule
  resource_type: firewall-rule
  provider: palo-alto
  automation_platform: terraform
  terraform_module: paloalto-security-rule

required_attributes:
  - source_zone
  - destination_zone
  - source_addresses
  - destination_addresses
  - applications
  - services
  - action

## ZScaler Policy

resource_catalog_entry:
  resource_id: zscaler-policy
  resource_type: internet-security-policy
  provider: zscaler
  automation_platform: terraform
  terraform_module: zscaler-policy

required_attributes:
  - policy_name
  - users
  - applications
  - action

## Infoblox Network

resource_catalog_entry:
  resource_id: infoblox-network
  resource_type: network
  provider: infoblox
  automation_platform: terraform
  terraform_module: infoblox-network

required_attributes:
  - network
  - cidr
  - network_view

## Infoblox IP Reservation

resource_catalog_entry:
  resource_id: infoblox-ip-reservation
  resource_type: ip-address
  provider: infoblox
  automation_platform: terraform
  terraform_module: infoblox-fixed-address

required_attributes:
  - ip_address
  - mac_address
  - network

## Infoblox DNS Record

resource_catalog_entry:
  resource_id: infoblox-dns-record
  resource_type: dns-record
  provider: infoblox
  automation_platform: terraform
  terraform_module: infoblox-dns-record

required_attributes:
  - fqdn
  - record_type
  - target

## SailPoint AD Group Creation

resource_catalog_entry:
  resource_id: ad-group
  resource_type: active-directory-group
  provider: sailpoint
  automation_platform: aap

required_attributes:
  - group_name
  - description
  - owner

## SailPoint AD User Creation

resource_catalog_entry:
  resource_id: ad-user
  resource_type: active-directory-user
  provider: sailpoint
  automation_platform: aap

required_attributes:
  - first_name
  - last_name
  - manager
  - department

## SailPoint AD Group Membership

resource_catalog_entry:
  resource_id: ad-group-membership
  resource_type: group-membership
  provider: sailpoint
  automation_platform: aap

required_attributes:
  - user_id
  - group_name
  - action

## VSphere DataStore Creation

resource_catalog_entry:
  resource_id: vsphere-datastore
  resource_type: datastore
  provider: vmware
  automation_platform: terraform
  terraform_module: vmware-datastore

## CyberArk Safe Creation

resource_catalog_entry:
  resource_id: cyberark-safe
  resource_type: privileged-access-safe
  provider: cyberark
  automation_platform: aap

required_attributes:
  - safe_name
  - owner
  - retention_policy