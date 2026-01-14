<!-- Code-First Deep Threat Modeling Workflow | Version 2.1.0 | https://github.com/fr33d3m0n/skill-threat-modeling | License: BSD-3-Clause | Welcome to cite but please retain all sources and declarations -->

# Control Set 12: Infrastructure Security (INFRA)

**Domain**: INFRA - Infrastructure Security
**Version**: 1.0
**Last Updated**: 2025-12-30

---

## Overview

Infrastructure security control set, applicable to containerized, orchestrated, and IaC environments. Automatically loaded when the following trigger conditions are detected:
- Dockerfile
- docker-compose.yml
- kubernetes/*.yaml
- helm chart
- terraform/*.tf

---

## Core Controls

### INFRA-01: Container Image Security
**Control Requirement**: Container images must undergo security scanning and signature verification

- Use minimal base images (distroless/alpine)
- Integrate image scanning into CI/CD pipeline
- Prohibit use of `latest` tag
- Implement image signature verification (cosign/notary)

### INFRA-02: Container Runtime Security
**Control Requirement**: Container runtime must follow least privilege principle

- Prohibit privileged containers (`privileged: false`)
- Prohibit running as root (`runAsNonRoot: true`)
- Read-only root filesystem (`readOnlyRootFilesystem: true`)
- Drop all capabilities (`drop: ALL`)

### INFRA-03: Resource Limits
**Control Requirement**: All containers must have resource limits configured

- CPU limits (`resources.limits.cpu`)
- Memory limits (`resources.limits.memory`)
- Storage quota limits
- PID limits to prevent fork bombs

### INFRA-04: Network Segmentation
**Control Requirement**: Network must be segmented and isolated

- NetworkPolicy to restrict Pod-to-Pod communication
- Service mesh mTLS (Istio/Linkerd)
- Ingress/Egress traffic control
- Sensitive services not exposed to public network

### INFRA-05: Secrets Management
**Control Requirement**: Sensitive information must not be hardcoded in configuration

- Use external secret management (Vault/AWS Secrets Manager)
- Kubernetes Secrets encrypted storage
- Prohibit storing sensitive data in environment variables
- Automatic secret rotation mechanism

### INFRA-06: Infrastructure as Code Security
**Control Requirement**: IaC templates must undergo security scanning

- Static scanning (checkov/tfsec/terrascan)
- Sensitive data not committed to version control
- Changes require approval workflow
- Drift detection and remediation

---

## L4 References

Detailed practice guides in the `references/` directory:
- reference-set-12-docker-security.md
- reference-set-12-kubernetes-security.md
- reference-set-12-iac-security.md

---

## STRIDE Mapping

| STRIDE | Applicable Controls |
|--------|---------------------|
| S | INFRA-02, INFRA-05 |
| T | INFRA-01, INFRA-06 |
| I | INFRA-04, INFRA-05 |
| D | INFRA-03 |
| E | INFRA-02, INFRA-04 |
