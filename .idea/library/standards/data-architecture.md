# Data Architecture

All reference architectures, platform capabilities, and self-service solutions must comply
with the applicable DAMA-DMBOK2 knowledge area obligations defined below.

## Data Governance

Aligned to DAMA-DMBOK2 — Data Governance.

Every data domain produced or consumed by a platform capability must have a designated
authoritative owner. Ownership assignments defined in `.idea/library/standards/enterprise-architecture.md`
are authoritative and must not be overridden without an approved ADR.

## Data Architecture

Aligned to DAMA-DMBOK2 — Data Architecture.

Every reference architecture must identify the authoritative data store for each data domain
it produces, referencing the platform authority boundaries defined in
`.idea/library/standards/enterprise-architecture.md`. Data must not be written to a platform
that is not designated as the authoritative owner for that domain.

## Data Storage and Operations

Aligned to DAMA-DMBOK2 — Data Storage and Operations.

All data stored by a platform capability must have a documented retention period and a tested
recovery procedure with explicit RPO and RTO targets. Platform storage authority assignments
in `.idea/library/standards/enterprise-architecture.md` determine which system holds each
data domain at rest.

## Data Security

Aligned to DAMA-DMBOK2 — Data Security.

All data must be classified at one of the following tiers: Public, Internal, Confidential,
or Restricted. Data classified Confidential or Restricted must be encrypted at rest and in
transit. Access must be restricted to roles with a documented need, enforced through the
identity and access controls governed by Okta per `.idea/library/standards/enterprise-architecture.md`.

## Metadata Management

Aligned to DAMA-DMBOK2 — Metadata Management.

All workflow execution metadata, canonical request payloads, and audit records must be
persisted to the authoritative metadata store defined in
`.idea/library/standards/enterprise-architecture.md`. Metadata must be sufficient to
reconstruct the full execution trace for any workflow instance.
