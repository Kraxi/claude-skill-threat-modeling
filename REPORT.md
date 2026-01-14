<!-- Code-First Deep Threat Modeling Workflow | Version 2.1.1 | https://github.com/fr33d3m0n/skill-threat-modeling | License: BSD-3-Clause | Welcome to cite but please retain all sources and declarations -->

# REPORT.md - Phase 7-8: Mitigation & Report Generation

> **Version**: 2.1.0
> **Scope**: Mitigation Measures generation + comprehensive report generation
> **Input**: P6 `validated_risks` (integrityRisk Inventory)
> **Output**: 4 Required reports + 4optional reports + phaseprocess document

---

## ⚠️ CRITICAL: Content Aggregation Requirements (content aggregation mandatory requirement)

> **Root Cause Fix**: this section addresses sparse content in final reports、omissionfundamentalcause

### coreprinciple

**final reportmust**:
1. **explicit read** allphase file (not dependent on context memory)
2. **integritycopy** risk details、POCcode、Attack Path（notcompress、notsummary）
3. **validationquantityconsistency** phase filemediumnumber of entries = final reportmediumnumber of entries

### prohibitbehavior

❌ use "see appendix" or "see Phase X" alternative expressions
❌ omit low severity risk details
❌ only listriskname and ID notprovideintegrityfield
❌ relies on context memory rather than explicit file reading
❌ to POC summarize orcompress

---

## Phase 7: Mitigation Generation <ultrathink><critical thinking>

**Goal**: KB-enriched, technology-specific mitigation design with ASVS compliance verification.

**Must Use**:
- **Input**: `validated_risks` (complete Phase 6 output)
- **Security Control Set**: Control Sets + OWASP References + CWE Mitigations
- **Verification Set**: ASVS requirements for compliance verification

**Output File**: `.phase_working/P7-MITIGATION-PLANNING.md` (Optional，such asgenerate)

### For Each Risk (canparallelstartsub-agent) <ultrathink><critical thinking>

1. **Query CWE mitigations**
 ```bash
 python scripts/unified_kb_query.py --cwe CWE-XXX --mitigations
 ```

2. **Query related CVE context**
 ```bash
 python scripts/unified_kb_query.py --cve-for-cwe CWE-XXX --cve-severity CRITICAL
 ```

3. **Get STRIDE control mapping**
 ```bash
 python scripts/unified_kb_query.py --stride [Category]
 ```

4. **Query ASVS requirements for compliance**
 ```bash
 # Get ASVS requirements by level
 python scripts/unified_kb_query.py --asvs-level L2

 # Get ASVS requirements by chapter
 python scripts/unified_kb_query.py --asvs-chapter V4 # Access Control

 # Combined query for specific domain
 python scripts/unified_kb_query.py --asvs-level L2 --chapter V2 # Authentication
 ```

5. **Design technology-specific mitigation**
 - Map KB recommendations to project's tech stack
 - Ensure mitigation meets ASVS requirements at target level
 - Provide concrete implementation guidance
 - Include code examples where applicable

6. **Collect fix location information** ⚠️ NEW in v2.0.5
 For each mitigation, provide precise fix locations:
 - **Module Positioning**: Which module/package the fix applies to
 - **Function/Class Positioning**: Specific function, class, or method name
 - **File Positioning**: Exact file path in the codebase
 - **line number location**: Line number or range (e.g., L45-L60)
 - **contextcode**: Show 2 lines before/after the vulnerable code
 - **associatedfix**: List related files that need coordinated changes

 ```yaml
 fix_location:
 module: "auth"
 function: "validateToken()"
 file: "src/middleware/auth.py"
 line_range: "45-52"
 context:
 before: ["def validateToken(token):", " # Missing validation"]
 vulnerable: " return True # ← Always returns True!"
 after: ["", "def refreshToken(token):"]
 related_files:
 - file: "src/routes/api.py"
 line: 23
 change_type: "modify"
 description: "Add auth middleware to route"
 ```

7. **Define verification criteria**
 - Map to ASVS verification requirements
 - Include test cases from Phase 6 validation
 - Define acceptance criteria

7. **Estimate effort and prioritize**

### Parallel Sub-Agent Pattern <ultrathink><critical thinking>

```
Main Agent
 │
 ├──► VR-001 ──► Agent ──► CWE Mitigation + ASVS Query ──► Stack-Specific Design
 ├──► VR-002 ──► Agent ──► CVE Context + ASVS Chapter ──► Code Example
 └──► VR-003 ──► Agent ──► STRIDE Controls + ASVS Level ──► Implementation Plan
 │
 ◄───────────── Aggregate Results ──────────────
```

### Mitigation Output Template (with ASVS Compliance)

