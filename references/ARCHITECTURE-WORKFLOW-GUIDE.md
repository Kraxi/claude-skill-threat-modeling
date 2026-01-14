<!-- Code-First Deep Threat Modeling Workflow | Version 2.1.1 | https://github.com/fr33d3m0n/skill-threat-modeling | License: BSD-3-Clause | Welcome to cite but please retain all sources and declarations -->

# STRIDE Skill Set Comprehensive Architecture and Workflow Guide

**version**: 2.1.0
**Date**: 2026-01-04
**Status**: Production - v2.1.0 Agentic Security Updated

---

## overview

thisdocumentdetaileddescription STRIDE threat modeling Skill Set ：
1. **dual four-level knowledge system** - two complementary knowledge setsarchitecture
2. **eight phase detailed workflow** - serialexecute threat modelingprocess
3. **between knowledge systemsassociatedand connection** - system A andsystem B collaboration relationship
4. **association and connection of knowledge with workflow** - each Phase howinvocationknowledge base

---

# Part 1：detailed explanation of dual four-level knowledge system

## 1.1 overall architecture view

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ STRIDE dualfour-levelknowledgearchitecturesystem │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ │
│ ╔═══════════════════════════════════════════════════════════════════════════════╗ │
│ ║ system A: SUKA v4.0 security controlhierarchy ║ │
│ ║ (Security Control Hierarchy) ║ │
│ ║ ║ │
│ ║ ┌─────────────────────────────────────────────────────────────────────────┐ ║ │
│ ║ │ L1: Security Principles (11individualsecurityprinciple) │ ║ │
│ ║ │ location: SKILL.md common paragraphs │ ║ │
│ ║ │ content: DID, LP, ZT, FS, SOD, SBD, CM, EOM, OD, IV, LA │ ║ │
│ ║ │ Purpose: guidanceallSecurity by Designdecision │ ║ │
│ ║ └─────────────────────────────────────────────────────────────────────────┘ ║ │
│ ║ │ ║ │
│ ║ ▼ guidance ║ │
│ ║ ┌─────────────────────────────────────────────────────────────────────────┐ ║ │
│ ║ │ L2: Security Design (16individualsecurity domain) │ ║ │
│ ║ │ location: assets/knowledge/security-design.yaml │ ║ │
│ ║ │ coredomain(10): AUTHN, AUTHZ, INPUT, OUTPUT, CLIENT, │ ║ │
│ ║ │ CRYPTO, LOG, ERROR, API, DATA │ ║ │
│ ║ │ Extension Domain(6): INFRA, SUPPLY, AI, MOBILE, CLOUD, AGENT │ ║ │
│ ║ │ Purpose: define inspection dimensions for security assessment │ ║ │
│ ║ └─────────────────────────────────────────────────────────────────────────┘ ║ │
│ ║ │ ║ │
│ ║ ▼ realize ║ │
│ ║ ┌─────────────────────────────────────────────────────────────────────────┐ ║ │
│ ║ │ L3: Security Controls (18individualcontrolcollectfile, 107individualcontrol) │ ║ │
│ ║ │ location: assets/knowledge/security-controls/control-set-*.md │ ║ │
│ ║ │ content: specific security control requirements and implementation guidance │ ║ │
│ ║ │ Purpose: Phase 4 securityfunctionassessment inspectInventory │ ║ │
│ ║ └─────────────────────────────────────────────────────────────────────────┘ ║ │
│ ║ │ ║ │
│ ║ ▼ practice ║ │
│ ║ ┌─────────────────────────────────────────────────────────────────────────┐ ║ │
│ ║ │ L4: Scenario Practices (73individualOWASPreferencefile) │ ║ │
│ ║ │ location: assets/knowledge/security-controls/references/reference-set-*.md │ ║ │
│ ║ │ content: OWASP Cheat Sheet detailedpracticeguide │ ║ │
│ ║ │ Purpose: Phase 7 mitigationrecommendtechnical reference │ ║ │
│ ║ └─────────────────────────────────────────────────────────────────────────┘ ║ │
│ ╚═══════════════════════════════════════════════════════════════════════════════╝ │
│ │
│ ╔═══════════════════════════════════════════════════════════════════════════════╗ │
│ ║ system B: Threat Intelligenceknowledgehierarchy ║ │
│ ║ (Threat Intelligence Hierarchy) ║ │
│ ║ ║ │
│ ║ ┌─────────────────────────────────────────────────────────────────────────┐ ║ │
│ ║ │ L1: STRIDE threat classificationmodel (STRIDE Threat Classification) │ ║ │
│ ║ │ location: assets/knowledge/stride-library.yaml │ ║ │
│ ║ │ content: 6largethreatCategory (S/T/R/I/D/E) + elementapplicability matrix │ ║ │
│ ║ │ Purpose: Phase 5 threatanalysis classificationbasic │ ║ │
│ ║ │ │ ║ │
│ ║ │ ┌─────────────────────────────────────────────────────────────┐ │ ║ │
│ ║ │ │ STRIDE threat intelligence chain item (Threat Intelligence Chain) │ │ ║ │
│ ║ │ │ ──────────────────────────────────────────────────────── │ │ ║ │
│ ║ │ │ STRIDE → CWE → CAPEC → ATT&CK → CVE/KEV │ │ ║ │
│ ║ │ │ L1 ├─────── L2 ────────┤ L3(validation) + L4(real-time) │ │ ║ │
│ ║ │ │ │ │ ║ │
│ ║ │ │ S(identityforgery) → CWE-287/290/307 → CAPEC-151/600 → T1078/T1110│ │ ║ │
│ ║ │ │ T(data tampering) → CWE-20/77/89 → CAPEC-66/88 → T1190 │ │ ║ │
│ ║ │ │ R(Repudiation) → CWE-117/223/778 → CAPEC-93 → T1070 │ │ ║ │
│ ║ │ │ I(information disclosure) → CWE-200/209/311 → CAPEC-116/157 → T1552/T1213│ │ ║ │
│ ║ │ │ D(rejectservice) → CWE-400/770/918 → CAPEC-125/227 → T1498/T1499│ │ ║ │
│ ║ │ │ E(permissionescalate) → CWE-269/284/862 → CAPEC-122/233 → T1068/T1548│ │ ║ │
│ ║ │ └─────────────────────────────────────────────────────────────┘ │ ║ │
│ ║ └─────────────────────────────────────────────────────────────────────────┘ ║ │
│ ║ │ ║ │
│ ║ ▼ L1→L2: threat classificationtoweakness enumeration ║ │
│ ║ ┌─────────────────────────────────────────────────────────────────────────┐ ║ │
│ ║ │ L2: Threat Intelligenceknowledgelayer (Threat Intelligence Layer) │ ║ │
│ ║ │ datasource: ATT&CK v18.1, CAPEC v3.9, CWE v4.19, OWASP Top 10 2025 │ ║ │
│ ║ │ record count: 835 technical + 615 pattern + 974 weakness + 248 CWEmapping │ ║ │
│ ║ │ storage: security_kb.sqlite │ ║ │
│ ║ │ Purpose: Phase 5 STRIDEanalysis threat enumeration │ ║ │
│ ║ │ │ ║ │
│ ║ │ L1-L2 relationship: STRIDECategory → CWEweaknesstype → CAPECattackpattern │ ║ │
│ ║ │ mappingtable: stride_cwe (STRIDE→CWE), capec_cwe (CAPEC→CWE) │ ║ │
│ ║ └─────────────────────────────────────────────────────────────────────────┘ ║ │
│ ║ │ ║ │
│ ║ ▼ validation ║ │
│ ║ ┌─────────────────────────────────────────────────────────────────────────┐ ║ │
│ ║ │ L3: securityvalidationknowledgelayer (Security Verification Layer) │ ║ │
│ ║ │ datasource: OWASP WSTG (121), MASTG (206), ASVS (345) │ ║ │
│ ║ │ storage: security_kb.sqlite (wstg_test, mastg_test, asvs_requirement) │ ║ │
│ ║ │ Purpose: Phase 6 riskvalidation testingmethod │ ║ │
│ ║ └─────────────────────────────────────────────────────────────────────────┘ ║ │
│ ║ │ ║ │
│ ║ ▼ compliance ║ │
│ ║ ┌─────────────────────────────────────────────────────────────────────────┐ ║ │
│ ║ │ L4: complianceframeworkknowledgelayer (Compliance Framework Layer) │ ║ │
│ ║ │ framework: NIST 800-53, CIS Controls, ISO 27001, CSA CCM v4 │ ║ │
│ ║ │ GDPR, EU AI Act, PCI-DSS, SOC 2, HIPAA │ ║ │
│ ║ │ storage: security_kb.sqlite + compliance-mappings.yaml │ ║ │
│ ║ │ Purpose: Phase 8 Compliance Report frameworkmapping │ ║ │
│ ║ └─────────────────────────────────────────────────────────────────────────┘ ║ │
│ ╚═══════════════════════════════════════════════════════════════════════════════╝ │
│ │
│ ╔═══════════════════════════════════════════════════════════════════════════════╗ │
│ ║ dataaccesslayer (Query Access Layers) ║ │
│ ║ unifiedqueryinterface: unified_kb_query.py ║ │
│ ║ ║ │
│ ║ ┌─────────────────────────────────────────────────────────────────────────┐ ║ │
│ ║ │ L1: YAML + Markdown (on-demandload, ~550KB) │ ║ │
│ ║ │ stride-library.yaml, llm-threats.yaml, cloud-services.yaml │ ║ │
│ ║ │ security-controls/*.md │ ║ │
│ ║ └─────────────────────────────────────────────────────────────────────────┘ ║ │
│ ║ ┌─────────────────────────────────────────────────────────────────────────┐ ║ │
│ ║ │ L2: SQLite Main (security_kb.sqlite, 18MB) │ ║ │
│ ║ │ CWE, CAPEC, ATT&CK, STRIDE, OWASP, WSTG, MASTG, ASVS, Compliance │ ║ │
│ ║ │ FTS5full textindex, semantic vector embedding (3,278 x 384-dim) │ ║ │
│ ║ └─────────────────────────────────────────────────────────────────────────┘ ║ │
│ ║ ┌─────────────────────────────────────────────────────────────────────────┐ ║ │
│ ║ │ L3: CVE Extension (security_kb_extension.sqlite, 304MB) │ ║ │
│ ║ │ 323,830+ CVErecord, CVE-CWEmapping, CVSSscore │ ║ │
│ ║ └─────────────────────────────────────────────────────────────────────────┘ ║ │
│ ║ ┌─────────────────────────────────────────────────────────────────────────┐ ║ │
│ ║ │ L4: Live API (real-timequery) │ ║ │
│ ║ │ NVD API: real-timeCVEdetaileddetails, CVSSscore │ ║ │
│ ║ │ KEV API: knownbyexploitvulnerabilityinspect │ ║ │
│ ║ └─────────────────────────────────────────────────────────────────────────┘ ║ │
│ ╚═══════════════════════════════════════════════════════════════════════════════╝ │
│ │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 1.2 system A detailed explanation：SUKA v4.0 security controlhierarchy

### L1: Security Principles (11individualsecurityprinciple)

| Numbering | Code | Principle Name | document | definition | Referenced Phases |------|------|----------|------|------|-----------| 1 | DID | Defense in Depth | Multi-layer independent security controls；Single point failure does not compromise system | 1, 2, 4 | 2 | LP | Least Privilege | Least Privilege | Grant only minimum permissions needed to complete task | 1, 3, 4 | 3 | ZT | Zero Trust | Never trust, always validate; assume network is compromised | 1, 2, 3, 4 | 4 | FS | Fail Securely | Secure Failure | Failure defaults to rejection；Do not expose sensitive information | 4 | 5 | SOD | Separation of Duties | Critical operations need multiple people/roles to complete | 3, 4 | 6 | SBD | Security by Design | Security by Design | Security built into design，Rather than after-the-fact patching | 4 | 7 | CM | Continuous Monitoring | Continuous Monitoring | Real-time detection、Alert and respond to security events | 4 | 8 | EOM | Economy of Mechanism | Mechanism Simplicity | Simpler security mechanisms are easier to verify and maintain | 4 | 9 | OD | Open Design | Open Design | Security not dependent on design confidentiality | 4 | 10 | IV | Input Validation | Input Validation | All external inputs require validation and sanitization | 2, 4 | 11 | LA | Least Agency | Minimum Agency | Limit AI Agent autonomous capabilities、tool access and decision scope | 1, 3, 4 |

**tierrelationship**: L1 principle → guidance L2 domaindesign → constraint L3 controlrealize

### L2: Security Design (16individualsecurity domain)

#### coredomain (01-10) and STRIDE mapping

```
┌─────────────────────────────────────────────────────────────────────────────┐
│ security domain and STRIDE mappingrelationship │
├─────────────────────────────────────────────────────────────────────────────┤
│ │
│ STRIDECategory mainassociateddomain securityproperty │
│ ───────────────────────────────────────────────────────────────────────── │
│ │
│ S (Spoofing) ──→ 01-AUTHN, 09-API, 05-CLIENT │
│ identityforgery └── authenticationmechanism、APIauthentication、clientidentity │
│ │
│ T (Tampering) ──→ 03-INPUT, 04-OUTPUT, 09-API │
│ data tampering └── Input Validation、outputencode、APIdataintegrityproperty │
│ │
│ R (Repudiation)──→ 07-LOG │
│ Repudiation └── logaudit、eventtracking │
│ │
│ I (Info Disc.) ──→ 06-CRYPTO, 08-ERROR, 10-DATA, 04-OUTPUT │
│ information disclosure └── encrypt、incorrectprocess、data protection、outputencode │
│ │
│ D (DoS) ──→ 09-API │
│ rejectservice └── rate limiting、resource quota │
│ │
│ E (Elevation) ──→ 02-AUTHZ │
│ permissionescalate └── authorizationaccesscontrol │
│ │
└─────────────────────────────────────────────────────────────────────────────┘
```

#### Extension Domain (ext-11 to ext-16) Trigger Condition

| Extension Domain | Code | Trigger Condition | Detection Pattern |--------|------|----------|----------| ext-11 | INFRA | Dockerfile, K8s manifests, IaC | `*.yaml`, `Dockerfile`, `*.tf` | ext-12 | SUPPLY | package.json, requirements.txt | Dependency declaration file | ext-13 | AI | openai/anthropic SDK, model files | AI library import, `.pt`, `.onnx` | ext-14 | MOBILE | iOS/Android project | `*.swift`, `*.kt`, `AndroidManifest.xml` | ext-15 | CLOUD | terraform, AWS/Azure/GCP SDK | Cloud provider SDK import | ext-16 | AGENT | Agent frameworks, MCP, Skills | `langchain`, `crewai`, `mcp.json`, Skills definition |

### L3: Security Controls (18individualcontrolcollect)

```
security-controls/
├── coredomaincontrolcollect (10individual)
│ ├── control-set-01-authentication.md # AUTHN: 6control
│ ├── control-set-02-authorization.md # AUTHZ: 6control
│ ├── control-set-03-input-validation.md # INPUT: 6control
│ ├── control-set-04-output-encoding.md # OUTPUT: 6control
│ ├── control-set-05-client-side.md # CLIENT: 6control
│ ├── control-set-06-cryptography.md # CRYPTO: 6control
│ ├── control-set-07-logging.md # LOG: 6control
│ ├── control-set-08-error-handling.md # ERROR: 5control
│ ├── control-set-09-api-security.md # API: 6control
│ └── control-set-10-data-protection.md # DATA: 6control
│
├── domainextensioncontrolcollect (2individual)
│ ├── control-set-ext-01_02-auth-patterns.md # authenticationauthorizationcrosspattern
│ └── control-set-ext-10-hardcoded-credentials.md # hardcoded credential
│
└── Extension Domaincontrolcollect (6individual)
 ├── control-set-ext-11-infrastructure.md # INFRA: 6control
 ├── control-set-ext-12-supply-chain.md # SUPPLY: 6control
 ├── control-set-ext-13-ai-llm.md # AI: 6control
 ├── control-set-ext-14-mobile.md # MOBILE: 6control
 ├── control-set-ext-15-cloud.md # CLOUD: 6control
 └── control-set-ext-16-agentic.md # AGENT: 10control
```

### L4: Scenario Practices (73individualOWASPreference)

**by domain distribution**:

| domain | reference count | representative files |----|--------|-----------| 01-AUTHN | 11 | authentication, mfa, session, password, jwt | 02-AUTHZ | 4 | authorization, access-control, idor | 03-INPUT | 13 | sql-injection, os-command-injection, ssrf | 04-OUTPUT | 0 | (new domain, to be supplemented) | 05-CLIENT | 13 | xss, csp, csrf, clickjacking | 06-CRYPTO | 6 | tls, key-management, cryptographic-storage | 07-LOG | 2 | logging, logging-vocabulary | 08-ERROR | 2 | error-handling, xs-leaks | 09-API | 7 | rest-security, graphql, microservices | 10-DATA | 3 | database-security, secrets-management | ext-11 INFRA | 4 | docker, kubernetes, iac | ext-12 SUPPLY | 4 | dependency-management, npm, cicd | ext-13 AI | 1 | ai-agent-security | ext-14 MOBILE | 2 | mobile-security | ext-15 CLOUD | 1 | cloud-architecture | ext-16 AGENT | 4 | agentic-security, mcp-security, multi-agent, skill-security |

---

## 1.3 system B detailed explanation：Threat Intelligenceknowledgehierarchy

### L1: STRIDE threat classificationmodel

STRIDE yesThreat Intelligencesystem basiclayer，provides six major Category framework for threat classification。

#### STRIDE Categorydefinition

| Category | English | document | definition | violated security property |------|------|------|------|----------------| **S** | Spoofing | identityforgery | disguised as otheruserorsystem | authentication (Authentication) | **T** | Tampering | data tampering | illegalmodifydataorcode | integrityproperty (Integrity) | **R** | Repudiation | Repudiation | norecognizeexecutepasscertainoperate | cannotnorecognizeproperty (Non-repudiation) | **I** | Information Disclosure | information disclosure | unauthorizedaccesssensitive information | Confidentiality (Confidentiality) | **D** | Denial of Service | rejectservice | usesystemorserviceunavailable | availableproperty (Availability) | **E** | Elevation of Privilege | permissionescalate | obtain beyondauthorization permission | authorization (Authorization) |

#### STRIDE per Element applicability matrix

| elementtype | S | T | R | I | D | E |----------|---|---|---|---|---|---| **Process** | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | **Data Store** | - | ✓ | ✓ | ✓ | ✓ | - | **Data Flow** | - | ✓ | - | ✓ | ✓ | - | **External Entity (as source)** | ✓ | - | ✓ | - | - | - |

#### L1→L2 threat intelligence chain item (Threat Intelligence Chain)

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│ STRIDE threat intelligence chain item │
├─────────────────────────────────────────────────────────────────────────────────┤
│ │
│ L1: STRIDE L2: Threat Intelligenceknowledge L3: validation L4: real-time │
│ ─────────── ───────────────────────── ───────── ───────── │
│ │
│ ┌─────────┐ ┌───────┐ ┌───────┐ ┌─────────┐ ┌───────┐ ┌─────────┐ │
│ │ STRIDE │──▶│ CWE │──▶│ CAPEC │──▶│ ATT&CK │──▶│ CVE │──▶│ KEV │ │
│ │ Category │ │ weakness │ │ attack │ │ technical │ │ vulnerability │ │ alreadyexploit │ │
│ └────┬────┘ └───┬───┘ └───┬───┘ └────┬────┘ └───┬───┘ └────┬────┘ │
│ │ │ │ │ │ │ │
│ │ │ │ │ │ │ │
│ classificationframework weakness enumeration attackpattern tactical techniques actualvulnerability active threat │
│ (6Category) (974item) (615item) (835item) (323K+) (real-timeAPI) │
│ │
│ mappingrelationship: │
│ ──────────────────────────────────────────────────────────────────────────── │
│ • stride_cwe: STRIDECategory → CWEweakness (eachCategorymapping10-50individualCWE) │
│ • capec_cwe: CWEweakness → CAPECattackpattern (how weaknesses are exploited) │
│ • capec_attack: CAPEC pattern → ATT&CK technique (attacker actual used techniques) │
│ • cve_cwe: CVEvulnerability → CWEweakness (weakness in actual vulnerabilitiesclassification) │
│ │
│ querychainexample (SQL Injection): │
│ ──────────────────────────────────────────────────────────────────────────── │
│ T(Tampering) → CWE-89 → CAPEC-66 → T1190 → CVE-2024-* → KEVinspect │
│ │
└─────────────────────────────────────────────────────────────────────────────────┘
```

#### each STRIDE Category threatchainmapping

| STRIDE | main CWE | main CAPEC | main ATT&CK | typical attack scenario |--------|----------|------------|-------------|--------------| **S** | CWE-287, 290, 307 | CAPEC-151, 194, 600 | T1078, T1110 | credential stuffing、identityforgery | **T** | CWE-20, 77, 78, 89 | CAPEC-66, 88, 248 | T1190, T1059 | SQLinjection、commandinjection | **R** | CWE-117, 223, 778 | CAPEC-93, 268 | T1070, T1562 | logforgery、auditbypass | **I** | CWE-200, 209, 311 | CAPEC-116, 157, 497 | T1552, T1213 | sensitive data leakage、configureexpose | **D** | CWE-400, 770, 918 | CAPEC-125, 227, 469 | T1498, T1499 | resource exhaustion、SSRF | **E** | CWE-269, 284, 862 | CAPEC-122, 233, 17 | T1068, T1548 | permissionbypass、vertical privilege escalation |

---

### L2: Threat Intelligenceknowledgelayer

#### datasourcedetaileddetails

| datasource | version | tablename | record count | primary fields |--------|------|------|--------|----------| MITRE ATT&CK | v18.1 | attack_technique | 835 | id, name, description, tactics, mitigations | CAPEC | v3.9 | capec | 615 | id, name, description, attack_steps, mitigations | CWE | v4.19 | cwe | 974 | id, name, description, mitigations, detection | OWASP Top 10 | 2025 | owasp_top10 | 10 | id, name, cwe_list, description |

#### knowledgemappingchain

```
┌─────────────────────────────────────────────────────────────────────────────┐
│ Threat Intelligenceknowledgemappingchain │
├─────────────────────────────────────────────────────────────────────────────┤
│ │
│ STRIDE Category │
│ │ │
│ ▼ │
│ ┌─────────────────────────────────────────────────────────────────────┐ │
│ │ OWASP Top 10 2025 │ │
│ │ A01: Broken Access Control → 26 CWEs → S, E │ │
│ │ A02: Cryptographic Failures → 18 CWEs → T, I │ │
│ │ A03: Injection → 30 CWEs → T │ │
│ │ A04: Insecure Design → 24 CWEs → T, E │ │
│ │ A05: Security Misconfiguration → 7 CWEs → T, E │ │
│ │ A06: Vulnerable Components → 13 CWEs → T, E │ │
│ │ A07: Auth Failures → 22 CWEs → S │ │
│ │ A08: Integrity Failures → 8 CWEs → T │ │
│ │ A09: Logging Failures → 5 CWEs → R │ │
│ │ A10: SSRF → 45 CWEs → I, D │ │
│ └─────────────────────────────────────────────────────────────────────┘ │
│ │ │
│ ▼ │
│ ┌─────────────────────────────────────────────────────────────────────┐ │
│ │ CWE (Common Weakness Enumeration) │ │
│ │ CWE-89: SQL Injection → CAPEC-66 → T1190 │ │
│ │ CWE-287: Improper Auth → CAPEC-151 → T1078 │ │
│ │ CWE-639: Auth Bypass → CAPEC-122 → T1087 │ │
│ │ CWE-79: XSS → CAPEC-86 → T1189 │ │
│ │ CWE-352: CSRF → CAPEC-62 → T1185 │ │
│ └─────────────────────────────────────────────────────────────────────┘ │
│ │ │
│ ▼ │
│ ┌─────────────────────────────────────────────────────────────────────┐ │
│ │ CAPEC (Attack Patterns) │ │
│ │ CAPEC-66: SQL Injection → T1190 (Exploit Public App) │ │
│ │ CAPEC-600: Credential Stuffing → T1110.004 │ │
│ │ CAPEC-122: Privilege Abuse → T1078 (Valid Accounts) │ │
│ └─────────────────────────────────────────────────────────────────────┘ │
│ │ │
│ ▼ │
│ ┌─────────────────────────────────────────────────────────────────────┐ │
│ │ ATT&CK (Adversary Tactics & Techniques) │ │
│ │ T1190: Exploit Public-Facing Application │ │
│ │ T1078: Valid Accounts │ │
│ │ T1110: Brute Force │ │
│ │ T1189: Drive-by Compromise │ │
│ └─────────────────────────────────────────────────────────────────────┘ │
│ │
└─────────────────────────────────────────────────────────────────────────────┘
```

### L3: securityvalidationknowledgelayer

| validation criteria | tablename | record count | purpose |----------|------|--------|------| OWASP WSTG | wstg_test | 121 | Webapplysecuritytestingguide | OWASP MASTG | mastg_test | 206 | moveapplysecuritytestingguide | OWASP ASVS | asvs_requirement | 345 | applysecurityvalidation criteria |

**validationknowledgestructure**:
```
wstg_test:
 - id: WSTG-INPV-05
 name: SQL Injection
 objective: "Test for SQL injection vulnerabilities"
 test_methods: ["Manual testing", "Automated scanning"]
 tools: ["sqlmap", "Burp Suite"]

asvs_requirement:
 - id: V1.1.1
 level: 1
 description: "Verify the use of a secure development lifecycle..."
 cwe: ["CWE-250", "CWE-749"]
```

### L4: complianceframeworkknowledgelayer

#### support complianceframework

| Category | framework | controlcount |------|------|--------| SDLC Security | NIST-SSDP, OWASP SAMM, AI-Governance | 50+ | App Security | OWASP ASVS, CWE Top 25, OWASP Top 10 | 300+ | Info Security | NIST CSF 2.0, NIST 800-53, CIS Controls | 800+ | Cloud Security | CSA CCM v4, CIS K8s, ISO 27017/18 | 400+ | Regional | GDPR, EU AI Act | 100+ |

#### compliancemappingstructure

```yaml
compliance_mapping:
 stride_Category: "Spoofing"
 security_domain: "01-AUTHN"
 frameworks:
 - framework: "NIST 800-53"
 controls: ["IA-1", "IA-2", "IA-5", "IA-8"]
 - framework: "ISO 27001"
 controls: ["A.9.2.1", "A.9.2.4", "A.9.4.2"]
 - framework: "PCI-DSS"
 controls: ["8.1", "8.2", "8.3"]
 - framework: "CIS Controls"
 controls: ["5.2", "5.3", "6.3"]
```

---

# Part 2：eight phase detailed workflow

## 2.1 workflowglobal view

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│ 8-Phase Deep Threat Modeling Workflow │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│ │
│ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐
│ │ Phase 1 │──▶│ Phase 2 │──▶│ Phase 3 │──▶│ Phase 4 │──▶│ Phase 5 │──▶│ Phase 6 │──▶│ Phase 7 │──▶│ Phase 8 │
│ │ Project │ │ DFD │ │ Trust │ │Security │ │ STRIDE │ │ Risk │ │Mitigate │ │ Report │
│ │Understanding│ Analysis│ │Boundary │ │ Design │ │Analysis │ │Validate │ │ Plan │ │Generate │
│ └─────────┘ └─────────┘ └─────────┘ └─────────┘ └─────────┘ └─────────┘ └─────────┘ └─────────┘
│ │ │ │ │ │ │ │ │
│ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼
│ project_ctx dfd_elements boundary_ctx security_gaps threat_inv valid_threats mitig_plan report_data
│ │
│ ═══════════════════════════════════════════════════════════════════════════════════════════ │
│ │
│ executepattern: strictserial (Phase N completebefore canonstart Phase N+1) │
│ in-depth thinking: eachPhaseuse <ultrathink><critical thinking> pattern │
│ knowledgequery: Phase 5/6/7 must query knowledge base for each risk │
│ parallelsub-agent: Phase 5/6/7 internally can launch parallel sub-agents for multiple risks │
│ │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

## 2.2 each Phase detaileddescription

### Phase 1: Project Understanding (Project Understanding)

| dimension | description |------|------| **goal** | Comprehensive project understanding: architecture, Functional modules, tech stack, and security design | **executor** | Claude + script-assisted | **Script Support** | `list_files.py --categorize --detect-type` | **knowledge base** | None needed | **input** | Project code path | **output** | `project_context` (Project type, Module inventory, integrations, Security design) |

**critical activity**:
1. obtain file structure and categorization
2. identificationProject type (Web/API/microservice/AI/LLM)
3. readcriticalfile (Entrypoint、configure、APIdefinition)
4. document architecture understanding
5. record preliminary security observations

### Phase 2: invocationflow/DFD analysis (Call Flow & DFD)

| dimension | description |------|------| **goal** | builddata flow diagram，trace how data flows through system | **executor** | Claude native capability | **Script Support** | none | **knowledge base** | None needed | **input** | `project_context` from Phase 1 | **output** | `dfd_elements` (elementInventory、data flow、DFD Diagram) |

**critical activity**:
1. identify external interactors (user、externalservice)
2. tracking data Entry points
3. mappingprocessprocess
4. identificationdatastorage
5. use Mermaid draw DFD

**DFD elementtype**:
- **EI**: External Interactor (External interactors)
- **P**: Process (process)
- **DS**: Data Store (datastorage)
- **DF**: Data Flow (data flow)

### Phase 3: trust boundaryassessment (Trust Boundary Evaluation)

| dimension | description |------|------| **goal** | identify critical interfaces, boundaries, data nodes and security posture | **executor** | Claude native capability | **Script Support** | none | **knowledge base** | None needed | **input** | `dfd_elements` from Phase 2 | **output** | `boundary_context` (boundary、interface、cross-boundary flow) |

**critical activity**:
1. Identify network boundary (DMZ, internal network, data layer)
2. identificationprocessboundary (container、VM、Serverless)
3. definitionusertrustlevel
4. mark cross-boundary data flow
5. analyze security controls at each boundary

### Phase 4: Security by Designassessment (Security Design Assessment)

| dimension | description |------|------| **goal** | in-depth analysis of design implementation across all security domains | **executor** | Claude + knowledge base | **Script Support** | `--control {domain}` (on-demand) | **knowledge base** | system A: L2 domaindefinition + L3 controlcollect | **input** | Phase 1-3 alloutput | **output** | `security_gaps` (gap、designmatrix) |

**mustcoverage 9 individualsecurity domain**:
1. identitymanage (Identity Management)
2. authentication (Authentication)
3. authorization/accesscontrol (Authorization)
4. encryptandkeymanage (Encryption)
5. logandaudit (Logging)
6. sensitive data protection (Data Protection)
7. highavailableproperty (High Availability)
8. Input Validation (Input Validation)
9. sessionmanage (Session Management)

### Phase 5: STRIDE analysis (STRIDE Analysis)

| dimension | description |------|------| **goal** | Use STRIDE + CWE + ATT&CK + LLM threats for comprehensive threat analysis | **executor** | script + Claude | **Script Support** | `stride_matrix.py`, `unified_kb_query.py --full-chain` | **knowledge base** | System B: L2 Threat Intelligence (CWE/CAPEC/ATT&CK) | **input** | Phase 2-4 output | **output** | `threat_inventory` (Threat inventory，Including ID/CWE/CAPEC) |

**STRIDE per Interaction matrix**:
```
 │ S │ T │ R │ I │ D │ E │
──────────┼───┼───┼───┼───┼───┼───┤
Process │ ✓ │ ✓ │ ✓ │ ✓ │ ✓ │ ✓ │
Data Store│ │ ✓ │ ✓ │ ✓ │ ✓ │ │
Data Flow │ │ ✓ │ │ ✓ │ ✓ │ │
+External │ ✓ │ │ ✓ │ │ │ │
```

**threat ID format**: `T-{STRIDE}-{ElementID}-{Seq}`
- example: `T-S-P1-001` (identityforgerythreat，for processesP1，Part1individual)

### Phase 6: riskvalidation (Risk Validation)

| dimension | description |------|------| **goal** | in-depth analysis of Attack Path for each risk、POCdesignandValidation Method | **executor** | script + Claude | **Script Support** | `--capec`, `--attack-technique`, `--check-kev`, `--cve-for-cwe` | **knowledge base** | System B: L2 Threat Intelligence + L3 validationknowledge (WSTG/ASVS) | **input** | `threat_inventory` from Phase 5 | **output** | `validated_threats` (including Attack Path、POCmethod) |

**eachrisk validationcontent**:
1. query CAPEC attackpattern
2. query ATT&CK technical
3. inspectknownbyexploitvulnerability (KEV)
4. buildAttack Path
5. design POC Validation Method

### Phase 7: mitigationrecommendgenerate (Mitigation Generation)

| dimension | description |------|------| **goal** | generate knowledge base enhanced technical specific Mitigation Measures for each risk | **executor** | script + Claude | **Script Support** | `--cwe {id} --mitigations`, `--stride {Category}` | **knowledge base** | system A: L3 controlcollect + L4 reference + system B: CWEmitigation | **input** | Phase 5 + Phase 6 output | **output** | `mitigation_plan` (Mitigation Measures, code examples, path diagrams) |

**eachrisk mitigationcontent**:
1. query CWE mitigationrecommend
2. query STRIDE controlmapping
3. design tech-stack-specific implementation
4. providecodeexample
5. estimate workload andprioritylevel

### Phase 8: comprehensive report generation (Comprehensive Report)

| dimension | description |------|------| **goal** | generateintegritythreatmodelreport，comprehensiveallPhaseoutput | **executor** | Claude | **Script Support** | none | **knowledge base** | system B: L4 complianceframework (on-demand) | **input** | Phase 1-7 alloutput | **output** | `{PROJECT}-RISK-ASSESSMENT-REPORT.md` |

**reportstructure**:
1. executesummary
2. systemarchitectureoverview
3. DFD andtrust boundary
4. Security by Designassessment
5. Threat inventory (by STRIDE classification)
6. riskanalysis (including Attack Path)
7. Mitigation Measures (including code example)
8. implementation path diagram
9. compliancemapping (on-demand)

---

# Part 3：between knowledge systemsassociatedand connection

## 3.1 system A andsystem B collaboration relationship

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ system A and system B collaboration relationship diagram │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ │
│ system A system B │
│ (security controlhierarchy) (Threat Intelligencehierarchy) │
│ │
│ ┌─────────────────────┐ ┌─────────────────────┐ │
│ │ L1: Security │ │ │ │
│ │ Principles │ │ │ │
│ │ (11principle) │ │ │ │
│ └──────────┬──────────┘ │ │ │
│ │ │ │ │
│ ▼ │ │ │
│ ┌─────────────────────┐ │ L2: Threat │ │
│ │ L2: Security │◀─────────────────▶│ Intelligence │ │
│ │ Design │ STRIDEmapping │ (ATT&CK/CAPEC/CWE)│ │
│ │ (16domain) │ │ │ │
│ └──────────┬──────────┘ └──────────┬──────────┘ │
│ │ │ │
│ ▼ ▼ │
│ ┌─────────────────────┐ ┌─────────────────────┐ │
│ │ L3: Security │ │ L3: Security │ │
│ │ Controls │◀─────────────────▶│ Verification │ │
│ │ (18controlcollect) │ CWE→Control │ (WSTG/MASTG/ASVS) │ │
│ └──────────┬──────────┘ └──────────┬──────────┘ │
│ │ │ │
│ ▼ ▼ │
│ ┌─────────────────────┐ ┌─────────────────────┐ │
│ │ L4: Scenario │ │ L4: Compliance │ │
│ │ Practices │◀─────────────────▶│ Frameworks │ │
│ │ (73reference) │ Control→framework │ (NIST/CIS/ISO) │ │
│ └─────────────────────┘ └─────────────────────┘ │
│ │
│ ════════════════════════════════════════════════════════════════════════════════ │
│ │
│ collaboration relationship: │
│ ───────────────────────────────────────────────────────────────────────────────── │
│ │
│ 1. STRIDE → Domain mapping: │
│ systemB STRIDECategorymappingtosystemA security domain │
│ example: S(Spoofing) → 01-AUTHN │
│ │
│ 2. CWE → Control mapping: │
│ systemB CWEweaknessmappingtosystemA security control │
│ example: CWE-89 → control-set-03-input-validation.md │
│ │
│ 3. Control → Verification mapping: │
│ systemA controlpasssystemB validation test and confirm │
│ example: authenticationcontrol → WSTG-ATHN-* testing │
│ │
│ 4. Control → Compliance mapping: │
│ systemA controlmappingtosystemB complianceframework │
│ example: MFAcontrol → NIST IA-2, PCI-DSS 8.3 │
│ │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## 3.2 knowledgemappingchainexample

### example 1: authenticationthreat integrityknowledgechain

```
threat: credential stuffingattack

system B path:
─────────────────────────────────────────────────────────────────────────
STRIDE: S (Spoofing)
 │
 ▼
OWASP: A07 (Authentication Failures)
 │
 ▼
CWE: CWE-307 (Improper Restriction of Excessive Authentication Attempts)
 │
 ▼
CAPEC: CAPEC-600 (Credential Stuffing)
 │
 ▼
ATT&CK: T1110.004 (Credential Stuffing)
 │
 ▼
validation: WSTG-ATHN-03 (Testing for Weak Lock Out Mechanism)
 │
 ▼
compliance: NIST IA-5, PCI-DSS 8.1.6

system A path:
─────────────────────────────────────────────────────────────────────────
STRIDE: S (Spoofing)
 │
 ▼
Domain: 01-AUTHN (authenticationandsession)
 │
 ▼
Control: control-set-01-authentication.md
 │
 ├── control item: accountlockedmechanism
 ├── control item: rate limiting
 └── control item: MFArealize
 │
 ▼
Reference: reference-set-01-authentication.md
 │
 ├── specificrealize: express-rate-limit
 └── codeexample: Redisaccountlocked
```

### example 2: SQLinjectionthreat integrityknowledgechain

```
threat: SQLinjectionattack

system B path:
─────────────────────────────────────────────────────────────────────────
STRIDE: T (Tampering)
 │
 ▼
OWASP: A03 (Injection)
 │
 ▼
CWE: CWE-89 (SQL Injection)
 │
 ▼
CAPEC: CAPEC-66 (SQL Injection)
 │
 ▼
ATT&CK: T1190 (Exploit Public-Facing Application)
 │
 ▼
validation: WSTG-INPV-05 (Testing for SQL Injection)
 │
 ▼
compliance: NIST SI-10, PCI-DSS 6.5.1

system A path:
─────────────────────────────────────────────────────────────────────────
STRIDE: T (Tampering)
 │
 ▼
Domain: 03-INPUT (Input Validation)
 │
 ▼
Control: control-set-03-input-validation.md
 │
 ├── control item: parameterizationquery
 ├── control item: input whitelist validation
 └── control item: ORMuse
 │
 ▼
Reference: reference-set-03-sql-injection-prevention.md
 │
 ├── specificrealize: SQLAlchemy ORM
 └── Code example: Pydantic validator
```

---

# Part 4：association and connection of knowledge with workflow

## 4.1 Phase andknowledge system mapping summary table

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ Phase → knowledge system mappingrelationshiptable │
├────────┬──────────────────┬──────────────────┬──────────────────────────────────────┤
│ Phase │ system A tier │ system B tier │ querycommandexample │
├────────┼──────────────────┼──────────────────┼──────────────────────────────────────┤
│ P1 │ - │ - │ list_files.py --detect-type │
│ │ (Claude native) │ (Claude native) │ │
├────────┼──────────────────┼──────────────────┼──────────────────────────────────────┤
│ P2 │ - │ - │ (nonescript) │
│ │ (Claude native) │ (Claude native) │ │
├────────┼──────────────────┼──────────────────┼──────────────────────────────────────┤
│ P3 │ - │ - │ (nonescript) │
│ │ (Claude native) │ (Claude native) │ │
├────────┼──────────────────┼──────────────────┼──────────────────────────────────────┤
│ P4 │ L1: principle │ - │ --control {domain} │
│ │ L2: domaindefinition │ │ --stride-controls {Category} │
│ │ L3: controlcollect │ │ │
├────────┼──────────────────┼──────────────────┼──────────────────────────────────────┤
│ P5 │ L2: STRIDEmapping │ L2: CWE/CAPEC │ --stride {Category} │
│ │ │ ATT&CK │ --full-chain CWE-XXX │
│ │ │ OWASP Top 10 │ --all-llm (AIcomponent) │
├────────┼──────────────────┼──────────────────┼──────────────────────────────────────┤
│ P6 │ - │ L2: CAPEC │ --capec CAPEC-XXX --attack-chain │
│ │ │ ATT&CK │ --attack-technique TXXX │
│ │ │ L3: WSTG/ASVS │ --check-kev CVE-XXXX │
│ │ │ CVE │ --cve-for-cwe CWE-XXX │
├────────┼──────────────────┼──────────────────┼──────────────────────────────────────┤
│ P7 │ L3: controlcollect │ L2: CWEmitigation │ --cwe CWE-XXX --mitigations │
│ │ L4: OWASPreference │ │ --control {domain} --implementation │
├────────┼──────────────────┼──────────────────┼──────────────────────────────────────┤
│ P8 │ - │ L4: complianceframework │ --compliance {framework} │
│ │ (Claudecomprehensive) │ │ │
└────────┴──────────────────┴──────────────────┴──────────────────────────────────────┘
```

## 4.2 Phase contextpassprotocol

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ Phase contextpassprocessdiagram │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ │
│ Phase 1 Phase 2 Phase 3 │
│ ┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐ │
│ │ project_context │──────▶│ dfd_elements │───────▶│ boundary_context │ │
│ │ │ │ │ │ │ │
│ │ - project_type │ │ - elements[] │ │ - boundaries[] │ │
│ │ - modules[] │ │ {id,type,name} │ │ - interfaces[] │ │
│ │ - integrations[] │ │ - flows[] │ │ - data_nodes[] │ │
│ │ - security_design│ │ {src,tgt,data} │ │ - cross_flows[] │ │
│ └──────────────────┘ │ - dfd_diagram │ └────────┬─────────┘ │
│ └──────────────────┘ │ │
│ │ │
│ Phase 4 Phase 5 Phase 6│ │
│ ┌──────────────────┐ ┌──────────────────┐ ┌───────▼──────────┐ │
│ │ security_gaps │──────▶│ threat_inventory │───────▶│ validated_threats│ │
│ │ │ │ │ │ │ │
│ │ - gaps[] │ │ - threats[] │ │ - threats[] │ │
│ │ {domain,sev, │ │ {id,element_id,│ │ {id,attack_path│ │
│ │ description} │ │ stride,cwe, │ │ capec,attck, │ │
│ │ - design_matrix{}│ │ priority} │ │ poc_method} │ │
│ └──────────────────┘ └──────────────────┘ └────────┬─────────┘ │
│ ▲ ▲ │ │
│ │ │ │ │
│ ┌──────┴───────────────────────────┴─────────────────┐ │ │
│ │ quote Phase 1-3 output │ │ │
│ └────────────────────────────────────────────────────┘ │ │
│ │ │
│ Phase 7 Phase 8 │ │
│ ┌──────────────────┐ ┌──────────────────┐ │ │
│ │ mitigation_plan │──────▶│ report_data │◀────────────────┘ │
│ │ │ │ │ │
│ │ - mitigations[] │ │ - all_phases │ │
│ │ {threat_id,cwe,│ │ - summary │ │
│ │ measures,impl,│ │ - roadmap │ │
│ │ effort} │ │ - compliance │ │
│ │ - roadmap{} │ └──────────────────┘ │
│ └──────────────────┘ │
│ │
│ ════════════════════════════════════════════════════════════════════════════════ │
│ │
│ cross-phase citation rule: │
│ ─────────────────────────────────────────────────────────────────────────────── │
│ • threatIDformat: T-{STRIDE}-{ElementID}-{Seq} │
│ • ElementID from Phase 2 dfd_elements │
│ • Phase 5-7 each threat analysis must reference its Element ID │
│ • Phase 6/7 mustquote Phase 5 threat ID │
│ │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## 4.3 parallelsub-agentpattern (Phase 5/6/7)

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ parallelsub-agentexecutepattern (Phase 5/6/7) │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ │
│ Phase 5: STRIDE analysis (manythreatparallel) │
│ ────────────────────────────────────────────────────────────────────────────────── │
│ │
│ Main Agent │
│ │ │
│ ├──▶ DFD Element P1 ──▶ Sub-Agent ──▶ KB: --full-chain CWE-89 │
│ │ ──▶ KB: --stride tampering │
│ │ ◀── threatanalysisresult │
│ │ │
│ ├──▶ DFD Element P2 ──▶ Sub-Agent ──▶ KB: --full-chain CWE-287 │
│ │ ──▶ KB: --stride spoofing │
│ │ ◀── threatanalysisresult │
│ │ │
│ ├──▶ DFD Element DS1 ──▶ Sub-Agent ──▶ KB: --full-chain CWE-359 │
│ │ ──▶ KB: --stride info_disclosure │
│ │ ◀── threatanalysisresult │
│ │ │
│ └──▶ AI Component ──▶ Sub-Agent ──▶ KB: --all-llm │
│ ──▶ KB: --llm LLM01 │
│ ◀── LLMthreatanalysisresult │
│ │ │
│ ◀──────────────────── aggregateallthreat ───────────────────── │
│ │
│ │
│ Phase 6: riskvalidation (manyriskparallel) │
│ ────────────────────────────────────────────────────────────────────────────────── │
│ │
│ Main Agent │
│ │ │
│ ├──▶ T-S-P1-001 ──▶ Sub-Agent ──▶ KB: --capec CAPEC-600 --attack-chain │
│ │ ──▶ KB: --attack-technique T1110.004 │
│ │ ◀── Attack Path + POCmethod │
│ │ │
│ ├──▶ T-T-DF1-001 ──▶ Sub-Agent ──▶ KB: --capec CAPEC-66 --attack-chain │
│ │ ──▶ KB: --cve-for-cwe CWE-89 │
│ │ ──▶ KB: --check-kev │
│ │ ◀── Attack Path + KEVStatus │
│ │ │
│ └──▶ T-E-P3-001 ──▶ Sub-Agent ──▶ KB: --capec CAPEC-122 --attack-chain │
│ ◀── Attack Path + POCmethod │
│ │ │
│ ◀──────────────────── aggregatevalidationresult ───────────────────── │
│ │
│ │
│ Phase 7: mitigationrecommend (manyriskparallel) │
│ ────────────────────────────────────────────────────────────────────────────────── │
│ │
│ Main Agent │
│ │ │
│ ├──▶ T-S-P1-001 ──▶ Sub-Agent ──▶ KB: --cwe CWE-307 --mitigations │
│ │ ──▶ KB: --control authentication │
│ │ ◀── Mitigation Measures + codeexample │
│ │ │
│ ├──▶ T-T-DF1-001 ──▶ Sub-Agent ──▶ KB: --cwe CWE-89 --mitigations │
│ │ ──▶ KB: --control input-validation │
│ │ ◀── Mitigation Measures + codeexample │
│ │ │
│ └──▶ T-E-P3-001 ──▶ Sub-Agent ──▶ KB: --cwe CWE-639 --mitigations │
│ ──▶ KB: --control authorization │
│ ◀── Mitigation Measures + codeexample │
│ │ │
│ ◀──────────────────── aggregatemitigationplan ───────────────────── │
│ │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## 4.4 knowledge basequerydecision matrix

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ knowledge basequerydecision matrix │
├─────────┬───────────────────────────────┬───────────────────────────────────────────┤
│ Phase │ queryprerequisite │ querycommand │
├─────────┼───────────────────────────────┼───────────────────────────────────────────┤
│ P4 │ assessmentspecificsecurity domain │ --control {domain} │
│ │ obtainSTRIDEcontrolmapping │ --stride-controls {Category} │
│ │ view all controls overview │ --all-controls │
├─────────┼───────────────────────────────┼───────────────────────────────────────────┤
│ P5 │ eachDFDelement │ --element {type} (obtain applicableSTRIDE) │
│ │ eachSTRIDECategory │ --stride {Category} │
│ │ eachidentification CWE │ --full-chain CWE-XXX │
│ │ detectiontoAI/LLMcomponent │ --all-llm or --llm LLM01 │
│ │ detectiontocloudservice │ --cloud {provider} │
│ │ semanticsearchrelatedthreat │ --semantic-search "query" │
├─────────┼───────────────────────────────┼───────────────────────────────────────────┤
│ P6 │ eachthreat CAPEC │ --capec CAPEC-XXX --attack-chain │
│ │ eachthreat ATT&CK │ --attack-technique TXXX │
│ │ inspectknownbyexploitvulnerability │ --check-kev CVE-XXXX │
│ │ search CVE by CWE │ --cve-for-cwe CWE-XXX │
│ │ queryCVEseverityproperty │ --cve CVE-XXXX --severity │
├─────────┼───────────────────────────────┼───────────────────────────────────────────┤
│ P7 │ eachCWE Mitigation Measures │ --cwe CWE-XXX --mitigations │
│ │ obtainSTRIDEcontrolmapping │ --stride {Category} │
│ │ obtaincontrolrealizeguide │ --control {domain} --implementation │
│ │ CVEcontextreference │ --cve-for-cwe CWE-XXX --cve-severity HIGH │
├─────────┼───────────────────────────────┼───────────────────────────────────────────┤
│ P8 │ complianceframeworkmapping(on-demand) │ --compliance {framework} │
│ │ allcomplianceoverview │ --all-compliance │
└─────────┴───────────────────────────────┴───────────────────────────────────────────┘
```

---

# Part 5：summary

## 5.1 architecturedesign philosophy

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│ STRIDE Skill Set design philosophy │
├─────────────────────────────────────────────────────────────────────────────────────┤
│ │
│ 1. "Script is black box" principle │
│ ──────────────────────────────────────────────────────────────────────────── │
│ • Script execution does not consume context，Only output consumption │
│ • complexcalculate（such asknowledge basequery）usescriptprocess │
│ • Claude focus on tasks requiring understanding and reasoning │
│ │
│ 2. "offseparation of concerns" principle │
│ ──────────────────────────────────────────────────────────────────────────── │
│ • system A (security control): definition"what to do"and"how to do" │
│ • system B (Threat Intelligence): definition"know what"and"validationwhat" │
│ • two systems each perform their duties, collaboration through mapping relationships │
│ │
│ 3. "progressive disclosure" principle │
│ ──────────────────────────────────────────────────────────────────────────── │
│ • Phase 1-3: pure Claude capability，noneneedknowledge base │
│ • Phase 4: on-demandloadsecurity domaincontrol │
│ • Phase 5-7: each risk independently queries knowledge base │
│ • Phase 8: on-demandloadcomplianceframework │
│ │
│ 4. "strictserial + parallelsub-agent" executepattern │
│ ──────────────────────────────────────────────────────────────────────────── │
│ • Phase strict betweenserial: ensurecontextpassintegrity │
│ • Phase internalparallelsub-agent: manyriskanalysiscanparallelexecute │
│ • maximize efficiency while ensuring quality │
│ │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## 5.2 criticaldatastatistics

| Category | statistics |------|------| **system A** | L1 securityprinciple | 11 | L2 security domain | 16 (10core + 6extension) | L3 controlcollectfile | 18 | L3 controlEntry | 107 | L4 OWASPreference | 74 | **system B** | ATT&CK technical | 835 | CAPEC attackpattern | 615 | CWE weakness | 974 | OWASP Top 10 CWEmapping | 248 | WSTG testing | 121 | MASTG testing | 206 | ASVS require | 345 | STRIDEvalidationmapping | 1,269 | CVE record | 323,830+ | complianceframework | 14+ | **dataaccesslayer** | SQLite Main | 18 MB | SQLite Extension | 304 MB | YAML configure | 550 KB | semanticvector | 3,278 x 384-dim |

## 5.3 documentversioninformation

| project | value |------|-----| documentversion | 2.1.0 | generateDate | 2026-01-04 | analysismethod | Ultrathink in-depth analysis | knowledge baseversion | SUKA v4.0 + Agentic Security | SQLiteversion | CWE v4.19, CAPEC v3.9, ATT&CK v18.1 |

---

*thisdocumentby Code-First Deep Risk Analysis Skill generate*
