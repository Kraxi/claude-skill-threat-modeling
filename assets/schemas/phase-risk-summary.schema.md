<!-- Code-First Deep Threat Modeling Workflow | Version 2.1.0 | https://github.com/fr33d3m0n/skill-threat-modeling | License: BSD-3-Clause | Welcome to cite but please retain all sources and declarations -->

# Phase Risk Summary Schema

> **Version**: 2.0.0
> **Last Updated**: 2026-01-02
> **Module**: Report Module v2.0.4

---

## 1. Overview

This document definesThreatat the end of each modeling phaserisksummarizedstandardFormat，ensureacross phases riskinformationcompletepass。

**design principle**:
- must be output at the end of each phaserisksummary
- Phase 1-4 produceFinding (Finding: F-P{N}-{Seq})
- Phase 5 produceThreat (Threat: T-{STRIDE}-{Element}-{Seq})
- Phase 6 produceValidated Risk (ValidatedRisk: VR-{Seq})
- Phase 7 produceMitigation Measure (Mitigation: M-{Seq})
- **must ensure quantity conservation**: `P5.total = consolidated_into_vr + excluded_with_reason`

**Core Entity Model (v2.0)**:
```
Finding (P1-P4) → Threat (P5) → ValidatedRisk (P6) → Mitigation (P7)
 F-P{N}-{Seq} T-{S}-{E}-{Seq} VR-{Seq} M-{Seq}
 │
 threat_refs[] (MANDATORY)
```

---

## 2. Phase Output Structure

### 2.1 Phase Context Protocol Extension

```yaml
# Current Phase Context Structure
phase_output:
 phase_id: "P{N}"
 phase_name: "Phase Name"
 status: "completed"
 timestamp: "YYYY-MM-DD HH:MM:SS"

 # === new: risksummaryField ===
 risk_summary:
 total_count: N
 by_severity:
 critical: N
 high: N
 medium: N
 low: N
 items: [] # risklist

 phase_reflection:
 key_findings: []
 attention_areas: []
 handover_notes: []
```

---

## 3. Phase 1-4: securityFindingFormat

### 3.1 Security Finding Schema

```yaml
# security-finding.schema.yaml
security_finding:
 id:
 type: string
 required: true
 format: "SF-P{N}-{Seq}"
 example: "SF-P1-001"

 phase:
 type: integer
 required: true
 range: [1, 4]
 description: "FindingsourcePhase"

 type:
 type: enum
 required: true
 values:
 - missing_security_control # missingsecuritycontrol
 - weak_configuration # weakconfiguration
 - design_flaw # design flaw
 - hardcoded_secret # hardencodingsensitiveinformation
 - insufficient_validation # validationnotsufficient
 - missing_encryption # missingencryption
 - insecure_dependency # notsecuritydepend on
 - other # other

 title:
 type: string
 required: true
 max_length: 100
 description: "Findingtitle"

 description:
 type: string
 required: true
 description: "Findingdescription"

 location:
 component:
 type: string
 required: true
 file:
 type: string
 required: false
 line:
 type: string
 required: false

 severity:
 type: enum
 required: true
 values: [Critical, High, Medium, Low, Info]

 risk_indicator:
 type: string
 required: false
 description: "riskindicator, for subsequent phases to dive deeperanalysis"

 recommended_action:
 type: enum
 required: true
 values:
 - deep_analysis_p5 # P5 dive deeperanalysis
 - deep_analysis_p6 # P6 validation
 - immediate_fix # immediatefix
 - monitor # monitoring
 - accept # connectaffected
```

### 3.2 Phase 1-4 risksummarytemplate

```markdown
## PhasesecurityFindingsummary

### P{N} Findingstatistics
| severity | quantity | percentage |
|---------|------|--------|
| Critical | X | X% |
| High | X | X% |
| Medium | X | X% |
| Low | X | X% |
| Info | X | X% |
| **total** | **X** | **100%** |

### rootPhaseFindingchecklist

| FindingID | Type | title | location | severity | subsequent phases |
|--------|------|------|------|---------|---------|
| SF-P{N}-001 | [Type] | [title] | `[file:row]` | 🔴/🟠/🟡/🟢 Critical/High/Medium/Low | P5/P6 |
| SF-P{N}-002 | [Type] | [title] | `[file:row]` | 🟠 High | P5 |

### riskindicator (forsubsequentanalysis)

| indicatordescription | relatedFinding | recommendationanalysisdeepdegree |
|-----------|---------|-------------|
| [indicator1] | SF-P{N}-001 | Deep |
| [indicator2] | SF-P{N}-002, SF-P{N}-003 | Standard |

### Phasereflection

**keyFinding**:
1. [keyFinding1]
2. [keyFinding2]

**needneedconcern**:
1. [concern1]
2. [concern2]

**passed to next phase**:
1. [passinformation1]
2. [passinformation2]

---
```

