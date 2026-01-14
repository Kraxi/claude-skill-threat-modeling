<!-- Code-First Deep Threat Modeling Workflow | Version 2.1.0 | https://github.com/fr33d3m0n/skill-threat-modeling | License: BSD-3-Clause | Welcome to cite but please retain all sources and declarations -->

# Control Set 16: Cloud Security (CLOUD)

**Domain**: CLOUD - Cloud Service Security
**Version**: 1.0
**Last Updated**: 2025-12-30

---

## Overview

Cloud service security control set, applicable to AWS/Azure/GCP/Alibaba Cloud and other cloud platforms. Automatically loaded when the following trigger conditions are detected:
- terraform/*.tf
- AWS SDK calls (boto3, aws-sdk)
- Azure SDK calls (azure-*)
- GCP SDK calls (google-cloud-*)
- serverless.yml

---

## Core Controls

### CLOUD-01: IAM Least Privilege
**Control Requirement**: IAM permissions must follow the least privilege principle

- Prohibit wildcard permissions (`*`)
- Regular permission review and cleanup
- Use roles instead of long-term credentials
- Condition policies to limit access scope

### CLOUD-02: Resource Access Policy
**Control Requirement**: Resource access policies must have explicit restrictions

- S3/Blob storage must prohibit public access
- Databases must not be exposed to public network
- API gateway authentication and authorization
- Resource tagging and classification

### CLOUD-03: Security Group Configuration
**Control Requirement**: Security groups must minimize exposure

- Prohibit 0.0.0.0/0 inbound rules (except necessary ports)
- Only open required ports
- Use security group references instead of IP addresses
- Regular security group rule auditing

### CLOUD-04: Logging and Monitoring
**Control Requirement**: Comprehensive logging and monitoring must be enabled

- CloudTrail/Activity Log enabled
- VPC Flow Logs enabled
- Security event alerting configured
- Centralized log storage and retention

### CLOUD-05: Encryption Configuration
**Control Requirement**: Data must be encrypted at rest and in transit

- Storage encryption (EBS/S3/RDS encryption)
- Transport encryption (TLS enforced)
- Key management (KMS/Key Vault)
- Customer-managed keys (CMK)

### CLOUD-06: Network Architecture
**Control Requirement**: Network architecture must be securely designed

- VPC/VNet isolation
- Applications deployed in private subnets
- NAT gateway for outbound traffic
- PrivateLink for service access

---

## L4 References

Detailed practice guides in the `references/` directory:
- reference-set-16-cloud-architecture.md
- reference-set-16-serverless-security.md

---

## STRIDE Mapping

| STRIDE | Applicable Controls |
|--------|---------------------|
| S | CLOUD-01 |
| T | CLOUD-02, CLOUD-05 |
| R | CLOUD-04 |
| I | CLOUD-02, CLOUD-03, CLOUD-05 |
| D | CLOUD-03, CLOUD-06 |
| E | CLOUD-01, CLOUD-02 |