Phase 7 produces a `mitigation_plan` with ASVS compliance verification:

```yaml
mitigation_plan:
 # For each validated risk from Phase 6
 - risk_id: "VR-001"
 risk_summary: "Authentication bypass via unprotected endpoint"
 severity: critical
 cwe_refs: [CWE-287, CWE-306]

 # CWE Mitigations from KB
 kb_recommendations:
 - "Implement centralized authentication middleware"
 - "Use strong session management"
 - "Enable MFA for sensitive operations"

 # ASVS Compliance Requirements
 asvs_requirements:
 target_level: L2
 applicable_requirements:
 - id: "V2.1.1"
 description: "Verify user set passwords are at least 12 characters"
 chapter: V2-Authentication
 status: not_met
 - id: "V2.2.1"
 description: "Verify anti-automation controls are effective"
 chapter: V2-Authentication
 status: partial

 # Tiered Mitigation Actions
 immediate_actions: # Quick fixes (hours)
 - action: "Block unprotected endpoint at WAF/gateway level"
 implementation: |
 # nginx configuration
 location /api/v2/user {
 auth_request /auth/verify;
 }
 effort: 1h
 risk_reduction: 80%

 short_term_fixes: # Code changes (days)
 - action: "Add authentication middleware to all /api/v2 routes"
 control_ref: "control-set-01-authentication.md#centralized-auth"
 asvs_satisfies: ["V2.2.1", "V2.5.1"]

 # ⚠️ NEW in v2.0.5: Fix Location (precise positioning)
 fix_location:
 primary:
 module: "middleware"
 function: "require_auth()"
 file: "src/middleware/auth.py"
 line_range: "1-15" # For new file: entire file
 related:
 - file: "src/routes/api.py"
 line: 23
 change_type: "modify"
 description: "Apply @require_auth decorator"
 - file: "src/routes/user.py"
 line: 45
 change_type: "modify"
 description: "Apply @require_auth decorator"

 code_changes:
 - file: "src/middleware/auth.py"
 change_type: create
 content: |
 from functools import wraps
 def require_auth(f):
 @wraps(f)
 def decorated(*args, **kwargs):
 token = request.headers.get('Authorization')
 if not validate_token(token):
 abort(401)
 return f(*args, **kwargs)
 return decorated
 effort: 2d

 long_term_improvements: # Architectural (weeks)
 - action: "Implement centralized API gateway with built-in auth"
 asvs_satisfies: ["V2.1.1", "V2.2.1", "V2.5.1"]
 architecture_change: true
 effort: 2w

 # Verification Criteria
 verification_criteria:
 test_cases:
 - "All test cases from VR-001 validation pass"
 asvs_verification:
 - "V2.1.1: Password length >= 12 verified"
```

### Checkpoint (Phase 7)

Before proceeding to Phase 8, verify:
- [ ] Each risk has KB-enriched mitigation from `validated_risks`
- [ ] Technology-specific implementations provided with code examples
- [ ] **Fix location provided for each mitigation** (module, function, file, line) ⚠️ NEW
- [ ] **Related files listed for coordinated changes** ⚠️ NEW
- [ ] ASVS requirements mapped to each mitigation
- [ ] Effort estimated for each mitigation
- [ ] Implementation roadmap prioritized by severity and ASVS coverage
- [ ] Verification criteria defined with test cases

---

## Phase 8: Comprehensive Report <ultrathink><critical thinking>

**Goal**: Generate complete threat model report synthesizing ALL phases with full detail preservation.

**Must Use**:
- **Input**: ALL phase outputs (`findings_1` through `mitigation_plan`)
- **Knowledge**: Compliance Frameworks
- **Verification Set**: ASVS for compliance verification matrix
- **Templates**: `assets/templates/` directory (8 report templates)
- **Schemas**: `assets/schemas/` directory (data format definitions)

**⚠️ CRITICAL**: Phase 6 (`validated_risks`) outputs MUST be included in FULL DETAIL without omission or summarization.

---

### "FULL DETAIL" Definition (integritydetaileddetailsdefinition)

> **Reference**: `assets/schemas/risk-detail.schema.md` Section 5.1 Required Fields

"FULL DETAIL" means each risk must contain all following Required Fields:

| Category | Required Field | description |------|---------|------| **Core** (5) | id, name, stride_Category, element_id, element_name | Risk basic information | **Description** (2) | description.brief, description.detailed | brief description + detaileddescription | **Location** (2) | location.component, location.file | Affected component and file | **Cause** (2) | cause_analysis.root_cause, cause_analysis.related_cwe | Root cause + CWE mapping | **Attack** (3) | attack_info.attack_path, attack_info.poc_method, attack_info.exploitability | Attack Path + POC + canexploitproperty | **Impact** (4) | impact.confidentiality, impact.integrity, impact.availability, impact.cvss_score | CIA impact + CVSS score | **Mitigation** (3) | mitigation.priority, mitigation.strategy, mitigation.short_term.description | Priority level + strategy + short-term measure |

