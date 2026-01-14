<!-- Code-First Deep Threat Modeling Workflow | Version 2.1.0 | https://github.com/fr33d3m0n/skill-threat-modeling | License: BSD-3-Clause | Welcome to cite but please retain all sources and declarations -->

# Risk InventoryReport: {PROJECT_NAME}

> **Assessment Time**: {ASSESSMENT_DATETIME}
> **AnalysisAnalyst**: Claude (Deep Risk Analysis)
> **FrameworkVersion**: STRIDE-TM v1.0.2
> **ReportVersion**: {REPORT_VERSION}
> **TotalRiskCount**: {TOTAL_RISK_COUNT}

---

## 1. RiskStatisticsSummary

### 1.1 BySeverityStatistics

| Severity | Count | Percentage | Description |
|---------|------|--------|------|
| 🔴 **Critical** | {CRITICAL_COUNT} | {CRITICAL_PCT}% | CVSS 9.0-10.0，Requires immediate fix |
| 🟠 **High** | {HIGH_COUNT} | {HIGH_PCT}% | CVSS 7.0-8.9，7Fix within days |
| 🟡 **Medium** | {MEDIUM_COUNT} | {MEDIUM_PCT}% | CVSS 4.0-6.9，30Fix within days |
| 🟢 **Low** | {LOW_COUNT} | {LOW_PCT}% | CVSS 0.1-3.9，Planned fix |
| **Total** | **{TOTAL_COUNT}** | **100%** | |

### 1.2 By STRIDE CategoryStatistics

| STRIDE | Name | Count | Percentage | Critical | High | Medium | Low |
|--------|------|------|------|----------|------|--------|-----|
| **S** | Spoofing (Deception) | {S_COUNT} | {S_PCT}% | {S_CRITICAL} | {S_HIGH} | {S_MEDIUM} | {S_LOW} |
| **T** | Tampering (Tampering) | {T_COUNT} | {T_PCT}% | {T_CRITICAL} | {T_HIGH} | {T_MEDIUM} | {T_LOW} |
| **R** | Repudiation (Repudiation) | {R_COUNT} | {R_PCT}% | {R_CRITICAL} | {R_HIGH} | {R_MEDIUM} | {R_LOW} |
| **I** | Info Disclosure (InformationDisclosure) | {I_COUNT} | {I_PCT}% | {I_CRITICAL} | {I_HIGH} | {I_MEDIUM} | {I_LOW} |
| **D** | DoS (DenialService) | {D_COUNT} | {D_PCT}% | {D_CRITICAL} | {D_HIGH} | {D_MEDIUM} | {D_LOW} |
| **E** | EoP (PermissionElevation) | {E_COUNT} | {E_PCT}% | {E_CRITICAL} | {E_HIGH} | {E_MEDIUM} | {E_LOW} |
| **Total** | | **{TOTAL_COUNT}** | **100%** | {TOTAL_CRITICAL} | {TOTAL_HIGH} | {TOTAL_MEDIUM} | {TOTAL_LOW} |

### 1.3 ByComponentDistributionStatistics

| Component | Element ID | RiskCount | MostHighLevel | HighRiskRatioExample |
|------|--------|--------|---------|-----------|
{COMPONENT_DISTRIBUTION_TABLE}
<!--
Format:
| API Gateway | P01 | 12 | 🔴 Critical | 67% |
| Auth Service | P02 | 8 | 🔴 Critical | 50% |
| Database | DS01 | 6 | 🟠 High | 33% |
| Frontend | P03 | 4 | 🟡 Medium | 0% |
-->

### 1.4 ByStatusStatistics

| Status | Count | Percentage |
|------|------|--------|
| PendingFix (Pending) | {PENDING_COUNT} | {PENDING_PCT}% |
| In Progress (In Progress) | {INPROGRESS_COUNT} | {INPROGRESS_PCT}% |
| AlreadyMitigation (Mitigated) | {MITIGATED_COUNT} | {MITIGATED_PCT}% |
| AlreadyAccept (Accepted) | {ACCEPTED_COUNT} | {ACCEPTED_PCT}% |
| False positive (False Positive) | {FP_COUNT} | {FP_PCT}% |

### 1.5 CountConservationValidation (Count Conservation)

> ⚠️ **ThisTableUseInValidationThreatToRiskConversionCompleteCapability**

