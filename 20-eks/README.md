# 20-eks — Kubernetes Platform (Amazon EKS)

This stack establishes the **managed Kubernetes platform** used to run modernized
application workloads as part of the enterprise cloud modernization program.

It builds on the Cloud Foundation (`10-foundation`) and provides a secure,
scalable, and operationally sound execution environment for containerized
applications.

---

## Purpose

The Kubernetes Platform stack delivers a production-ready Amazon EKS environment
that:

- Standardizes how container workloads are deployed and operated
- Abstracts infrastructure complexity from application teams
- Enforces security, identity, and access controls at the platform layer
- Enables consistent deployment patterns across environments

This stack is designed to be owned by a **platform or infrastructure team** and
consumed by application teams.

---

## Scope

### In Scope
This stack typically includes:

- Amazon EKS control plane
- Managed node groups (or equivalent compute abstraction)
- Cluster networking configuration
- IAM Roles for Service Accounts (IRSA)
- Core EKS add-ons and baseline platform components
- Integration with the foundational VPC and subnets

### Out of Scope
The following are intentionally excluded:

- Application deployments
- CI/CD pipeline implementation
- Application-specific Kubernetes manifests
- Stateful data services (databases, queues, etc.)

These concerns are handled in downstream stacks.

---

## Design Principles

- **Platform-first ownership**  
  The Kubernetes cluster is a shared platform, not an application artifact.

- **Private-by-default networking**  
  Worker nodes and workloads run in private subnets.

- **Identity-driven access**  
  IAM is integrated with Kubernetes using IRSA to avoid static credentials.

- **Least privilege by design**  
  Cluster and workload permissions are explicitly scoped.

- **Composable platform**  
  Outputs are exported for use by application and observability layers.

---

## Architecture Overview

At a high level, this stack provisions:

- An Amazon EKS cluster integrated into the foundational VPC
- Managed node groups distributed across availability zones
- Cluster networking aligned with enterprise segmentation standards
- Identity integration enabling workload-level IAM access via IRSA

The platform is designed to support:
- Multiple application namespaces
- Horizontal scaling
- Controlled ingress and egress
- Centralized observability and security tooling

---

## Key Outputs

This stack exposes outputs required by downstream layers, including:

- EKS cluster name
- Cluster endpoint
- Cluster OIDC provider information
- Node group identifiers
- Kubernetes provider connection details

These outputs are consumed by:
- `40-app` (application deployment)
- `50-observability-security` (logging, metrics, security agents)

---

## Terraform Structure

This stack is implemented as an **independent Terraform state** with:

- Dedicated backend configuration
- Provider and module version pinning
- Explicit variables for environment-specific inputs
- Explicit outputs for platform integration

Typical files include:

- `backend.tf` — Remote state configuration (placeholders only)
- `providers.tf` — Terraform and AWS provider constraints
- `variables.tf` — Environment and platform inputs
- `main.tf` — Core EKS and node group resources
- `outputs.tf` — Platform outputs

---

## Deployment Guidance

### Prerequisites
- Cloud Foundation stack (`10-foundation`) successfully applied
- Terraform installed (>= 1.6)
- IAM permissions to create EKS and related resources
- Environment-specific variables defined externally

### Apply Process
1. Initialize Terraform backend
2. Validate inputs (VPC, subnets, environment identifiers)
3. Review plan output carefully (cluster changes are high-impact)
4. Apply once per environment
5. Validate cluster access and health post-deployment

> ⚠️ Platform changes affect all workloads.  
> Treat this stack as shared infrastructure and apply change controls accordingly.

---

## Operational Considerations

- Cluster upgrades should be planned and tested per environment
- Node group scaling policies should align with workload profiles
- Access to the cluster should be tightly controlled and audited
- Platform-level add-ons should be standardized and versioned

---

## Relationship to the Modernization Factory

This stack represents the **platform enablement phase** of the modernization
factory.

It bridges the gap between foundational infrastructure and application delivery,
allowing teams to move from infrastructure provisioning to rapid, repeatable
application deployment.

A well-designed Kubernetes platform:
- Accelerates application modernization
- Reduces operational friction
- Improves security consistency
- Enables scalable delivery across teams

---

## Notes

- All values in this stack are placeholders by design
- No secrets or account-specific identifiers are committed
- This implementation prioritizes platform stability, security, and extensibility
