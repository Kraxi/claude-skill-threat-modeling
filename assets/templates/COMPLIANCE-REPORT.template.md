<!-- Code-First Deep Threat Modeling Workflow | Version 2.1.0 | https://github.com/fr33d3m0n/skill-threat-modeling | License: BSD-3-Clause | Welcome to cite but please retain all sources and declarations -->

# Compliance Report: {PROJECT_NAME}

> **Assessment Time**: {ASSESSMENT_DATETIME}
> **AnalysisAnalyst**: Claude (Deep Risk Analysis)
> **FrameworkVersion**: STRIDE-TM v1.0.2
> **ReportVersion**: {REPORT_VERSION}

---

## 1. ComplianceOverview

### 1.1 ApplicableUseCompliance Framework

| Framework | Version | ApplicableUseReason | Assessment Scope |
|------|------|---------|---------|
| OWASP Top 10 | 2021 | Web ApplicationSecurityStandard | All |
| OWASP LLM Top 10 | 2023 | AI/LLM Security (Such asApplicableUse) | {LLM_SCOPE} |
| CWE/SANS Top 25 | 2023 | Risk SoftwareVulnerability | All |
| NIST CSF | 2.0 | NetworkSecurityFramework | {NIST_SCOPE} |
| ISO 27001 | 2022 | InformationSecurityManagement | {ISO_SCOPE} |
| PCI-DSS | 4.0 | Payment cardDataSecurity | {PCI_SCOPE} |

### 1.2 ComplianceGapSummary

| Framework | RelatedControl | Compliant | Partially Compliant | Non-compliant | CompliantRate |
|------|---------|------|---------|--------|-------|
| OWASP Top 10 | {OWASP_TOTAL} | {OWASP_PASS} | {OWASP_PARTIAL} | {OWASP_FAIL} | {OWASP_RATE}% |
| CWE Top 25 | {CWE_TOTAL} | {CWE_PASS} | {CWE_PARTIAL} | {CWE_FAIL} | {CWE_RATE}% |
| NIST CSF | {NIST_TOTAL} | {NIST_PASS} | {NIST_PARTIAL} | {NIST_FAIL} | {NIST_RATE}% |
| ISO 27001 | {ISO_TOTAL} | {ISO_PASS} | {ISO_PARTIAL} | {ISO_FAIL} | {ISO_RATE}% |

### 1.3 ComplianceStatusOverview

