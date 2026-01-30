# 30-data — Data Services Layer

This stack provisions the **stateful data services** required by modernized
applications, with a strong emphasis on **security, isolation, availability,
and blast-radius control**.

It builds on the Cloud Foundation (`10-foundation`) and Kubernetes Platform
(`20-eks`) stacks and is intentionally designed to be **independently managed**
from application delivery.

---

## Purpose

The Data Services stack delivers a secure and scalable data layer that:

- Supports application workloads without embedding data concerns into app stacks
- Enforces strict network and access boundaries
- Aligns with enterprise data protection and compliance expectations
- Minimizes operational and security blast radius for stateful components

This stack is typically owned by a **platform, data, or infrastructure team** and
consumed by application teams via well-defined interfaces.

---

## Scope

### In Scope
This stack typically includes:

- Managed relational database services (e.g., Amazon Aurora)
- Database subnet groups and network placement
- Security groups and access control boundaries
- Secrets Manager placeholders for credentials
- Supporting IAM policies for controlled access

### Out of Scope
The following are intentionally excluded:

- Application-level schema management
- Data migration tooling or one-time cutover scripts
- Application configuration or secrets injection
- Backup and restore automation beyond managed service defaults

These concerns are addressed separately to preserve separation of duties.

---

## Design Principles

- **Isolation by default**  
  Data services run in private subnets with no direct public access.

- **Least privilege access**  
  Only explicitly authorized workloads or identities may connect to data services.

- **Blast-radius containment**  
  Stateful resources are isolated from application and platform stacks.

- **Managed over self-hosted**  
  Preference is given to managed services to reduce operational overhead.

- **Explicit contracts**  
  Access patterns are enforced via security groups, IAM, and secrets references.

---

## Architecture Overview

At a high level, this stack provisions:

- One or more managed database clusters deployed into private subnets
- Network security rules restricting access to known platform components
- Secrets placeholders for database credentials and connection details
- IAM constructs enabling controlled access from Kubernetes workloads (via IRSA)

The data layer is intentionally **opaque** to application teams:
- Applications consume endpoints and credentials
- Infrastructure teams manage availability, scaling, and patching

---

## Key Outputs

This stack exposes outputs required by downstream layers, including:

- Database endpoints
- Database identifiers
- Security group references
- Secrets Manager resource ARNs

These outputs are typically consumed by:
- `40-app` (application deployment and configuration)
- `50-observability-security` (monitoring and auditing)

---

## Terraform Structure

This stack is implemented as an **independent Terraform state**, featuring:

- Dedicated backend configuration
- Explicit provider version pinning
- Clearly defined input variables
- Explicit outputs for controlled downstream use

Typical files include:

- `backend.tf` — Remote state configuration (placeholders only)
- `providers.tf` — Terraform and provider constraints
- `variables.tf` — Environment and data-layer inputs
- `main.tf` — Core data service resources
- `outputs.tf` — Exported connection references

---

## Deployment Guidance

### Prerequisites
- Cloud Foundation (`10-foundation`) applied
- Kubernetes Platform (`20-eks`) available (for access integration)
- Terraform installed (>= 1.6)
- Environment-specific variables defined externally

### Apply Process
1. Initialize Terraform backend
2. Validate network placement and access rules
3. Review plan carefully (stateful changes are high-impact)
4. Apply once per environment
5. Validate connectivity from approved platform components only

> ⚠️ Data services have high blast radius.  
> Treat changes to this stack with elevated change control and review.

---

## Operational Considerations

- Database scaling and maintenance should follow managed service best practices
- Access patterns should be reviewed regularly
- Secrets rotation should be handled via managed or external mechanisms
- Decommissioning requires explicit coordination with application teams

---

## Relationship to the Modernization Factory

This stack represents the **stateful services phase** of the modernization factory.

By isolating data services:
- Application teams iterate faster
- Security boundaries remain enforceable
- Failures and changes are contained
- Compliance posture is easier to maintain

A disciplined data layer is critical to long-term platform stability.

---

## Notes

- All values in this stack are placeholders by design
- No secrets or account-specific identifiers are committed
- This implementation prioritizes security, isolation, and enterprise operability
