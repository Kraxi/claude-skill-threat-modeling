<!-- Code-First Deep Threat Modeling Workflow | Version 2.1.0 | https://github.com/fr33d3m0n/skill-threat-modeling | License: BSD-3-Clause | Welcome to cite but please retain all sources and declarations -->

# Risk Detail Schema

> **Version**: 2.0.0
> **Last Updated**: 2026-01-02
> **Module**: Report Module v2.0.4

---

## 1. Overview

This document definesin the threat modeling processrisk details standard data format，ensure all risk information completeness and consistency。

**Applicable Scope**:
- Phase 5 (STRIDE Analysis) generatedThreat
- Phase 6 (Risk Validation) validated attack path
- Phase 7 (Mitigation) generatedMitigation Measure
- Phase 8 (Report) output risk details block

---

## 2. Core Entity Model

### 2.0 Entity Relationship Overview

```
Finding (P1-P4) → Threat (P5) → ValidatedRisk (P6) → Mitigation (P7)
 F-P{N}-{Seq} T-{S}-{E}-{Seq} VR-{Seq} M-{Seq}
 │
 threat_refs[] (MANDATORY)
```

**Quantity Conservation Formula**: `P5.total = consolidated_into_vr + excluded_with_reason`

### 2.1 Threat - Phase 5

**IDFormat**: `T-{STRIDE}-{ElementID}-{Seq}`

| Field | Format | Example |
|------|------|------|
| STRIDE | S/T/R/I/D/E | S, T, R, I, D, E |
| ElementID | P{NN}/DS{NN}/DF{NN}/EI{NN} | P01, DS01, DF01, EI01 |
| Seq | NNN | 001, 002, 003 |

**Example**: `T-S-P01-001`, `T-T-DS01-002`, `T-I-DF03-001`

### 2.2 Validated Risk - Phase 6

**IDFormat**: `VR-{Seq}`

| Field | Format | Example |
|------|------|------|
| Seq | NNN | 001, 002, 003 |

**Example**: `VR-001`, `VR-015`

```yaml
ValidatedRisk:
 id: "VR-001"
 threat_refs: ["T-T-P13-001", "T-T-P13-002", "T-E-P13-001"] # ⚠️ MANDATORY
 finding_refs: ["F-P4-003"] # Optional
 severity: critical
 cvss_score: 10.0
 validation_status: verified
```

> ⚠️ **Key Field**: `threat_refs[]` Required，traces to P5 original threat，used for quantity conservation validation

**Prohibited ID Format**:
- ❌ `RISK-001` → use `VR-001`
- ❌ `T-E-RCE-001` → use `T-E-P13-001` (preserve ElementID)

### 2.3 Finding - Phase 1-4

**IDFormat**: `F-P{N}-{Seq}`

| Field | Format | Example |
|------|------|------|
| Phase | P1/P2/P3/P4 | P1, P2, P3, P4 |
| Seq | NNN | 001, 002, 003 |

**Example**: `F-P1-001`, `F-P4-002`

### 2.4 Mitigation - Phase 7

**IDFormat**: `M-{Seq}`

| Field | Format | Example |
|------|------|------|
| Seq | NNN | 001, 002, 003 |

**Example**: `M-001`, `M-005`

---

## 3. risk details standard format

### 3.1 YAML Schema definition