---

## 4. Phase 5-7: ThreatFormat

### 4.1 Threat Summary Schema

```yaml
# threat-summary.schema.yaml
threat_summary:
 id:
 type: string
 required: true
 format: "T-{STRIDE}-{ElementID}-{Seq}"
 example: "T-S-P01-001"

 stride_category:
 type: enum
 required: true
 values: [S, T, R, I, D, E]

 element_id:
 type: string
 required: true

 element_name:
 type: string
 required: true

 title:
 type: string
 required: true
 max_length: 100

 description_brief:
 type: string
 required: true
 max_length: 200

 cwe:
 type: string
 required: true
 format: "CWE-{NNN}"

 cvss_score:
 type: float
 required: true
 range: [0.0, 10.0]

 severity:
 type: enum
 required: true
 values: [Critical, High, Medium, Low]

 status:
 type: enum
 required: true
 values:
 - identified # P5: alreadyidentify
 - validated # P6: alreadyvalidation
 - mitigated # P7: existing mitigation solution
 - accepted # connectaffectedrisk
 - false_positive # false positive

 validation_result:
 type: object
 required: false # P6 fill
 properties:
 attack_path_confirmed: boolean
 poc_available: boolean
 exploitability: enum[Very High, High, Medium, Low]

 mitigation_status:
 type: object
 required: false # P7 fill
 properties:
 priority: enum[P0, P1, P2, P3]
 strategy_defined: boolean
 short_term_available: boolean
 long_term_available: boolean
```

### 4.2 Phase 5-7 Threatsummarytemplate

```markdown
## PhaseThreatsummary

### P{N} Threatstatistics

#### byseverity
| severity | quantity | percentage |
|---------|------|--------|
| 🔴 Critical | X | X% |
| 🟠 High | X | X% |
| 🟡 Medium | X | X% |
| 🟢 Low | X | X% |
| **total** | **X** | **100%** |

#### by STRIDE category
| STRIDE | name | quantity | Critical | High | Medium | Low |
|--------|------|------|----------|------|--------|-----|
| S | Spoofing | X | X | X | X | X |
| T | Tampering | X | X | X | X | X |
| R | Repudiation | X | X | X | X | X |
| I | Info Disclosure | X | X | X | X | X |
| D | DoS | X | X | X | X | X |
| E | EoP | X | X | X | X | X |

### rootPhaseThreatchecklist

| ThreatID | STRIDE | element | title | CWE | CVSS | severity | status |
|--------|--------|------|------|-----|------|---------|------|
| T-S-P01-001 | S | P01 | [title] | CWE-XXX | X.X | 🔴 Critical | [status] |
| T-T-DS01-001 | T | DS01 | [title] | CWE-XXX | X.X | 🟠 High | [status] |

### highriskelementdistribution

| elementID | elementname | Threatcount | mosthighseverity |
|--------|---------|--------|-------------|
| P01 | [name] | X | 🔴 Critical |
| DS01 | [name] | X | 🟠 High |

### Phasereflection

**keyThreat**:
1. [keyThreat1] - reasonanalysis
2. [keyThreat2] - reasonanalysis

**highriskarea**:
1. [area1]: X individualThreat
2. [area2]: X individualThreat

**passed to next phase**:
1. [needneed P{N+1} handlingitems]
2. [needneed P{N+1} handlingitems]

---
```

---

## 5. PhasespecificField

### 5.1 each phaseoutputextension

