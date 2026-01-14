<!-- Code-First Deep Threat Modeling Workflow | Version 2.1.0 | https://github.com/fr33d3m0n/skill-threat-modeling | License: BSD-3-Clause | Welcome to cite but please retain all sources and declarations -->

# Control Set 13: Supply Chain Security (SUPPLY)

**Domain**: SUPPLY - Supply Chain Security
**Version**: 1.0
**Last Updated**: 2025-12-30

---

## Overview

Supply chain security control set, applicable to dependency management and build pipelines. Automatically loaded when the following trigger conditions are detected:
- package.json / package-lock.json
- requirements.txt / Pipfile
- pom.xml / build.gradle
- go.mod / go.sum
- Cargo.toml

---

## Core Controls

### SUPPLY-01: Dependency Vulnerability Scanning
**Control Requirement**: All dependencies must undergo vulnerability scanning

- Integrate into CI/CD pipeline (npm audit/pip-audit/snyk)
- Block builds with high-severity vulnerabilities
- Regular scanning of deployed applications
- Vulnerability fix SLA (Critical: 24h, High: 7d)

### SUPPLY-02: Dependency Pinning
**Control Requirement**: Dependency versions must be locked

- Use lock files (package-lock.json, Pipfile.lock)
- Prohibit range versions (`^`, `~`, `*`)
- Regular dependency review and updates
- Document dependency update reasons

### SUPPLY-03: SBOM Generation
**Control Requirement**: Software Bill of Materials must be generated and maintained

- Generate SBOM on each build (CycloneDX/SPDX)
- SBOM storage and version management
- Support vulnerability traceability queries
- Compliance report generation

### SUPPLY-04: Source Verification
**Control Requirement**: Dependency sources must be verifiable

- Use official/trusted repositories
- Verify package integrity (checksum/signature)
- Private repository access control
- Prohibit direct installation from Git of unpublished packages

### SUPPLY-05: CI/CD Pipeline Security
**Control Requirement**: Build pipelines must be securely configured

- Pipeline configuration under version control
- No hardcoded secrets in pipelines
- Least privilege execution
- Build artifact signing

### SUPPLY-06: License Compliance
**Control Requirement**: Dependency licenses must be compliant

- License scanning and review
- Prohibit incompatible licenses
- Maintain license whitelist
- Legal compliance documentation

---

## L4 References

Detailed practice guides in the `references/` directory:
- reference-set-13-dependency-management.md
- reference-set-13-supply-chain-security.md
- reference-set-13-npm-security.md
- reference-set-13-cicd-security.md

---

## STRIDE Mapping

| STRIDE | Applicable Controls |
|--------|---------------------|
| S | SUPPLY-04 |
| T | SUPPLY-01, SUPPLY-02, SUPPLY-05 |
| I | SUPPLY-03 |
| E | SUPPLY-05 |