```yaml
# risk-detail.schema.yaml
# Threatdetails standardFormat - v1.0.0

risk_detail:
 # ============================================
 # Basic Information (Basic Information)
 # ============================================
 id:
 type: string
 required: true
 format: "VR-{Seq}"
 description: "Validated Riskunique identifier"
 example: "VR-001"

 # ============================================
 # Traceability References (Traceability References) - NEW v2.0
 # ============================================
 threat_refs:
 type: array[string]
 required: true # ⚠️ MANDATORY
 format: "T-{STRIDE}-{ElementID}-{Seq}"
 description: "all threats that are the source of this riskThreat ID (from P5)"
 example: ["T-T-P13-001", "T-T-P13-002", "T-E-P13-001"]
 min_length: 1
 validation: "used for quantity conservation validation: consolidated + excluded = P5.total"

 finding_refs:
 type: array[string]
 required: false
 format: "F-P{N}-{Seq}"
 description: "source of this risk P1-P4 Finding (Optional)"
 example: ["F-P4-003"]

 name:
 type: string
 required: true
 max_length: 100
 description: "risk short name"
 example: "JWT Token forgery"

 stride_category:
 type: enum
 required: true
 values: [S, T, R, I, D, E]
 mapping:
 S: "Spoofing"
 T: "Tampering"
 R: "Repudiation"
 I: "Information Disclosure"
 D: "Denial of Service"
 E: "Elevation of Privilege (Elevation of Privilege)"

 element_id:
 type: string
 required: true
 format: "P{NN}|DS{NN}|DF{NN}|EI{NN}|TB{NN}"
 description: "impacted DFD elementID"
 example: "P01"

 element_name:
 type: string
 required: true
 description: "affectedimpactelement name"
 example: "authenticationservice (AuthService)"

 # ============================================
 # descriptioninformation (Description)
 # ============================================
 description:
 brief:
 type: string
 required: true
 max_length: 200
 description: "one sentenceriskdescription"
 example: "attacker can forgery JWT token bypass identity authentication"

 detailed:
 type: string
 required: true
 min_length: 100
 description: "detailedtechnologydescription, including attack principle andimpactrange"
 example: |
 system use weak key(such as default key or short key)sign JWT token，
 attacker can throughbrute forceor known key list to guess the key，
 then forgery valid authentication token access system resources。

 # ============================================
 # locationinformation (Location)
 # ============================================
 location:
 component:
 type: string
 required: true
 description: "affectedimpactcomponentname"
 example: "packages/cli/src/auth"

 file:
 type: string
 required: true
 description: "filepath"
 example: "packages/cli/src/auth/jwt.service.ts"

 line_range:
 type: string
 required: false
 format: "L{start}-L{end}"
 description: "coderowrange"
 example: "L45-L67"

 code_snippet:
 type: string
 required: false
 max_length: 500
 description: "relatedcodefragment(Optional)"
 example: |
 const token = jwt.sign(payload, 'weak-secret-key', {
 expiresIn: '24h'
 });

 # ============================================
 # reasonanalysis (Cause Analysis)
 # ============================================
 cause_analysis:
 root_cause:
 type: string
 required: true
 description: "rootroot causeanalysis"
 example: "usehardencoding weak keyperformJWTsignature"

 contributing_factors:
 type: array[string]
 required: false
 description: "contributing factors list"
 example:
 - "lack of key rotation mechanism"
 - "notusekey managementservice"

 related_cwe:
 type: string
 required: true
 format: "CWE-{NNN}"
 description: "relatedCWEnumber"
 example: "CWE-347"
 kb_lookup: true

 related_capec:
 type: string
 required: false
 format: "CAPEC-{NNN}"
 description: "relatedCAPECnumber"
 example: "CAPEC-233"
 kb_lookup: true

 # ============================================
 # attackinformation (Attack Information)
 # ============================================
 attack_info:
 attack_path:
 type: string
 required: true
 description: "attack pathdescription"
 format: "Entry → Step1 → Step2 → ... → Impact"
 example: "attacker → obtainvalidJWT → brute forcekey → forgeryadministratorToken → accessmanagementAPI"

 prerequisites:
 type: array[string]
 required: false
 description: "attack prerequisitescondition"
 example:
 - "canenoughobtainoneindividualvalid JWTtokensampleroot"
 - "have computational resources for keybrute force"

 attck_technique:
 type: string
 required: false
 format: "T{NNNN}"
 description: "MITRE ATT&CK technologynumber"
 example: "T1078"
 kb_lookup: true

 poc_method:
 type: object
 required: true
 properties:
 type:
 type: enum
 values: [manual, automated, command, script]
 description: "validationmethodType"
 description:
 type: string
 required: true
 description: "POCvalidationmethoddescription"
 command:
 type: string
 required: false
 description: "validationcommandorscriptroot"
 example:
 type: "command"
 description: "use jwt-cracker toolbrute forceweak key"
 command: "jwt-cracker <token> --max-length 10"

 exploitability:
 type: enum
 required: true
 values: [Very High, High, Medium, Low]
 description: "canexploitpropertyassessment"
 scoring:
 Very High: "can be exploited without special conditions"
 High: "needneedminimal prerequisitescondition"
 Medium: "needneedspecificconditionortechnologycanpower"
 Low: "needneedcomplexconditionorhighleveltechnology"

 # ============================================
 # impactassessment (Impact Assessment)
 # ============================================
 impact:
 confidentiality:
 type: enum
 required: true
 values: [High, Medium, Low, None]
 description: "confidentialityimpact"

 integrity:
 type: enum
 required: true
 values: [High, Medium, Low, None]
 description: "completenessimpact"

 availability:
 type: enum
 required: true
 values: [High, Medium, Low, None]
 description: "availablepropertyimpact"

 cvss_score:
 type: float
 required: true
 range: [0.0, 10.0]
 description: "CVSS 3.1 scoring"
 example: 8.8

 cvss_vector:
 type: string
 required: false
 format: "CVSS:3.1/AV:.../AC:.../..."
 description: "CVSS vector string"
 example: "CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H"

 # ============================================
 # Mitigation
 # ============================================
 mitigation:
 priority:
 type: enum
 required: true
 values: [P0, P1, P2, P3]
 mapping:
 P0: "immediatefix - Critical risk"
 P1: "urgent - High risk"
 P2: "highprioritylevel - Medium risk"
 P3: "planmedium - Low risk"

 strategy:
 type: string
 required: true
 description: "mitigationpolicyOverview"
 example: "usestrong keyandkey managementservice，implement key rotation"

 short_term:
 description:
 type: string
 required: true
 description: "short-termfixsolutiondescription"
 implementation:
 type: string
 required: false
 description: "codeorconfigurationExample"

 long_term:
 description:
 type: string
 required: false
 description: "long-term solutiondescription"
 implementation:
 type: string
 required: false
 description: "architecturelevelimprovement solution"

 kb_reference:
 type: string
 required: false
 description: "knowledgelibraryreference source"
 example: "codeguard-authentication.yaml → jwt_weak_signing_key"
```