| CheckItem | Count | Description |
|--------|------|------|
| P5 ThreatTotalCount | {P5_THREAT_TOTAL} | FromFrom P5-STRIDE-THREATS.md |
| Merged into VR Threat | {THREATS_CONSOLIDATED} | through threat_refs Traceability |
| ExcludeThreat | {THREATS_EXCLUDED} | With Exclusion Reason |
| **ConservationValidation** | **{CONSERVATION_STATUS}** | `{THREATS_CONSOLIDATED} + {THREATS_EXCLUDED} = {P5_THREAT_TOTAL}` |

<!--
ValidationRule:
✅ Conservation through: THREATS_CONSOLIDATED + THREATS_EXCLUDED = P5_THREAT_TOTAL
❌ ConservationFailure: Threat Lost，NeedCheck P6 threat_disposition Table
-->

---

## 2. RiskSummaryTable

### 2.1 Critical Risk

| Risk ID | STRIDE | Element | Risk Name | Threat Refs | CWE | CVSS | Status |
|--------|--------|------|---------|-------------|-----|------|------|
{CRITICAL_RISKS_TABLE}
<!--
FormatExample:
| VR-001 | T,E | P13 | CodeExecutionRisk | T-T-P13-001, T-T-P13-002, T-E-P13-001 | CWE-94 | 10.0 | Pending |
Note: Threat Refs ListTraceabilityTo P5 OriginalThreat，UseInCountConservationValidation
-->

### 2.2 High Risk

| Risk ID | STRIDE | Element | Risk Name | Threat Refs | CWE | CVSS | Status |
|--------|--------|------|---------|-------------|-----|------|------|
{HIGH_RISKS_TABLE}

### 2.3 Medium Risk

| Risk ID | STRIDE | Element | Risk Name | Threat Refs | CWE | CVSS | Status |
|--------|--------|------|---------|-------------|-----|------|------|
{MEDIUM_RISKS_TABLE}

### 2.4 Low Risk

| Risk ID | STRIDE | Element | Risk Name | Threat Refs | CWE | CVSS | Status |
|--------|--------|------|---------|-------------|-----|------|------|
{LOW_RISKS_TABLE}

---

## 3. RiskDetails

<!--
EachRiskCompleteDetailsBlock，By risk-detail.schema.md DefinitionFormat
Critical and High RiskMustHaveCompleteDetails
Medium and Low RiskCanUsageSimplifiedFormat
-->

{RISK_DETAILS_SECTION}

<!--
=============================================================================
RiskDetailsTemplate (CompleteFormat - Critical/High Must Have)
=============================================================================

### VR-{SEQ}: {RISK_NAME}

**Basic Information**:

| Property | Value |
|------|-----|
| Risk ID | VR-{SEQ} |
| **Threat Refs** | {THREAT_REFS} |
| STRIDE Type | {STRIDE_FULL_NAME} |
| Affected Element | {ELEMENT_ID} - {ELEMENT_NAME} |
| Severity | {SEVERITY_ICON} {SEVERITY} |
| CVSSScore | {CVSS_SCORE} |
| CVSSVector | `{CVSS_VECTOR}` |

<!--
THREAT_REFS Example: T-T-P13-001, T-T-P13-002, T-E-P13-001
TraceabilityTo P5-STRIDE-THREATS.md MediumOriginalThreat ID
This Field Required，Used to Ensure CountConservationValidation
-->

**RiskDescription**:

{DESCRIPTION_BRIEF}

**DetailedDescription**:

{DESCRIPTION_DETAILED}

**LocationLocate**:

- **Component**: {LOCATION_COMPONENT}
- **File**: `{LOCATION_FILE}:{LOCATION_LINE_RANGE}`
- **KeyCode**:

```{CODE_LANGUAGE}
{LOCATION_CODE_SNIPPET}
```

**ReasonAnalysis**:

- **Root Cause**: {ROOT_CAUSE}
- **Contributing Factor**:
 - {CONTRIBUTING_FACTOR_1}
 - {CONTRIBUTING_FACTOR_2}
- **RelatedCWE**: [{RELATED_CWE}]({CWE_URL}) - {CWE_NAME}
- **RelatedCAPEC**: [{RELATED_CAPEC}]({CAPEC_URL}) - {CAPEC_NAME}

**Attack Path**:

```
{ATTACK_PATH}
```

**Prerequisites**:

1. {PREREQUISITE_1}
2. {PREREQUISITE_2}
3. {PREREQUISITE_3}

**ATT&CKMapping**: [{ATTCK_TECHNIQUE}]({ATTCK_URL}) - {ATTCK_NAME}