```yaml
# Phase 1: projectunderstanding
phase_1_extension:
 project_type: string # Web/API/microservice/AI/LLM
 technology_stack: [] # technologystacklist
 security_relevant_modules: [] # securityrelatedmodule
 initial_attack_surface: string # initialattack surfaceassessment

# Phase 2: DFD build
phase_2_extension:
 dfd_elements:
 processes: [] # P01, P02, ...
 data_stores: [] # DS01, DS02, ...
 data_flows: [] # DF01, DF02, ...
 external_entities: [] # EI01, EI02, ...
 data_classification:
 pii: [] # containPII element
 credentials: [] # containcredentialselement
 sensitive: [] # other sensitivedata

# Phase 3: trust boundary
phase_3_extension:
 trust_boundaries:
 - id: "TB01"
 name: "edgeboundaryname"
 type: "network|process|user|cloud"
 elements_inside: []
 crossing_flows: []
 critical_interfaces: [] # keyinterfacelist

# Phase 4: securitydesignassessment
phase_4_extension:
 security_domains:
 authentication:
 status: "implemented|partial|missing"
 gaps: []
 authorization:
 status: "implemented|partial|missing"
 gaps: []
 # ... other 9 individualsecuritydomain
 design_matrix: {} # securitydesignmatrix

# Phase 5: STRIDE analysis
phase_5_extension:
 stride_matrix: {} # STRIDE per Interaction matrix
 threat_generation_filters: [] # shouldusedfilterdevice
 kb_queries: [] # knowledgelibraryqueryrecord

# Phase 6: riskvalidation (v2.0 Updated)
phase_6_extension:
 # Validated Risklist (newFormat)
 validated_risks:
 - id: "VR-001"
 threat_refs: ["T-T-P13-001", "T-T-P13-002"] # ⚠️ MANDATORY
 finding_refs: ["F-P4-003"] # Optional
 severity: critical
 cvss_score: 10.0
 validation_status: verified

 # Threathandlingrecord (quantityconservationtraceability)
 threat_disposition:
 input_count: 120 # P5 Threattotal
 output_summary:
 consolidated_into_vr: 98 # mergeto VR
 excluded_with_reason: 22 # exclude (havemanageby)
 validation_formula: "98 + 22 = 120 ✅"
 vr_threat_mapping:
 VR-001: ["T-T-P13-001", "T-T-P13-002", "T-E-P13-001"]
 excluded_threats:
 - threat_id: "T-S-P02-002"
 reason: "MITIGATED - escape_filter_chars() applied"

 attack_paths_confirmed: [] # confirm attack path
 poc_methods: [] # POC method

# Phase 7: Mitigation Measure
phase_7_extension:
 mitigation_plan:
 p0_items: [] # immediatefix
 p1_items: [] # urgent
 p2_items: [] # highprioritylevel
 p3_items: [] # planmedium
 defense_in_depth: {} # depthdeepdefense architecture
 compliance_mapping: {} # compliancemapping
```

---

## 6. accumulationriskchecklist

### 6.1 Full Risk Registry Schema

```yaml
# complete accumulation across phasesriskchecklist
full_risk_registry:
 metadata:
 project_name: string
 created_at: datetime
 last_updated: datetime
 total_risks: integer

 # Phase 1-4 securityFinding
 security_findings:
 - id: "SF-P{N}-XXX"
 phase: N
 # ... security_finding schema fields
 status: "open|addressed|deferred"

 # Phase 5-7 Threat
 threats:
 - id: "T-X-XX-XXX"
 # ... threat_summary schema fields
 full_detail: {} # completerisk details (risk-detail.schema.md)

 # summarystatistics
 summary:
 by_phase:
 P1: { total: N, critical: N, high: N, medium: N, low: N }
 P2: { total: N, critical: N, high: N, medium: N, low: N }
 # ...
 by_severity:
 Critical: N
 High: N
 Medium: N
 Low: N
 by_stride:
 S: N
 T: N
 R: N
 I: N
 D: N
 E: N
 by_element:
 P01: N
 DS01: N
 # ...
```

---

## 7. Phase 8 contextaggregate

### 7.1 Aggregated Context Schema

```yaml
# P8 aggregatecontextStructure
aggregated_context:
 # P1-P7 Phase Output
 phase_outputs:
 P1: { project_context, security_findings }
 P2: { dfd_elements, security_findings }
 P3: { boundary_context, security_findings }
 P4: { security_gaps, security_findings }
 P5: { threat_inventory, stride_matrix }
 P6: { validated_threats, attack_paths }
 P7: { mitigation_plan, compliance_mapping }

 # completeriskchecklist
 full_risk_registry: {} # see above

 # statistical information required for report generation
 report_statistics:
 total_threats: N
 critical_count: N
 high_count: N
 medium_count: N
 low_count: N
 mitigated_count: N
 pending_count: N

 # quality indicator
 quality_metrics:
 completeness_score: float # Fieldcompleteness
 coverage_score: float # elementcoverage rate
 validation_score: float # validationcompletion rate
```

---

## 8. andother Schema relationship

```
┌─────────────────────────────────────────────────────────────────┐
│ Schema Dependencies │
├─────────────────────────────────────────────────────────────────┤
│ │
│ phase-risk-summary.schema.md (rootdocument) │
│ │ │
│ ├─── depend on: risk-detail.schema.md │
│ │ (ThreatdetailscompleteFormat) │
│ │ │
│ ├─── byreferused for: WORKFLOW.md → Phase 1-7 section │
│ │ (each phaserisksummaryneedrequire) │
│ │ │
│ └─── byreferused for: WORKFLOW.md → Phase 8 Step 8.1 │
│ (contextaggregatestep) │
│ │
└─────────────────────────────────────────────────────────────────┘
```

---

## 9. Versionhistory

| Version | date | changeDescription |
|------|------|---------|
| 1.0.0 | 2025-12-26 | initialVersion，definitionPhaserisksummaryFormat |
| 2.0.0 | 2026-01-02 | **dataarchitecturerefactoring**: add ValidatedRisk entity，`threat_refs[]` Required，`threat_disposition` traceabilitytable |

---

**documentend**