---

## 4. severitymapping

### 4.1 CVSS toseverity

| CVSS scoring | severity | figuremark | prioritylevel |
|-----------|---------|------|--------|
| 9.0 - 10.0 | Critical | 🔴 | P0 |
| 7.0 - 8.9 | High | 🟠 | P1 |
| 4.0 - 6.9 | Medium | 🟡 | P2 |
| 0.1 - 3.9 | Low | 🟢 | P3 |

### 4.2 STRIDE todefaultimpact

| STRIDE | mainimpact | default CIA |
|--------|---------|----------|
| Spoofing (S) | confidentiality、completeness | C:H, I:M, A:N |
| Tampering (T) | completeness | C:L, I:H, A:M |
| Repudiation (R) | notcanrepudiation | C:L, I:M, A:N |
| Info Disclosure (I) | confidentiality | C:H, I:N, A:N |
| DoS (D) | availableproperty | C:N, I:N, A:H |
| EoP (E) | completeness、confidentiality | C:H, I:H, A:M |

---

## 5. Fieldcompletenessrule

### 5.1 RequiredFieldchecklist

followingFieldinplacehaverisk detailsmediummustfill in:

```yaml
required_fields:
 core:
 - id # riskID (VR-xxx)
 - threat_refs # ⚠️ original threatreference (MANDATORY v2.0)
 - name # riskname
 - stride_category # STRIDEcategory
 - element_id # affectedimpactelement
 - element_name # elementname

 description:
 - description.brief # brief description
 - description.detailed # detaileddescription

 location:
 - location.component # componentname
 - location.file # filelocation

 cause:
 - cause_analysis.root_cause # rootroot cause
 - cause_analysis.related_cwe # CWEmapping

 attack:
 - attack_info.attack_path # attack path
 - attack_info.poc_method # POCmethod
 - attack_info.exploitability # canexploitproperty

 impact:
 - impact.confidentiality # confidentialityimpact
 - impact.integrity # completenessimpact
 - impact.availability # availablepropertyimpact
 - impact.cvss_score # CVSSscoring

 mitigation:
 - mitigation.priority # prioritylevel
 - mitigation.strategy # mitigationpolicy
 - mitigation.short_term.description # short-termfix
```