```
┌─────────────────────────────────────────────────────────────────────────────┐
│ Compliance Status Overview │
├─────────────────────────────────────────────────────────────────────────────┤
│ │
│ OWASP Top 10: │
│ ████████████████████████░░░░░░░░░░░░░░░░ {OWASP_RATE}% Compliant │
│ │
│ CWE Top 25: │
│ ██████████████████████████████░░░░░░░░░░ {CWE_RATE}% Compliant │
│ │
│ NIST CSF: │
│ ████████████████████████████████████░░░░ {NIST_RATE}% Compliant │
│ │
│ ISO 27001: │
│ ██████████████████████████████████████░░ {ISO_RATE}% Compliant │
│ │
│ DiagramExample: ████ Compliant ░░░░ Gap │
│ │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. OWASP Top 10 (2021) Mapping

### 2.1 MappingOverview

| # | OWASP | Name | RelatedThreat | Status | GapDescription |
|---|-------|------|---------|------|---------|
| A01 | Broken Access Control | InvalidAccessControl | {A01_THREATS} | {A01_STATUS} | {A01_GAP} |
| A02 | Cryptographic Failures | EncryptionMechanismInvalid | {A02_THREATS} | {A02_STATUS} | {A02_GAP} |
| A03 | Injection | Injection | {A03_THREATS} | {A03_STATUS} | {A03_GAP} |
| A04 | Insecure Design | NotSecurityDesign | {A04_THREATS} | {A04_STATUS} | {A04_GAP} |
| A05 | Security Misconfiguration | SecurityConfigurationError | {A05_THREATS} | {A05_STATUS} | {A05_GAP} |
| A06 | Vulnerable Components | Vulnerable toAttackComponent | {A06_THREATS} | {A06_STATUS} | {A06_GAP} |
| A07 | Auth Failures | IdentityAuthenticationInvalid | {A07_THREATS} | {A07_STATUS} | {A07_GAP} |
| A08 | Data Integrity Failures | Software and DataCompleteCapabilityInvalid | {A08_THREATS} | {A08_STATUS} | {A08_GAP} |
| A09 | Logging Failures | SecurityLog and MonitoringInvalid | {A09_THREATS} | {A09_STATUS} | {A09_GAP} |
| A10 | SSRF | ServiceToolEndRequestForgery | {A10_THREATS} | {A10_STATUS} | {A10_GAP} |

**StatusDescription**: ✅ Compliant | ⚠️ Partially Compliant | ❌ Non-compliant | ➖ Not Applicable

### 2.2 DetailedAnalysis

{OWASP_DETAILED_ANALYSIS}
<!--
Format:

#### A01: Broken Access Control (InvalidAccessControl)

**RelatedThreat**:
| Threat ID | Risk Name | Severity |
|--------|---------|---------|
| T-E-P01-001 | Horizontal privilege escalationAccess | 🔴 Critical |
| T-E-P02-002 | Vertical privilege escalationAccess | 🟠 High |

** when BeforeStatus**: ⚠️ Partially Compliant

**FindingIssue**:
1. {ISSUE_1}
2. {ISSUE_2}

**RecommendationMeasure**:
1. {MEASURE_1}
2. {MEASURE_2}

**ReferenceMitigation**: M-016 (RBAC Enhancement)

---
-->

---

## 3. OWASP LLM Top 10 Mapping

<!-- Only when ProjectInclude AI/LLM Componentgenerate this section when -->

{LLM_SECTION_CONDITION}

### 3.1 MappingOverview

| # | LLM | Name | RelatedThreat | Status | GapDescription |
|---|-----|------|---------|------|---------|
| LLM01 | Prompt Injection | PromptInjection | {LLM01_THREATS} | {LLM01_STATUS} | {LLM01_GAP} |
| LLM02 | Insecure Output | NotSecurityOutputProcessing | {LLM02_THREATS} | {LLM02_STATUS} | {LLM02_GAP} |
| LLM03 | Training Data Poisoning | Training Data Poisoning | {LLM03_THREATS} | {LLM03_STATUS} | {LLM03_GAP} |
| LLM04 | Model DoS | ModelDenialService | {LLM04_THREATS} | {LLM04_STATUS} | {LLM04_GAP} |
| LLM05 | Supply Chain | Supply chainVulnerability | {LLM05_THREATS} | {LLM05_STATUS} | {LLM05_GAP} |
| LLM06 | Sensitive Info | SensitiveInformationDisclosure | {LLM06_THREATS} | {LLM06_STATUS} | {LLM06_GAP} |
| LLM07 | Insecure Plugin | NotSecurityPluginDesign | {LLM07_THREATS} | {LLM07_STATUS} | {LLM07_GAP} |
| LLM08 | Excessive Agency | ExcessiveAgency | {LLM08_THREATS} | {LLM08_STATUS} | {LLM08_GAP} |
| LLM09 | Overreliance | ExcessiveDependency | {LLM09_THREATS} | {LLM09_STATUS} | {LLM09_GAP} |
| LLM10 | Model Theft | ModelTheft | {LLM10_THREATS} | {LLM10_STATUS} | {LLM10_GAP} |

### 3.2 DetailedAnalysis

{LLM_DETAILED_ANALYSIS}

---

## 4. CWE Mapping

### 4.1 By CWE GroupRisk Inventory

| CWE | Name | ThreatCount | MostHighSeverity | RelatedThreat |
|-----|------|--------|-------------|---------|
{CWE_GROUPED_TABLE}
<!--
Format:
| CWE-89 | SQLInjection | 3 | 🔴 Critical | T-T-DS01-001, T-I-DS01-002 |
| CWE-79 | XSS | 2 | 🟠 High | T-T-P01-003, T-I-P01-004 |
| CWE-352 | CSRF | 1 | 🟡 Medium | T-T-P01-005 |
-->

### 4.2 CWE Top 25 Coverage

| Rank | CWE | Name | Is Involved | RelatedThreat |
|------|-----|------|---------|---------|
{CWE_TOP25_TABLE}
<!--
Format:
| 1 | CWE-787 | Out of boundsWrite | No | - |
| 2 | CWE-79 | XSS | Is | T-T-P01-003 |
| 3 | CWE-89 | SQLInjection | Is | T-T-DS01-001 |
-->

---

## 5. NIST CSF Mapping

### 5.1 FunctionDomainMapping

| Function | Category | RelatedControl | Compliant | Partial | Non-compliant |
|------|------|---------|------|------|--------|
| **Identification (ID)** | AssetManagement | {ID_AM_CONTROLS} | {ID_AM_PASS} | {ID_AM_PARTIAL} | {ID_AM_FAIL} |
| | BusinessEnvironment | {ID_BE_CONTROLS} | {ID_BE_PASS} | {ID_BE_PARTIAL} | {ID_BE_FAIL} |
| | RiskAssessment | {ID_RA_CONTROLS} | {ID_RA_PASS} | {ID_RA_PARTIAL} | {ID_RA_FAIL} |
| **Protection (PR)** | AccessControl | {PR_AC_CONTROLS} | {PR_AC_PASS} | {PR_AC_PARTIAL} | {PR_AC_FAIL} |
| | DataSecurity | {PR_DS_CONTROLS} | {PR_DS_PASS} | {PR_DS_PARTIAL} | {PR_DS_FAIL} |
| | ProtectionTechnology | {PR_PT_CONTROLS} | {PR_PT_PASS} | {PR_PT_PARTIAL} | {PR_PT_FAIL} |
| **Detection (DE)** | ExceptionDetection | {DE_AE_CONTROLS} | {DE_AE_PASS} | {DE_AE_PARTIAL} | {DE_AE_FAIL} |
| | Continuous Monitoring | {DE_CM_CONTROLS} | {DE_CM_PASS} | {DE_CM_PARTIAL} | {DE_CM_FAIL} |
| **Response (RS)** | ResponsePlan | {RS_RP_CONTROLS} | {RS_RP_PASS} | {RS_RP_PARTIAL} | {RS_RP_FAIL} |
| | Mitigation Measures | {RS_MI_CONTROLS} | {RS_MI_PASS} | {RS_MI_PARTIAL} | {RS_MI_FAIL} |
| **Recovery (RC)** | RecoveryPlan | {RC_RP_CONTROLS} | {RC_RP_PASS} | {RC_RP_PARTIAL} | {RC_RP_FAIL} |

### 5.2 Control MeasuresMapping

| NIST Control | ControlDescription | RelatedMitigation Measures | Status |
|-----------|---------|-------------|------|
{NIST_CONTROLS_TABLE}
<!--
Format:
| PR.AC-1 | IdentityManagement | M-001, M-003 | ✅ |
| PR.DS-1 | StaticDataProtection | M-006 | ⚠️ |
| PR.DS-2 | TransmissionDataProtection | M-007, M-017 | ✅ |
-->

---

## 6. GapAnalysis

### 6.1 NotCoverageControl

| Framework | Control | Description | RiskImpact | RecommendationPriority |
|------|------|------|---------|-----------|
{UNCOVERED_CONTROLS_TABLE}
<!--
Format:
| OWASP | A09 | SecurityLog and Monitoring | High | P1 |
| NIST | DE.CM-1 | NetworkMonitoring | Medium | P2 |
-->

### 6.2 Partially ImplementedControl

| Framework | Control | when BeforeStatus | GapDescription | RecommendationMeasure |
|------|------|---------|---------|---------|
{PARTIAL_CONTROLS_TABLE}
<!--
Format:
| OWASP | A01 | BasicRBACImplementation | Lacks Fine-grained Permission Control | M-016 |
| NIST | PR.AC-4 | SessionManagement | LacksSessionTimeout | M-003 |
-->

### 6.3 RecommendationImprovement

#### Short-termImprovement (P0-P1)

{SHORT_TERM_IMPROVEMENTS}
<!--
Format:
1. **EnhancementAccessControl** (A01)
 - Implementation Fine-grained RBAC
 - ReferenceMeasure: M-016

2. **Strengthen Log Monitoring** (A09)
 - ImplementationSecurityEventLog
 - ReferenceMeasure: M-005, M-008
-->

#### Long-termImprovement (P2-P3)

{LONG_TERM_IMPROVEMENTS}

---

## 7. Threat-ComplianceMappingMatrix

### 7.1 CompleteMappingTable

| Threat ID | Risk Name | CWE | OWASP | NIST | ISO |
|--------|---------|-----|-------|------|-----|
{THREAT_COMPLIANCE_MATRIX}
<!--
Format:
| T-S-P01-001 | JWTForgery | CWE-347 | A07 | PR.AC-1 | A.9.4.2 |
| T-T-DS01-001 | SQLInjection | CWE-89 | A03 | PR.DS-5 | A.14.2.5 |
-->

### 7.2 HeatmapDiagram

```
{COMPLIANCE_HEATMAP}
```
<!--
Example:
 OWASP CWE NIST ISO
ThreatCount ████ ███ ██ ██
Critical ██ ██ █ █
High ███ ██ ██ ██
-->

---

## 8. ComplianceActionActionPlan

### 8.1 ByPrioritySort

| Priority | ComplianceGap | RelatedThreat | RecommendationMeasure | Expected Impact |
|--------|---------|---------|---------|---------|
{COMPLIANCE_ACTION_PLAN}

### 8.2 Compliance Roadmap Diagram

```
{COMPLIANCE_ROADMAP}
```
<!--
Example:
Phase 1 (P0): OWASP A03, A07 → Basic Security Hardening
 ↓
Phase 2 (P1): OWASP A01, A02 → AccessControl and Encryption
 ↓
Phase 3 (P2): OWASP A09, NIST DE → Monitoring and Detection
 ↓
Phase 4 (P3): ISO 27001 → Management System Improvement
-->

---

## Appendices

### Appendices A: FrameworkVersionDescription

| Framework | Version | PublishDate | ReferenceLink |
|------|------|---------|---------|
| OWASP Top 10 | 2021 | 2021-09 | https://owasp.org/Top10/ |
| OWASP LLM Top 10 | 2023.1 | 2023-08 | https://owasp.org/www-project-top-10-for-large-language-model-applications/ |
| CWE Top 25 | 2023 | 2023-06 | https://cwe.mitre.org/top25/ |
| NIST CSF | 2.0 | 2024-02 | https://www.nist.gov/cyberframework |
| ISO 27001 | 2022 | 2022-10 | https://www.iso.org/isoiec-27001-information-security.html |

### Appendices B: TerminologyTable

| Terminology | Definition |
|------|------|
| Compliant | CompleteSatisfyControlRequirement |
| Partially Compliant | PartialSatisfyControlRequirement，ExistsGap |
| Non-compliant | Not Implemented or NotSatisfyControlRequirement |
| Not Applicable | ControlNot ApplicableIn when BeforeProjectScope |

---

**ReportEnd**