**total**: 21 individualRequired Field，100% integrityrate to qualify "FULL DETAIL"

---

### Step 8.0: Mandatory File Reading (Requiredfileread) ⚠️ NEW

**goal**: before starting any report generationbefore，must explicitly read all phase artifact files to context。

**⚠️ thisstepasRequiredstep，cannot skip**

**executeaction**:

```yaml
mandatory_file_reads:
 # ════════════════════════════════════════════════════════════════════════════
 # Priority 0: must read first (Report Core Content Source)
 # ════════════════════════════════════════════════════════════════════════════
 P0_critical:
 - file: ".phase_working/P6-RISK-VALIDATION.md"
 purpose: "risk details、POCcode、Attack Path - Most important content source"
 extract:
 - section: "risk_details[]" # all "### VR-XXX" block
 - section: "attack_paths[]" # allAttack Path
 - section: "detailed_steps[]" # all POC step
 - section: "poc_code" # all POC code block
 action: "READ_COMPLETE_FILE"

 - file: ".phase_working/P5-STRIDE-THREATS.md"
 purpose: "Threat inventory, STRIDE classification - threat identification source"
 extract:
 - section: "threat_inventory[]" # all "T-X-XX-XXX" Entry
 - section: "stride_matrix" # STRIDE distribution matrix
 action: "READ_COMPLETE_FILE"

 # ════════════════════════════════════════════════════════════════════════════
 # Priority 1: timeswantread (Report Background Content Source)
 # ════════════════════════════════════════════════════════════════════════════
 P1_important:
 - file: ".phase_working/P4-SECURITY-DESIGN-REVIEW.md"
 purpose: "Security by Designgap"
 action: "READ_COMPLETE_FILE"

 - file: ".phase_working/P3-TRUST-BOUNDARY.md"
 purpose: "trust boundarydefinition"
 action: "READ_COMPLETE_FILE"

 # ════════════════════════════════════════════════════════════════════════════
 # Priority 2: backgroundread (projectcontext)
 # ════════════════════════════════════════════════════════════════════════════
 P2_context:
 - file: ".phase_working/P2-DFD-ANALYSIS.md"
 purpose: "DFDelement、data flow"
 action: "READ_COMPLETE_FILE"

 - file: ".phase_working/P1-PROJECT-UNDERSTANDING.md"
 purpose: "projectcontext"
 action: "READ_COMPLETE_FILE"
```

**validationinspect**:
```
✅ P6-RISK-VALIDATION.md alreadyread
✅ P5-STRIDE-THREATS.md alreadyread
✅ P4-SECURITY-DESIGN-REVIEW.md alreadyread
✅ P3-TRUST-BOUNDARY.md alreadyread
✅ P2-DFD-ANALYSIS.md alreadyread
✅ P1-PROJECT-UNDERSTANDING.md alreadyread
```

**failureprocess**: if any files are missing，ABORT and prompt user to complete corresponding phase first。

**output**: all phase content loaded to context

---

### Step 8.1: Context Aggregation (contextaggregate)

**goal**: based on Step 8.0 read file，buildintegrityRisk Inventory。

**input** (from Step 8.0 read file):
- P1: project_context
- P2: dfd_elements
- P3: boundary_context
- P4: security_gaps
- P5: threat_inventory
- P6: validated_risks (core)

**executeaction**:

1. **from P6 extractRisk Inventory**:
 - identificationall `### VR-XXX:` or `### Risk:` block
 - count: `count_p6_risks = N`

2. **from P6 extractAttack Path**:
 - identificationall `### AP-XXX:` or `Attack Chain` block
 - count: `count_p6_paths = M`

3. **from P6 extract POC code**:
 - identificationall ` ```python`, ` ```bash` code block
 - count: `count_p6_pocs = K`

4. **recordcountfor subsequentvalidation**:
 ```yaml
 aggregation_counts:
 p6_risks: {N}
 p6_attack_paths: {M}
 p6_poc_blocks: {K}
 ```

**output**: `aggregated_context` contains complete risk data and counts

---

### Step 8.2: Content Source Mapping (Content Source Mapping) ⚠️ NEW

**goal**: define which phase file each section of each report must come from **COPY** (instead of summarize) content。

#### ⚠️ Traceability Preservation Rules (traceabilityretainrule) ⚠️ CRITICAL

> **Core Principle**: final reportmustretainfrom VR → Threat → Element integritytraceabilitychain

