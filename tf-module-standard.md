# Terraform Module Standard

## Purpose

The Terraform Module Standard defines governance, ownership, approval, and lifecycle requirements for Terraform modules used by Platform Engineering workflows.

## Architectural Principles

### TMS-1 Approved Module Usage

Provisioning SHALL use approved modules whenever available.

### TMS-2 Single Module Authority

Each approved capability SHALL have an authoritative Terraform module.

### TMS-3 Version Traceability

Provisioning executions SHALL record the module version used.

## Module Definition

terraform_module:
  module_id:
  module_name:
  version:
  owner:
  resource_type:
  approval_status:
  repository:
  documentation_uri:

## Approved Module Requirements

Every approved module SHALL:

- Have an owner
- Have documentation
- Have version control
- Pass security review
- Pass policy validation
- Pass integration testing

## Module Status

Allowed Values:

- Draft
- Approved
- Deprecated
- Retired

Only Approved modules MAY be used for automated provisioning.

## Module Mapping

Every catalog offering SHALL map to exactly one approved module.

Example:

module_mapping:
  resource_type: vm
  catalog_entry: standard-linux
  module: terraform-vm-linux

## Version Selection

### TMS-1 Default Version

The latest approved module version SHALL be used unless explicitly specified.

### TMS-2 Deprecated Versions

Deprecated versions SHALL NOT be selected for new deployments.

## Module Testing

Every release SHALL validate:

- Syntax validation
- Security validation
- Policy validation
- Integration validation

## Audit Requirements

module_execution:
  trace_id:
  module_name:
  module_version:
  terraform_run_id:
  timestamp:

# Terraform Modules

## VMware VM

terraform_module:
  module_name: vmware-vsphere-vm
  resource_type: virtual-machine
  provider: vmware
  lifecycle_owner: Virtualization Team

supported_operations:
  - create
  - update
  - destroy

## NSX Rule

terraform_module:
  module_name: nsx-distributed-firewall-rule
  resource_type: network-security-rule

supported_operations:
  - create
  - modify
  - remove

## Palo Alto Firewall Rule

terraform_module:
  module_name: paloalto-security-rule

supported_operations:
  - create
  - modify
  - remove

## ZScaler Policy

terraform_module:
  module_name: zscaler-policy

supported_operations:
  - create
  - modify
  - remove

## Infoblox Network

terraform_module:
  module_name: infoblox-network

## Infoblox IP Reservation

terraform_module:
  module_name: infoblox-fixed-address

## Infoblox DNS Record

terraform_module:
  module_name: infoblox-dns-record

## F5 Load Balancer

resource_catalog_entry:
  resource_id: f5-virtual-server
  resource_type: load-balancer
  provider: f5
  automation_platform: terraform
  terraform_module: f5-virtual-server

required_attributes:
  - vip
  - pool_members
  - service_port

## VSphere DataStore Creation

terraform_module:
  module_name: vmware-datastore

## F5 Load Balancer

terraform_module:
  module_name: f5-virtual-server