### 5.2 completenessvalidationrule

```yaml
validation_rules:
 - name: "IDFormatvalidation"
 field: "id"
 rule: "matches('^T-[STRIDE]-[A-Z]+[0-9]+-[0-9]{3}$')"

 - name: "descriptionminimumlength"
 field: "description.detailed"
 rule: "length >= 100"

 - name: "CWEFormatvalidation"
 field: "cause_analysis.related_cwe"
 rule: "matches('^CWE-[0-9]+$')"

 - name: "CVSSrangevalidation"
 field: "impact.cvss_score"
 rule: "value >= 0.0 AND value <= 10.0"

 - name: "attack pathFormat"
 field: "attack_info.attack_path"
 rule: "contains('→')"

completeness_threshold: 95%
```

---

## 6. Markdown outputtemplate

followingisrisk detailsinreportmedium standard Markdown Format:

```markdown
### {id}: {name}

**Basic Information**:
| attribute | value |
|------|-----|
| riskID | {id} |
| **Threat Refs** | {threat_refs} |
| STRIDEType | {stride_category_full} |
| affectedimpactelement | {element_id} - {element_name} |
| severity | {severity_icon} {severity} |
| CVSSscoring | {cvss_score} |

**riskdescription**:
{description.brief}

**detailedDescription**:
{description.detailed}

**locationlocate**:
- **component**: {location.component}
- **file**: `{location.file}:{location.line_range}`
- **keycode**:
 ```{language}
 {location.code_snippet}
 ```

**reasonanalysis**:
- **rootroot cause**: {cause_analysis.root_cause}
- **relatedCWE**: {cause_analysis.related_cwe} ({cwe_name})
- **relatedCAPEC**: {cause_analysis.related_capec} ({capec_name})

**attack path**:
```
{attack_info.attack_path}
```

**prerequisitescondition**:
{attack_info.prerequisites - as numbered list}

**ATT&CKmapping**: {attack_info.attck_technique} - {attck_name}

**POCvalidationmethod**:
```{poc_language}
{attack_info.poc_method.command}
```

**impactassessment**:
| dimension | impactdegree |
|------|---------|
| confidentiality | {impact.confidentiality} |
| completeness | {impact.integrity} |
| availableproperty | {impact.availability} |

**Mitigation Measure**:

**prioritylevel**: {mitigation.priority} - {priority_description}

**short-termfix**:
{mitigation.short_term.description}
```{language}
{mitigation.short_term.implementation}
```

**long-term solution**:
{mitigation.long_term.description}

**KBreference**: {mitigation.kb_reference}

---
```

---

## 7. andother Schema relationship

```
┌─────────────────────────────────────────────────────────────────┐
│ Schema Dependencies │
├─────────────────────────────────────────────────────────────────┤
│ │
│ risk-detail.schema.md (rootdocument) │
│ │ │
│ ├─── byreferused for: phase-risk-summary.schema.md │
│ │ (PhaserisksummaryFormat) │
│ │ │
│ ├─── byreferused for: assets/templates/RISK-INVENTORY.template.md │
│ │ (riskchecklistreporttemplate) │
│ │ │
│ └─── byreferused for: assets/templates/THREAT-MODEL-REPORT.template.md │
│ (mainreportThreatdetailssection) │
│ │
│ depend onrelationship: │
│ - report-naming.schema.md (reportNaming Convention) │
│ - knowledge/codeguard-*.yaml (KBquerysource) │
│ │
└─────────────────────────────────────────────────────────────────┘
```

---

## 8. Versionhistory

| Version | date | changeDescription |
|------|------|---------|
| 1.0.0 | 2025-12-26 | initialVersion，definitionThreatdetails standardFormat |
| 2.0.0 | 2026-01-02 | **dataarchitecturerefactoring**: add ValidatedRisk entity，`threat_refs[]` RequiredField，Quantity Conservation Formula |

---

**documentend**