**POCValidation Method**:

**Type**: {POC_TYPE} (manual/automated/command/script)

**Description**: {POC_DESCRIPTION}

```{POC_LANGUAGE}
{POC_COMMAND}
```

**ExploitableUseCapability**: {EXPLOITABILITY} (Very High/High/Medium/Low)

**ImpactAssessment**:

| Dimension | Impact Level | Description |
|------|---------|------|
| ConfidentialCapability (C) | {IMPACT_C} | {IMPACT_C_DESC} |
| CompleteCapability (I) | {IMPACT_I} | {IMPACT_I_DESC} |
| CanUseCapability (A) | {IMPACT_A} | {IMPACT_A_DESC} |

**Mitigation Measures**:

**Priority**: {MITIGATION_PRIORITY}

| Priority | Description |
|--------|------|
| P0 | Immediate Fix - Critical Risk，LikelyAlreadyByExploitUse |
| P1 | Urgent - High Risk，7Fix within days |
| P2 | HighPriority - Medium Risk，30Fix within days |
| P3 | PlanMedium - Low Risk，PlanningMediumFix |

**MitigationPolicy**:

{MITIGATION_STRATEGY}

**Short-termFix** (Priority: {SHORT_TERM_PRIORITY}):

{SHORT_TERM_DESCRIPTION}

```{SHORT_TERM_LANGUAGE}
{SHORT_TERM_IMPLEMENTATION}
```

**Long-termSolution** (Priority: {LONG_TERM_PRIORITY}):

{LONG_TERM_DESCRIPTION}

```{LONG_TERM_LANGUAGE}
{LONG_TERM_IMPLEMENTATION}
```

**KBReference**: {KB_REFERENCE}

---

=============================================================================
RiskDetailsTemplate (SimplifiedFormat - Medium/Low Optional)
=============================================================================

### VR-{SEQ}: {RISK_NAME}

| Property | Value |
|------|-----|
| **Threat Refs** | {THREAT_REFS} |
| STRIDE | {STRIDE_FULL_NAME} |
| Element | {ELEMENT_ID} - {ELEMENT_NAME} |
| Severity | {SEVERITY_ICON} {SEVERITY} |
| CVSS | {CVSS_SCORE} |
| CWE | {RELATED_CWE} |
| Location | `{LOCATION_FILE}` |

**Description**: {DESCRIPTION_BRIEF}

**Mitigation**: {MITIGATION_STRATEGY}

---

-->

---

## 4. Risk Trend Analysis

### 4.1 RiskDistributionHeatmapDiagram

```
{RISK_HEATMAP}
```
<!--
Example:
Component S T R I D E Total RiskLevel
───────────────────────────────────────────────────────────────
API Gateway ██ ███ █ ██ ██ ███ 12 ████████ Critical
Auth Service ███ ██ ░ █ ░ ██ 8 ██████ High
Database ░ ███ ░ ██ █ █ 6 █████ High
Frontend █ █ ░ █ █ ░ 4 ███ Medium
Worker ░ ░ ░ █ █ ░ 2 ██ Low

DiagramExample: ███ Critical ██ High █ Medium ░ Low/None
-->

### 4.2 RiskAssociationDiagram

```
{RISK_CORRELATION_MAP}
```
<!--
Example:
T-S-P01-001 (JWTForgery) ────┬──► T-E-P01-002 (PermissionElevation)
 │
 └──► T-T-DS01-001 (DataTampering)
 │
 └──► T-I-DS01-002 (DataDisclosure)
-->

---

## 5. Additional Information

### 5.1 CWE Distribution

| CWE | Name | RiskCount | Severity |
|-----|------|--------|---------|
{CWE_DISTRIBUTION_TABLE}

### 5.2 ATT&CK MappingSummary

| Tactic | Technology | RiskCount | RelatedThreat |
|------|------|--------|---------|
{ATTCK_MAPPING_TABLE}

### 5.3 Knowledge Base Query Records

| Query Type | Parameters | ResultCount | UsageIn |
|---------|------|--------|-------|
{KB_QUERY_LOG}

---

**ReportEnd**

---

> **Note**: ThisRisk Inventory Should be Used with Mitigation MeasuresReport ({PROJECT_NAME}-MITIGATION-MEASURES.md)
> and Attack Path ValidationReport ({PROJECT_NAME}-ATTACK-PATH-VALIDATION.md) CombineUsage。