```yaml
traceability_rules:
 # ─────────────────────────────────────────────────────────────────────────
 # rule 1: VR Must Contain threat_refs
 # ─────────────────────────────────────────────────────────────────────────
 vr_threat_refs:
 requirement: "each VR mustlistits threat_refs[]"
 format: |
 ### VR-001: Plugin anycodeexecute
 **source threat**: T-T-P13-001, T-T-P13-002, T-E-P13-001 ← Must Contain
 example: | VR ID | source threat (threat_refs) | quantity |-------|------------------------|------| VR-001 | T-T-P13-001, T-T-P13-002, T-E-P13-001 | 3 |

 # ─────────────────────────────────────────────────────────────────────────
 # rule 2: retainoriginal Threat ID format
 # ─────────────────────────────────────────────────────────────────────────
 threat_id_preservation:
 requirement: "prohibit  T-{STRIDE}-{Element}-{Seq} convertfor otherformat"
 forbidden:
 - "T-E-RCE-001" # ❌ notretain ElementID
 - "RISK-001" # ❌ completely differentformat
 - "T-T-EXEC-001" # ❌ usedescriptionidentifier
 allowed:
 - "T-T-P13-001" # ✅ retain ElementID
 - "T-E-P13-001" # ✅ retain ElementID

 # ─────────────────────────────────────────────────────────────────────────
 # rule 3: quantityconsistencyproperty
 # ─────────────────────────────────────────────────────────────────────────
 count_consistency:
 check: |
 P6 VR quantity = RISK-INVENTORY VR quantity = MAIN-REPORT riskquantity
 example: "8 VR = 8 Entry = 8 risk"
```

#### RISK-INVENTORY.md contentmapping

| Section | source file | extractcontent | action |------|---------|---------|------| Risk Inventorytable | P5-STRIDE-THREATS.md | all `T-X-XX-XXX` Entry | COPY ALL | riskdetailedinformation | P6-RISK-VALIDATION.md | all `### VR-XXX:` block | COPY FULL BLOCKS | **⚠️ threat_refs list** | P6-RISK-VALIDATION.md | each VR `threat_refs[]` | **COPY VERBATIM** | POC code | P6-RISK-VALIDATION.md | all ` ```python/bash code block | COPY VERBATIM | Attack Path | P6-RISK-VALIDATION.md | all `attack_path` field | COPY ALL | canfeasibility matrix | P6-RISK-VALIDATION.md | `feasibility_assessment[]` | COPY ALL |

#### MITIGATION-MEASURES.md contentmapping

| Section | source file | extractcontent | action |------|---------|---------|------| Mitigation Measurestable | P6-RISK-VALIDATION.md | `mitigation.*` field | COPY ALL | prioritylevel matrix | P6-RISK-VALIDATION.md | `priority` field | AGGREGATE | implementcode | P6-RISK-VALIDATION.md | `mitigation.code` field | COPY VERBATIM | ASVS compliance | P6-RISK-VALIDATION.md | `asvs_requirements[]` | COPY ALL |

#### PENETRATION-TEST-PLAN.md contentmapping

| Section | source file | extractcontent | action |------|---------|---------|------| test case | P6-RISK-VALIDATION.md | `poc_method` field | TRANSFORM → TC-X-XXX | POC script | P6-RISK-VALIDATION.md | all POC code block | COPY VERBATIM | attack chain | P6-RISK-VALIDATION.md | `attack_paths[]` integrityblock | COPY ALL | ATT&CK mapping | P6-RISK-VALIDATION.md | `attack_ids[]` field | COPY ALL |

#### RISK-ASSESSMENT-REPORT.md contentmapping

| Section | source file | extractcontent | action |------|---------|---------|------| §1 executesummary | P6-RISK-VALIDATION.md | `risk_summary` | SUMMARIZE | §2 systemarchitecture | P1, P2 | projectcontext、DFD | REFERENCE | §3 trust boundary | P3 | boundarydefinition | REFERENCE | §4 Security by Design | P4 | securitygap matrix | COPY | §5 threatanalysis | P5, P6 | Threat inventory、validationresult | COPY ALL | §6 risk details | P6-RISK-VALIDATION.md | **all VR-XXX integrityblock** | **COPY FULL BLOCKS** | §7 Attack Path | P6-RISK-VALIDATION.md | **allattack chaindiagramframe** | **COPY VERBATIM** | §8 mitigationrecommend | P6-RISK-VALIDATION.md | `mitigation` field | COPY ALL | §9 compliancemapping | P6 | ASVS matrix | COPY |

**⚠️ COPY ruledefinition**:
- `COPY ALL`: integritycopyallmatchitem，notcompressnotsummary
- `COPY FULL BLOCKS`: copyintegrity markdown block（includesubtitles andallcontent）
- `COPY VERBATIM`: word-for-word copy，including code format、comment、blank line
- `TRANSFORM`: format conversion but preserve all information
- `SUMMARIZE`: onlyforexecutesummarySection
- `REFERENCE`: quote and appropriately reorganize

---

### Step 8.3: Report Section Generation (Sectiongenerate)

**input**: `aggregated_context` (from Step 8.1)

**generate each report by templateSection** (comply Step 8.2 contentmapping):

1. Executive Summary (executesummary)
2. Architecture Overview (architectureoverview) - from P1/P2
3. Security Design Assessment (Security by Designassessment) - from P4
4. STRIDE Threat Analysis (threatanalysis) - COPY ALL from P5
5. Risk Details (risk details) - **COPY FULL BLOCKS from P6**
6. Attack Path Analysis (Attack Pathanalysis) - **COPY VERBATIM from P6**
7. Mitigation Recommendations (mitigationrecommend) - COPY ALL from P6
8. Compliance Mapping (compliancemapping)
9. Appendices (appendix)

**output**: `report_sections{}`

---

### Step 8.4: Content Completeness Verification (contentintegritypropertyvalidation) ⚠️ NEW

**goal**: ensure final report contains all risk entries from phase files，noneomission。

#### 8.4.1 Count Conservation Verification (count conservation validation) ⚠️ CRITICAL

> **Source of Truth**: P6 `threat_disposition` yesquantityvalidationauthoritative sourcesource

```yaml
count_conservation_chain:
 # ═════════════════════════════════════════════════════════════════════════
 # phase 1: P5 → P6 threatprocessvalidation
 # ═════════════════════════════════════════════════════════════════════════
 p5_to_p6:
 input: "P5.threat_inventory.summary.total" # example: 120
 output_breakdown:
 consolidated: "sum(P6.VR.threat_refs.length)" # example: 98
 excluded: "len(P6.threat_disposition.excluded_threats)" # example: 22
 formula: "consolidated + excluded = input"
 example: "98 + 22 = 120 ✅"

 # ═════════════════════════════════════════════════════════════════════════
 # phase 2: P6 → final report VR passvalidation
 # ═════════════════════════════════════════════════════════════════════════
 p6_to_reports:
 source: "len(P6.validated_risks)" # example: 8 VR
 targets:
 risk_inventory: "COUNT(RISK-INVENTORY:VR-XXX)"
 main_report: "COUNT(MAIN-REPORT:risk details)"
 pentest_plan: "COUNT(PENTEST-PLAN:test_cases)"
 formula: "source = risk_inventory = main_report"
 example: "8 = 8 = 8 ✅"

 # ═════════════════════════════════════════════════════════════════════════
 # phase 3: threat_refs traceabilityvalidation
 # ═════════════════════════════════════════════════════════════════════════
 traceability_verification:
 check: |
 FOR each VR in RISK-INVENTORY:
 VR.threat_refs MUST NOT be empty
 VR.threat_refs values MUST match P5 threat IDs (T-{STRIDE}-{Element}-{Seq})
 on_fail: "ABORT - traceability chain break"
```

#### 8.4.2 Report Count Verification Matrix (reportquantityvalidationmatrix)

| Check Item | Source Count | Target Count | Validation Method | Must Equal |--------|---------|---------|---------|---------| VR Total | COUNT(P6:VR-XXX) | COUNT(RISK-INVENTORY:risks) | Count Comparison | ✅ yes | Threat Refs Total | sum(P6:VR.threat_refs) | P6.threat_disposition.consolidated | Count Comparison | ✅ yes | POC Code Block | COUNT(P6:```code) | COUNT(PENTEST-PLAN:poc_scripts) | Count Comparison | ✅ yes | Attack Path | COUNT(P6:AP-XXX) | COUNT(PENTEST-PLAN:attack_chains) | Count Comparison | ✅ yes | Mitigation Measures | COUNT(P6:mitigation) | COUNT(MITIGATION:measures) | Count Comparison | ✅ yes |

**validationexecute**:

```yaml
verification_checks:
 - check: "threatquantityconservation"
 priority: "CRITICAL"
 source: "P6.threat_disposition"
 validation: |
 consolidated_into_vr + excluded_with_reason = P5.total
 on_fail: "ABORT - threatloss，nonemethodgeneratereport"

 - check: "VR quantityconsistency"
 source: "P6-RISK-VALIDATION.md"
 source_pattern: "### VR-" or "### Risk:"
 target: "RISK-INVENTORY.md"
 target_section: "riskdetailedinformation"
 action: |
 IF COUNT(source) != COUNT(target):
 1. identificationmissing VR-ID
 2. RE-READ P6 file
 3. supplement missing risk blocks to target report
 4. restartvalidationdirecttopass

 - check: "threat_refs integrity"
 source: "P6.VR.threat_refs[]"
 target: "RISK-INVENTORY.VR.threat_refs[]"
 validation: |
 each VR musthavenon-empty threat_refs[]
 threat_refs formatmustas T-{STRIDE}-{Element}-{Seq}
 on_fail: "ABORT - traceability chain break"

 - check: "POCcodenoneomission"
 source: "P6-RISK-VALIDATION.md"
 source_pattern: "```python" or "```bash"
 target: "PENETRATION-TEST-PLAN.md"
 target_section: "POC script"
 action: |
 IF COUNT(source) != COUNT(target):
 1. identificationmissing code blocks
 2. COPY VERBATIM togoalreport
 3. restartvalidation

 - check: "attack chainintegrity"
 source: "P6-RISK-VALIDATION.md"
 source_pattern: "### AP-" or "Attack Chain"
 target: "PENETRATION-TEST-PLAN.md"
 target_section: "attack chainanalysis"
 action: |
 IF COUNT(source) != COUNT(target):
 1. identificationmissing attack chain
 2. COPY FULL BLOCKS togoalreport
 3. restartvalidation
```

**validationpassstandard**:
- [ ] threatconservation: P5.total = consolidated + excluded ✅
- [ ] VR quantity: P6 = RISK-INVENTORY ✅
- [ ] threat_refs: All VR have non-empty threat_refs ✅
- [ ] POCquantity: P6 = PENTEST-PLAN ✅
- [ ] Attack Path: P6 = PENTEST-PLAN ✅
- [ ] Mitigation Measures: P6 = MITIGATION ✅

**validationfailureprocess**:
```
IF threatconservationfailure:
 ABORT - data loss，needinspect P6 threat_disposition

IF threat_refs missing:
 ABORT - traceability chain break，needfix VR structure

IF Other check failures:
 1. Output detailed difference report
 2. RE-READ source phase file
 3. supplement missing content
 4. duplicatevalidationdirecttoallpass
```

**output**: validation pass confirmation + Difference fix（if needed）

---

### Step 8.5: Report Assembly (Report Assembly)

**outputdirectory**: `{PROJECT_ROOT}/Risk_Assessment_Report/`

```
{PROJECT_ROOT}/
└── Risk_Assessment_Report/ ← finalreport directory
 ├── {PROJECT}-RISK-ASSESSMENT-REPORT.md
 ├── {PROJECT}-RISK-INVENTORY.md
 ├── {PROJECT}-MITIGATION-MEASURES.md
 ├── {PROJECT}-PENETRATION-TEST-PLAN.md
 └── ... (Optionalreport)
```

**8 standard report definition**:

| # | Report Type | Filename | Template File | Main Source | Required |---|----------|--------|----------|---------|------| 1 | Main Report | `{PROJECT}-RISK-ASSESSMENT-REPORT.md` | `assets/templates/RISK-ASSESSMENT-REPORT.template.md` | P1-P8 all | ✅ | 2 | Risk Inventory | `{PROJECT}-RISK-INVENTORY.md` | `assets/templates/RISK-INVENTORY.template.md` | P5, P6 | ✅ | 3 | Mitigation Measures | `{PROJECT}-MITIGATION-MEASURES.md` | `assets/templates/MITIGATION-MEASURES.template.md` | P6, P7 | ✅ | 4 | Penetration Test Plan | `{PROJECT}-PENETRATION-TEST-PLAN.md` | `assets/templates/PENETRATION-TEST-PLAN.template.md` | P6 | ✅ | 5 | Architecture Analysis | `{PROJECT}-ARCHITECTURE-ANALYSIS.md` | `assets/templates/ARCHITECTURE-ANALYSIS.template.md` | P1, P2 | Optional | 6 | DFD Diagram | `{PROJECT}-DFD-DIAGRAM.md` | `assets/templates/DFD-DIAGRAM.template.md` | P2, P3 | Optional | 7 | Compliance Report | `{PROJECT}-COMPLIANCE-REPORT.md` | `assets/templates/COMPLIANCE-REPORT.template.md` | P5, P7 | Optional | 8 | Attack Validation | `{PROJECT}-ATTACK-PATH-VALIDATION.md` | `assets/templates/ATTACK-PATH-VALIDATION.template.md` | P6 | Optional |

**report generationstep**:

1. **createoutputdirectory**: `mkdir -p {PROJECT_ROOT}/Risk_Assessment_Report/`
2. **exact PROJECT name**: from project_context extract，convertcapitalize
3. **generate each report by template**: quote `assets/templates/` directorycorrespondencetemplate
4. **validationfile naming**: ensureconform `{PROJECT}-{REPORT_TYPE}.md` format

**output**: standardizationreportfilecollect

---

### Step 8.6: Quality Validation (qualityvalidation)

**qualityinspectInventory (Content Quality)**:
- [ ] all risks have complete 5 elements (description、location、cause、attack、mitigation)
- [ ] Risk Inventory corresponds one-to-one with detail blocks
- [ ] statisticsdata accuracy (total、eachlevelquantity、percentage)
- [ ] reportformatconformtemplatestructure
- [ ] all charts correctly rendered (ASCII, Mermaid)
- [ ] ASVScompliancematrixintegrity

**namingvalidationinspectInventory (Naming Validation)**:

```
✅ PROJECT Namevalidation:
 • format: uppercase letters + numbers + hyphens
 • regex: ^[A-Z][A-Z0-9-]{0,29}$
 • example: N8N, OPEN-WEBUI, MY-PROJECT-V2

✅ Filenameformatvalidation:
 • format: {PROJECT}-{REPORT_TYPE}.md
 • REPORT_TYPE must be one of 8 standard types of

✅ outputlocationvalidation:
 • mustlocated: {PROJECT_ROOT}/Risk_Assessment_Report/
```

**validationscript** (Optionalexecute):
```bash
# validationreportnaming and location
ls -la Risk_Assessment_Report/*.md

# inspectFilenameformat
for f in Risk_Assessment_Report/*.md; do
 if [[ ! $(basename "$f") =~ ^[A-Z][A-Z0-9-]+-[A-Z-]+\.md$ ]]; then
 echo "INVALID: $f"
 fi
done
```

**output**: validationpass final reportcollect

---

### Step 8.7: Penetration Test Plan Generation (penetration testsolutiongenerate) <ultrathink><critical thinking>

**goal**: based on P6 riskvalidationdata，generatedetailed penetration testsolution。

**input**:
- P6: `validated_risks[]` - Validated Risk Inventory
- P6: `attack_paths[]` - Attack Path
- P6: `poc_methods[]` - POC Validation Method

**Requiredoutput**: `{PROJECT}-PENETRATION-TEST-PLAN.md`

**template**: `assets/templates/PENETRATION-TEST-PLAN.template.md`

#### test casegeneraterule

**from P6 risk validation data maps to test cases**:

| P6 field | test casefield | mappingrule |---------|-------------|---------| `risk.id` | `test_case.id` | TC-{STRIDE}-{SEQ} (e.g., TC-S-001) | `risk.name` | `test_case.objective` | directmapping | `risk.attack_path` | `test_case.steps` | convertastestingstep | `risk.poc_method` | `test_case.poc_script` | **directcontain (COPY VERBATIM)** | `risk.attack_info.attack_techniques` | `test_case.mitre_attack` | ATT&CK ID mapping | `risk.priority` | `test_case.priority` | P0→Critical, P1→High, P2→Medium | `risk.location` | `test_case.target` | goalcomponent/file |

#### ATT&CK technicalmapping

| STRIDE | ATT&CK Tactic | Common Techniques |--------|--------------|-----------------| **S**poofing | Initial Access | T1078 (Valid Accounts), T1566 (Phishing) | **T**ampering | Impact | T1485 (Data Destruction), T1565 (Data Manipulation) | **R**epudiation | Defense Evasion | T1070 (Indicator Removal), T1036 (Masquerading) | **I**nfo Disclosure | Collection | T1005 (Data from Local System), T1039 (Data from Network) | **D**enial of Service | Impact | T1499 (Endpoint DoS), T1498 (Network DoS) | **E**levation | Privilege Escalation | T1068 (Exploitation), T1548 (Abuse Elevation) |

**output**: `{PROJECT}-PENETRATION-TEST-PLAN.md`

---

### Step 8.8: Phase Output Publication (phase artifactrelease) <ultrathink><critical thinking>

**goal**: copy phase working documents from hidden directory to report directory，asintegrityone of the deliverablespartial。

**⚠️ thisstepasRequiredstep，cannot skip**

**executeprerequisite**: execute after all report generation completes

#### filereleaseInventory

```yaml
source_directory: "{PROJECT_ROOT}/Risk_Assessment_Report/.phase_working/"
target_directory: "{PROJECT_ROOT}/Risk_Assessment_Report/"

files_to_publish: # retainEnglish Filename
 - P1-PROJECT-UNDERSTANDING.md # Phase 1: Project Understanding
 - P2-DFD-ANALYSIS.md # Phase 2: DFDanalysis
 - P3-TRUST-BOUNDARY.md # Phase 3: trust boundary
 - P4-SECURITY-DESIGN-REVIEW.md # Phase 4: Security by Designassessment
 - P5-STRIDE-THREATS.md # Phase 5: STRIDEthreatanalysis
 - P6-RISK-VALIDATION.md # Phase 6: riskvalidation
```

#### executecommand

```bash
# copy phase process documents to report directory (retainEnglishname)
cd {PROJECT_ROOT}/Risk_Assessment_Report/

cp .phase_working/P1-PROJECT-UNDERSTANDING.md ./
cp .phase_working/P2-DFD-ANALYSIS.md ./
cp .phase_working/P3-TRUST-BOUNDARY.md ./
cp .phase_working/P4-SECURITY-DESIGN-REVIEW.md ./
cp .phase_working/P5-STRIDE-THREATS.md ./
cp .phase_working/P6-RISK-VALIDATION.md ./
```

#### finaldirectory structure

```
{PROJECT_ROOT}/Risk_Assessment_Report/
├── {PROJECT}-RISK-ASSESSMENT-REPORT.md ← Requiredreport (Main Report)
├── {PROJECT}-RISK-INVENTORY.md ← Requiredreport
├── {PROJECT}-MITIGATION-MEASURES.md ← Requiredreport
├── {PROJECT}-PENETRATION-TEST-PLAN.md ← Requiredreport
├── P1-PROJECT-UNDERSTANDING.md ← phasedocument
├── P2-DFD-ANALYSIS.md ← phasedocument
├── P3-TRUST-BOUNDARY.md ← phasedocument
├── P4-SECURITY-DESIGN-REVIEW.md ← phasedocument
├── P5-STRIDE-THREATS.md ← phasedocument
├── P6-RISK-VALIDATION.md ← phasedocument
└── .phase_working/ ← working directory (retain)
 └── ...
```

**Value Description**:
- phase process document records complete analysis derivation process
- supportaudittraceability andqualityvalidation
- facilitate team understanding of threat modeling logic chain
- preserve English name to ensure consistency with naming conventions

**output**: complete phase document set published to report directory

---

## Output Files

**outputdirectory**: `{PROJECT_ROOT}/Risk_Assessment_Report/`

**Main Report**: `{PROJECT}-RISK-ASSESSMENT-REPORT.md` - integritythreatmodelreport

**Requiredreportcollect** (alwaysgenerate):
| Number | Filename | description |------|--------|------| 1 | `{PROJECT}-RISK-ASSESSMENT-REPORT.md` | risk assessmentreport (Main Report) | 2 | `{PROJECT}-RISK-INVENTORY.md` | Risk Inventory | 3 | `{PROJECT}-MITIGATION-MEASURES.md` | Mitigation Measures | 4 | `{PROJECT}-PENETRATION-TEST-PLAN.md` | penetration testsolution |

**Optionalreportcollect** (on-demandgenerate):
| Number | Filename | description |------|--------|------| 5 | `{PROJECT}-ARCHITECTURE-ANALYSIS.md` | Architecture Analysis | 6 | `{PROJECT}-DFD-DIAGRAM.md` | DFD Diagram | 7 | `{PROJECT}-COMPLIANCE-REPORT.md` | Compliance Report | 8 | `{PROJECT}-ATTACK-PATH-VALIDATION.md` | Attack Pathvalidation |

**phaseprocess document** (automaticrelease):
| Number | Filename | description |------|--------|------| - | `P1-PROJECT-UNDERSTANDING.md` | Phase 1 Project Understanding | - | `P2-DFD-ANALYSIS.md` | Phase 2 DFDanalysis | - | `P3-TRUST-BOUNDARY.md` | Phase 3 trust boundary | - | `P4-SECURITY-DESIGN-REVIEW.md` | Phase 4 Security by Designassessment | - | `P5-STRIDE-THREATS.md` | Phase 5 STRIDEthreatanalysis | - | `P6-RISK-VALIDATION.md` | Phase 6 riskvalidation |

**templatelocation**: `assets/templates/` directorycontain 8 individualreportTemplate File

**Schema location**: `assets/schemas/` directorycontaindataformatdefinition

---

## Final Checkpoint

Before completing Phase 8, verify:
- [ ] Step 8.0: all phase files explicitly read
- [ ] Step 8.1: riskcountalreadyrecord (P6_risks = N)
- [ ] Step 8.2: Content Source Mappingalreadycomply
- [ ] Step 8.3: reportSectionalreadygenerate
- [ ] Step 8.4: contentintegritypropertyvalidationalreadypass (P6 = final report)
- [ ] Step 8.5: 4 Required reportsalreadygenerate
- [ ] Step 8.6: qualityvalidationalreadypass
- [ ] Step 8.7: penetration testsolutionalreadygenerate (containallPOC)
- [ ] Step 8.8: phase artifactalreadyrelease

**Reflection**: Review all reports for completeness. Ensure NO risk details, POC code, or Attack Paths were omitted or summarized.
