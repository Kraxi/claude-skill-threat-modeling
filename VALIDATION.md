<!-- Code-First Deep Threat Modeling Workflow | Version 2.1.1 | https://github.com/fr33d3m0n/skill-threat-modeling | License: BSD-3-Clause | Welcome to cite but please retain all sources and declarations -->

# VALIDATION.md - Phase 6: Risk Validation (riskvalidation)

> **Version**: 2.1.0
> **Scope**: riskvalidation、Attack Pathanalysis、POCdesign
> **Input**: P1-P5 allphase artifact
> **Output**: `validated_risks` integrityRisk Inventory + POCvalidation

---

## Phase 6: Risk Validation <ultrathink><critical thinking>

**Goal**: Comprehensive risk validation with Attack Path verification, POC design, and Verification Set integration.

**Must Use**:
- **ALL previous findings**: `findings_1` + `findings_2` + `findings_3` + `findings_4` + `findings_5`
- **Threat Pattern Set**: CAPEC → ATT&CK → CVE/KEV
- **Verification Set**: WSTG/MASTG test procedures

**Output File**: `.phase_working/P6-RISK-VALIDATION.md`

---

## Consolidation Process (merge algorithm)

Phase 6 primary task is to P1-P5 merge all security findings into unified Risk Inventory，avoid duplication and keep traceability。

### Step 6.1: Collect all findings

**⚠️ MANDATORY FILE READS** (mustexecute fileread):

```yaml
input_sources:
 - source: ".phase_working/P1-PROJECT-UNDERSTANDING.md"
 extract_section: "preliminarysecurityobserve" | "Initial Security Observations"
 id_prefix: SF-P1
 expected_fields: [description, component, severity_hint]

 - source: ".phase_working/P2-DFD-ANALYSIS.md"
 extract_section: "data flowrisk" | "Data Flow Risks"
 id_prefix: SF-P2
 expected_fields: [description, element_id, data_sensitivity]

 - source: ".phase_working/P3-TRUST-BOUNDARY.md"
 extract_section: "boundaryrisk" | "Boundary Risks"
 id_prefix: SF-P3
 expected_fields: [description, boundary_id, crossing_type]

 - source: ".phase_working/P4-SECURITY-DESIGN-REVIEW.md"
 extract_section: "securitygap" | "Security Gaps"
 id_prefix: SF-P4
 expected_fields: [domain, gap_description, current_status, risk_level]

 - source: ".phase_working/P5-STRIDE-THREATS.md"
 extract_section: "Threat inventory" | "Threat Inventory"
 id_prefix: T-{STRIDE}
 expected_fields: [id, stride_Category, element_id, description, cwe, priority]
```

### Step 6.2: standardstandardize to unifiedformat

 allfindingconvertas `normalized_finding` mediumbetweenformat：

```yaml
normalized_finding:
 # === identification information ===
 original_id: "SF-P3-001" # originalID (Required)
 source_phase: "P3" # source phase (Required)

 # === matchkey (for deduplication) ===
 related_cwe: "CWE-287" # mainmatchkey1 (Required，if missing, infer or markUNKNOWN)
 location_file: "src/api/routes.py" # mainmatchkey2 (Required，standardconvert to relativepath)
 location_component: "api" # assistedmatchkey (modulelevel)

 # === classificationinformation ===
 stride_Category: "S" # STRIDEclassification (ifyesSFneedinfer)

 # === severity ===
 severity: "high" # unifiedas: critical/high/medium/low
 cvss_estimate: 7.5 # CVSSestimate (such ashave)

 # === descriptioninformation ===
 description_brief: "authenticationmissing..." # brief description (for backupmatch)
 description_full: "..." # integritydescription

# === CWE inferrule (when originalfindinglackCWEtime) ===
cwe_inference:
 rules:
 - pattern: "authentication|authentication|login|credential"
 infer_cwe: "CWE-287"
 - pattern: "authorization|authorization|access control|permission"
 infer_cwe: "CWE-863"
 - pattern: "injection|injection|SQL|XSS|command"
 infer_cwe: "CWE-74"
 - pattern: "encrypt|encryption|crypto|secret|key"
 infer_cwe: "CWE-327"
 - pattern: "log|logging|audit|repudiation"
 infer_cwe: "CWE-778"
 fallback: "CWE-UNKNOWN" # nonemark when cannot infer

# === pathstandardizationrule ===
path_normalization:
 - remove_prefix: ["/home/", "~/", "./"]
 - use_forward_slash: true
 - lowercase: false # retaincase
 - remove_trailing_slash: true
```

### Step 6.3: deduplicationmatchrule

```yaml
deduplication_rules:
 # ─────────────────────────────────────────────────────────────────────────
 # rule1: exact match (different aspects of same issueperspective)
 # ─────────────────────────────────────────────────────────────────────────
 exact_match:
 name: "MERGE - exact match"
 criteria:
 - related_cwe: EQUAL
 - location_file: EQUAL
 action: MERGE

 merge_strategy:
 # IDprocess: generatenewVR-ID，retainalloriginalID
 new_id_format: "VR-{SEQ:03d}"
 original_ids: COLLECT_ALL # → original_refs: ["SF-P3-001", "T-S-P01-001"]

 # severity: take highest
 severity: MAX
 cvss: MAX

 # description: mergeand annotate sourcesource
 description: |
 [Consolidated from {count} findings]

 Source P{X}: {description_1}
 Source P{Y}: {description_2}
 ...

 # CWE: priority use P5 CWE (most authoritative)
 cwe_priority: [P5, P4, P3, P2, P1]

 # STRIDE: priorityuseP5classification
 stride_priority: [P5, P4, P3, P2, P1]

 # ─────────────────────────────────────────────────────────────────────────
 # rule2: componentlevelmatch (relatedbut different issues)
 # ─────────────────────────────────────────────────────────────────────────
 component_match:
 name: "LINK - componentlevelmatch"
 criteria:
 - related_cwe: EQUAL
 - location_component: EQUAL
 - location_file: NOT_EQUAL
 action: LINK

 link_strategy:
 # keepasindependentrisk
 keep_separate: true

 # establish associated relationship
 add_field: "related_risks"
 link_type: "same_cwe_same_component"

 # each keeps independent VR-ID
 each_gets_vr_id: true

 # ─────────────────────────────────────────────────────────────────────────
 # rule3: descriptionsimilaritymatch (backuprule - whenCWEor when files are missing)
 # ─────────────────────────────────────────────────────────────────────────
 description_similarity:
 name: "LINK - description similar"
 trigger: "when related_cwe = 'CWE-UNKNOWN' or location_file empty"
 criteria:
 - description_similarity: >= 0.85 # use edit distance or semantic similarity
 - location_component: EQUAL # at leastcomponentsame
 action: LINK # notautomaticMERGE，needmanualconfirm

 link_strategy:
 keep_separate: true
 add_field: "possibly_related"
 requires_review: true # markneedmanualconfirm
 similarity_score: RECORD # recordsimilarity score

 # ─────────────────────────────────────────────────────────────────────────
 # rule4: nonematch
 # ─────────────────────────────────────────────────────────────────────────
 no_match:
 name: "KEEP - independentrisk"
 action: KEEP_AS_IS

 strategy:
 assign_vr_id: true
 mark_as: "standalone"
```

### Step 6.4: severityunifiedmapping

```yaml
severity_normalization:
 # inputformatunifiedization
 input_mapping:
 critical: critical
 high: high
 medium: medium
 low: low
 # variant mapping
 severity: critical
 high: high
 medium: medium
 low: low
 P0: critical
 P1: high
 P2: medium
 P3: low
 "9.0-10.0": critical
 "7.0-8.9": high
 "4.0-6.9": medium
 "0.1-3.9": low

 # MAX policyrealize
 severity_order: [critical, high, medium, low] # smaller index meansseverity
 max_logic: "take minimum severity_order index value"
```

### Step 6.5: generatevalidationriskID

```yaml
validated_risk_id_schema:
 format: "VR-{SEQ:03d}"
 examples: ["VR-001", "VR-002", "VR-003"]

 sequence_rules:
 start_from: 1
 increment: 1
 ordering: "by severity descending order (critical → low)"

 # ═════════════════════════════════════════════════════════════════════════
 # ⚠️ CRITICAL: threat_refs[] yesRequired Field！
 # ═════════════════════════════════════════════════════════════════════════
 Required_fields:
 - vr_id: "VR-001"
 - threat_refs: ["T-T-P13-001", "T-T-P13-002"] # ⚠️ MANDATORY: threat sourcelisttable
 - finding_refs: ["F-P4-003"] # OPTIONAL: P1-P4 finding source
 - merge_type: "exact_match" | "component_match" | "standalone"
 - merged_count: 2 # mergeseveralthreat
 - canonical_cwe: "CWE-287" # authoritativeCWE
 - canonical_file: "src/api/routes.py" # representative files
 - stride_Category: "S"
 - severity: "high"
 - cvss_score: 7.5

# outputexample (conformnew datamodel)
validated_risk_example:
 vr_id: "VR-001"
 threat_refs: ["T-T-P13-001", "T-T-P13-002", "T-E-P13-001"] # ⚠️ critical: trace to originalthreat
 finding_refs: ["F-P4-003"]
 merge_type: "exact_match"
 merged_count: 3
 canonical_cwe: "CWE-94"
 canonical_file: "utils/plugin.py"
 stride_categories: ["T", "E"] # mergeaftercancontainmultiple STRIDE type
 severity: "critical"
 cvss_score: 10.0
 description_brief: "Plugin anycodeexecute"
 description_full: |
 [Consolidated from 3 threats]

 Source T-T-P13-001: Plugin codeinjection (Tampering)
 Source T-T-P13-002: pip supplychainattack (Tampering)
 Source T-E-P13-001: privilege escalation to server control (Elevation)
```

### Step 6.5.1: Threat Disposition Tracking (threatprocesstracking) ⚠️ NEW

> **Purpose**: ensureeach P5 threatall explicitlyprocess，supportcount conservation validation

```yaml
# ═════════════════════════════════════════════════════════════════════════
# threatprocesstrackingtable - mustat P6 outputmediumcontain
# ═════════════════════════════════════════════════════════════════════════
threat_disposition:
 # ─────────────────────────────────────────────────────────────────────────
 # statisticssummary
 # ─────────────────────────────────────────────────────────────────────────
 input_count: 120 # from P5 threat total
 output_summary:
 consolidated_into_vr: 98 # mergeto VR threatcount
 excluded_with_reason: 22 # exclude threatcount (needgiveexitreason)
 validation_formula: "98 + 22 = 120 ✅"

 # ─────────────────────────────────────────────────────────────────────────
 # group threat source by VR
 # ─────────────────────────────────────────────────────────────────────────
 vr_threat_mapping:
 VR-001:
 threat_refs: ["T-T-P13-001", "T-T-P13-002", "T-E-P13-001"]
 count: 3
 VR-002:
 threat_refs: ["T-S-P01-001", "T-S-P01-002"]
 count: 2
 VR-003:
 threat_refs: ["T-I-DS01-001"]
 count: 1
 # ... each VR must list its threat_refs

 # ─────────────────────────────────────────────────────────────────────────
 # exclude threat (mustdescriptionreason)
 # ─────────────────────────────────────────────────────────────────────────
 excluded_threats:
 - threat_id: "T-D-P07-001"
 reason: "MITIGATED - alreadyhave rate limiting control (routers/auths.py:45)"
 status: "mitigated"
 - threat_id: "T-S-P02-002"
 reason: "MITIGATED - escape_filter_chars() alreadyapply (auths.py:298)"
 status: "mitigated"
 - threat_id: "T-I-DF15-003"
 reason: "LOW_RISK - theoretically feasible but requires physical access"
 status: "excluded_low_risk"

 # ─────────────────────────────────────────────────────────────────────────
 # validationrule
 # ─────────────────────────────────────────────────────────────────────────
 validation_rules:
 - rule: "each P5 threatmustappears in some VR.threat_refs or excluded_threats medium"
 formula: "sum(vr_threat_mapping.*.count) + len(excluded_threats) = input_count"
 - rule: "excluded_threats musthave reason field"
 formula: "all(excluded_threats.*.reason != null)"
```

**P6 outputMust Contain**:
1. `threat_disposition.input_count` - from P5 threat total
2. `threat_disposition.vr_threat_mapping` - each VR threat source
3. `threat_disposition.excluded_threats` - exclude threatandreason
4. `threat_disposition.validation_formula` - count conservation validation

### Step 6.6: integritypropertyvalidation

```yaml
completeness_verification:
 # ═════════════════════════════════════════════════════════════════════════
 # 1. P5 threatprocessvalidation (corevalidation) ⚠️ CRITICAL
 # ═════════════════════════════════════════════════════════════════════════
 p5_threat_verification:
 input_count: "P5.threat_inventory.summary.total" # example: 120
 output_breakdown:
 consolidated_into_vr: "sum(all VR.threat_refs.length)" # example: 98
 excluded_with_reason: "len(threat_disposition.excluded_threats)" # example: 22

 conservation_formula: |
 consolidated_into_vr + excluded_with_reason = input_count
 example: 98 + 22 = 120 ✅

 validation_rules:
 - name: "threatquantityconservation"
 formula: "consolidated + excluded = P5_total"
 on_fail: "ABORT - threatloss，inspect threat_disposition"

 - name: "each threat has attribution"
 check: |
 FOR each threat T in P5.threat_inventory.threats:
 T.id MUST appear in:
 - some VR.threat_refs[], OR
 - threat_disposition.excluded_threats[]
 on_fail: "ABORT - threat {T.id} notbyprocess"

 - name: "excludethreathavereason"
 formula: "all(excluded_threats.*.reason != null)"
 on_fail: "WARNING - exclude threatlackreason"

 # ═════════════════════════════════════════════════════════════════════════
 # 2. VR structurevalidation
 # ═════════════════════════════════════════════════════════════════════════
 vr_structure_verification:
 checks:
 - name: "allVRhavethreat_refs"
 formula: "all(VR.threat_refs.length > 0)"
 on_fail: "ABORT - VR musthave threat_refs[]"

 - name: "noneduplicateVR-ID"
 formula: "count(unique(vr_ids)) == len(vr_ids)"
 on_fail: "ABORT - VR-ID conflict"

 - name: "allVRhaveCWE"
 formula: "all(VR.canonical_cwe != null)"
 on_fail: "WARNING - partialVRlackCWE"

 # ═════════════════════════════════════════════════════════════════════════
 # 3. outputsummary (Must Containat P6 reportmedium)
 # ═════════════════════════════════════════════════════════════════════════
 consolidation_summary:
 template: |
 ## Phase 6 threatprocesssummary

 ### P5 input
 | Source | Count |------|------| P5 threat total | **{P5_total}** |

 ### processresult
 | processtype | quantity | percentage |---------|------|--------| mergeto VR | {consolidated} | {consolidated/P5_total*100}% | exclude (alreadymitigation/lowrisk) | {excluded} | {excluded/P5_total*100}% | **total** | **{P5_total}** | 100% |

 ### VR generatestatistics
 | indicator | value |------|-----| generate VR quantity | {VR_count} | average per VR mergethreatcount | {consolidated/VR_count} |

 ### ⚠️ count conservation validation
 ```
 P5 threat total: {P5_total}
 = mergeto VR ({consolidated}) + exclude ({excluded})
 = {consolidated} + {excluded}
 ✅ validationpass
 ```

 ### threat_refs traceability example
 | VR ID | source threat | quantity |-------|---------|------| VR-001 | T-T-P13-001, T-T-P13-002, T-E-P13-001 | 3 | VR-002 | T-S-P01-001, T-S-P01-002 | 2 | ... | ... | ... |
```

**⚠️ validationfailureprocess**:
- quantityconservationfailure → immediately ABORT，inspect threat_disposition
- threat_refs missing → immediately ABORT，VR nonemethodtraceability
- excludereasonmissing → WARNING，continuebutmark

---

## For Each Risk (canparallelstartsub-agent) <ultrathink><critical thinking>

1. **Query CAPEC attack patterns**
 ```bash
 python scripts/unified_kb_query.py --capec CAPEC-XXX --attack-chain
 ```

2. **Query ATT&CK techniques**
 ```bash
 python scripts/unified_kb_query.py --attack-technique TXXX
 ```

3. **Check for known exploited vulnerabilities**
 ```bash
 python scripts/unified_kb_query.py --check-kev CVE-XXXX
 python scripts/unified_kb_query.py --cve-for-cwe CWE-XXX
 ```

4. **Query Verification Set for test procedures** (NEW in v2.0)
 ```bash
 # Get STRIDE-specific tests
 python scripts/unified_kb_query.py --stride-tests S

 # Get CWE-specific tests
 python scripts/unified_kb_query.py --cwe-tests CWE-89

 # Get WSTG Category tests
 python scripts/unified_kb_query.py --wstg-Category ATHN
 ```

5. **Construct Attack Path**
 - Entry point → Step 1 → Step 2 → ... → Impact
 - Identify prerequisites and conditions
 - Include ATT&CK technique references

6. **Design POC verification approach**
 - Generate test cases from `verification_procedure` table
 - Manual testing steps with commands
 - Automated testing with expected results
 - Tools Required

### Parallel Sub-Agent Pattern <ultrathink><critical thinking>

```
Main Agent
 │
 ├──► T-S-P1-001 ──► Agent ──► CAPEC Query + ATT&CK Query + STRIDE Tests ──► Attack Path
 ├──► T-T-DF1-001 ──► Agent ──► CAPEC Query + KEV Check + CWE Tests ──► POC Design
 └──► T-E-P3-001 ──► Agent ──► CAPEC Query + CVE Search + WSTG Tests ──► Verification
 │
 ◄───────────── Aggregate Results ──────────────
```

---

## Risk Validation Output Template (5-Part Structure)

Phase 6 produces a comprehensive `validated_risks` output with 5 distinct parts:

```yaml
validated_risks:
 # ─────────────────────────────────────────────────────────────────
 # Part 1: Risk Summary (Assessment Overview)
 # ─────────────────────────────────────────────────────────────────
 risk_summary:
 total_identified: 45
 validated_high: 12
 validated_medium: 18
 validated_low: 10
 dismissed: 5
 risk_by_stride: {S: 8, T: 12, R: 3, I: 10, D: 4, E: 8}
 risk_by_domain: {AUTHN: 10, INPUT: 15, AUTHZ: 8, API: 6, CRYPTO: 4, DATA: 2}

 # ─────────────────────────────────────────────────────────────────
 # Part 2: Detailed Risk Analysis (Per-Item Analysis)
 # ─────────────────────────────────────────────────────────────────
 risk_details:
 - risk_id: "VR-001"
 original_refs: ["T-S-P1-001", "SD-001", "DFD-002"] # Consolidated sources
 stride_Category: S
 severity: critical

 # 2.1 Issue Location
 location:
 files: ["src/auth/login.py:45-67", "src/api/handlers.py:120"]
 elements: ["P1:AuthService", "DF-3:UserCredentials"]
 trust_boundary: "TB-1:Internet/DMZ"

 # 2.2 Detailed Analysis
 detailed_analysis:
 vulnerability_type: "CWE-287 Improper Authentication"
 cwe_ids: [CWE-287, CWE-306]
 capec_ids: [CAPEC-151, CAPEC-600]
 attack_ids: [T1110, T1078]
 description: "Authentication bypass possible via unprotected endpoint"
 technical_details: "The login function at line 45 accepts..."
 attack_surface: "External, unauthenticated"
 affected_data: ["user_credentials", "session_tokens"]
 impact: "Complete authentication bypass, account takeover"

 # 2.3 Root Cause Analysis
 root_cause:
 primary_cause: "Missing authentication check on /api/v2 endpoint"
 contributing_factors:
 - "No centralized authentication middleware"
 - "Inconsistent route protection patterns"
 design_flaw: true
 implementation_flaw: true
 cwe_chain: [CWE-287, CWE-863]

 # 2.4 Test Cases / POC (from Verification Set)
 validation:
 verification_tests: # From WSTG/MASTG
 - test_id: "WSTG-ATHN-01"
 name: "Test for Credentials Transported over Encrypted Channel"
 result: FAIL
 - test_id: "WSTG-ATHN-04"
 name: "Testing for Bypassing Authentication Schema"
 result: FAIL
 test_cases:
 - name: "TC-001: Direct endpoint access"
 method: "GET /api/v2/user/profile without token"
 expected: "401 Unauthorized"
 actual: "200 OK with user data"
 result: FAIL
 poc_available: true
 poc_complexity: low
 cvss_score: 9.1
 kev_status: false

 # 2.5 Mitigation Outline (Phase 7 detailed)
 mitigation:
 priority: "P0"
 strategy: "Implement centralized authentication middleware"
 short_term:
 description: "Add auth check to /api/v2 routes"
 estimated_effort: "2 days"
 long_term:
 description: "Implement API gateway with built-in auth"
 estimated_effort: "2 weeks"

 # ─────────────────────────────────────────────────────────────────
 # Part 3: Attack Path Analysis
 # ─────────────────────────────────────────────────────────────────
 attack_paths:
 - path_id: "AP-001"
 name: "Authentication Bypass to Data Exfiltration"
 risk_refs: ["VR-001", "VR-005", "VR-012"]
 description: "Attacker chains authentication bypass with data access"

 # 3.1 Attack Chain Summary
 attack_chain:
 Entry_point: "External:Internet"
 target: "DataStore:UserDatabase"
 trust_boundaries_crossed: ["TB-1", "TB-2"]
 techniques_used:
 - capec: CAPEC-151
 attack: T1078
 description: "Identity Spoofing via authentication bypass"
 - capec: CAPEC-116
 attack: T1552
 description: "Credential extraction from memory"

 overall_complexity: medium
 detection_difficulty: high
 business_impact: critical

 # ─────────────────────────────────────────────────────────────
 # Part 4: Step-by-Step Attack Flow (Detailed POC)
 # ─────────────────────────────────────────────────────────────
 detailed_steps:
 - step: 1
 phase: "Reconnaissance"
 action: "Enumerate API endpoints via /swagger.json"
 technique: T1592.002
 tools: ["curl", "burpsuite"]
 commands: |
 curl -s https://target.com/swagger.json | jq '.paths | keys'
 expected_result: "List of all API endpoints"

 - step: 2
 phase: "Initial Access"
 action: "Access unprotected /api/v2/user endpoint"
 technique: T1190
 tools: ["curl"]
 commands: |
 curl -s https://target.com/api/v2/user/profile \
 -H "X-Forwarded-For: 127.0.0.1"
 expected_result: "User profile data returned without authentication"

 - step: 3
 phase: "Credential Access"
 action: "Extract session tokens from response"
 technique: T1552.001
 tools: ["jq", "python"]
 poc_code: |
 import requests
 resp = requests.get("https://target.com/api/v2/user/profile")
 tokens = resp.json().get("active_sessions", [])
 print(f"Extracted {len(tokens)} session tokens")
 expected_result: "Valid session tokens extracted"

 - step: 4
 phase: "Lateral Movement"
 action: "Use extracted tokens to access other user accounts"
 technique: T1550.001
 commands: |
 for token in $TOKENS; do
 curl -s https://target.com/api/v1/admin \
 -H "Authorization: Bearer $token"
 done
 expected_result: "Access to admin functionality"

 # ─────────────────────────────────────────────────────────────────
 # Part 5: Feasibility Assessment (canLinepropertyassessment)
 # ─────────────────────────────────────────────────────────────────
 feasibility_assessment:
 - risk_id: "VR-001"
 attack_feasibility:
 access_Required: "Network" # Network/Adjacent/Local/Physical
 privileges_Required: "None" # None/Low/High
 user_interaction: "None" # None/Required
 exploit_complexity: "Low" # Low/High
 detection_likelihood: "Low" # Low/Medium/High
 overall_feasibility: "High" # Critical/High/Medium/Low
 feasibility_score: 9.5 # 0.0-10.0
```

---

## Attack Path Validation Standards (Attack Pathvalidation criteria)

valid Attack Path must meet following validation criteria:

### minimumstructurerequire

| require | standard | description |------|------|------| **Minimum Step Count** | ≥ 2 | Entry Point + Impact (at least) | **Maximum Step Count** | ≤ 10 | Over 10 steps needs to be split into multiple paths | **Must Contain** | Entry_point, target | Starting point and endpoint | **Required for Each Step** | step, phase, action | Number, Phase, Action |

### eachstepRequired Field

```yaml
step_requirements:
 Required:
 - step: integer # stepNumber (1-N)
 - phase: string # attackphase (Reconnaissance/Initial Access/...)
 - action: string # specificactiondescription
 recommended:
 - technique: string # ATT&CK technicalNumbering (T1xxx)
 - tools: array[string] # Tools Used
 - commands: string # specific commands or code
 - expected_result: string # expected result
```

### Valid Path Determination Rules

```yaml
valid_path_criteria:
 Entry_point:
 must_be_one_of:
 - "External:*" # external source
 - "Compromised:*" # compromisedcomponent
 - "Insider:*" # internalpersonnel

 target:
 must_be_one_of:
 - "DataStore:*" # datastorage
 - "Process:*" # criticalprocess
 - "Service:*" # service
 - "Impact:*" # Impact Description

 chain_continuity:
 rule: "each step result must serve as prerequisite for next step"
 validation: "inspect expected_result[N] yesnosupport action[N+1]"

 trust_boundary_crossing:
 rule: "span at least onetrust boundary"
 exception: "internal threat scenario can be exempted"
```

---

## POC Verification Methodology Template (POC Verification Methodology)

> **Critical Quality Requirement**: professionalpenetration testlevelquality

### Verification Methodology Table

```markdown
## Verification Methodology

| Verification Level | description | Example Scenario |---------|------|---------| ✅ **verified** | passed code audit/static analysis confirms exploitable，already have POC code | Hardcoded key、SQL injection code path | ⚠️ **Needs Validation** | needs runtime environment/network prerequisite validation | SSRF DNS Rebinding, timing attack | 📋 **Theoretical** | code path exists，needs specific prerequisite to trigger | Race condition、Memory corruption |
```

### POC Code Example Template (independentPOC Code Blocktemplate)

each high-risk threat must contain independent POC code block，formatas follows：

```markdown
#### POC-{SEQ:03d}: {POC_TITLE}

` ``python
# POC: {brief descriptionpurpose}
# Preconditions: {Prerequisite List}

import requests
import jwt # on-demandimport

# ============================================
# Step 1: {stepdescription}
# ============================================
# {detaileddescription}

# ============================================
# Step 2: {stepdescription}
# ============================================
# {coderealize}

# ============================================
# Verification: {Validation Method}
# ============================================
# expected result: {expected_result}
` ``

**validationStatus**: ✅ codepathalreadyconfirm / ⚠️ needruntimevalidation / 📋 Theoretical
**exploitdifficulty**: low (noneneedinteraction) / medium (needspecificprerequisite) / high (needcomplexenvironment)
**impact**: {Impact Description，such as: complete identity forgery、complete server control}
```

**POC codequalityrequire**:
- Must contain complete import statements
- must have clear step comments
- must explain prerequisites and expected results
- code should be directly executable（after modifying target address）

---

## Attack Path Feasibility Matrix Template (Attack Pathcanfeasibility matrix)

> **Critical Quality Requirement**: quantityizationattackcanLineproperty，supportrisk prioritylevelsort

```markdown
## Attack Pathcanfeasibility matrix

| Attack Path | Entry | Required Permission | Exploit Complexity | Detection Difficulty | Comprehensive Score |---------|------|---------|-----------|---------|---------| {Path Description} | {Network/Internal/Physical} | {None/User/Admin} | {Low/Medium/High} | {Low/Medium/High} | **{0.0-10.0}** |

### scorecalculatemethod

Comprehensive Score = Base Score × permissioncorrection × Complexitycorrection × Detection Difficulty adjustment

| Factor | value | Correction Factor |------|-----|---------| **Base Score** | CVSS Base Score | 1.0 | **Required Permission** | none | ×1.0 | | user | ×0.9 | | administrator | ×0.7 | **Exploit Complexity** | low | ×1.0 | | medium | ×0.85 | | high | ×0.7 | **Detection Difficulty** | high (Hard to Detect) | ×1.1 | | medium | ×1.0 | | low (Easy to Detect) | ×0.9 |
```

---

## Attack Chain ASCII Art Box Template (attack chainASCIIdiagramframetemplate)

each major Attack Path must contain ASCII diagram representation:

```markdown
## attack chainanalysis

### attack chain {N}: {ATTACK_CHAIN_NAME}

` ``
┌─────────────────────────────────────────────────────────────────────────────┐
│ Attack Chain {N}: {ATTACK_CHAIN_NAME} │
├─────────────────────────────────────────────────────────────────────────────┤
│ │
│ 1. {Step 1 Title} │
│ {Step 1 description} │
│ {code snippets or commands (such ashave)} │
│ │ │
│ ▼ │
│ 2. {Step 2 Title} │
│ {Step 2 description} │
│ {criticalcodepath: file.py:line} │
│ │ │
│ ▼ │
│ 3. {Step 3 Title} │
│ {Step 3 description} │
│ │ │
│ ▼ │
│ 4. {Impact Description} │
│ {finalimpactandconsequence} │
│ │
│ CVSS: {X.X} ({Critical/High/Medium}) | canexploitproperty: {high/medium/low} | impact: {description} │
└─────────────────────────────────────────────────────────────────────────────┘
` ``
```

**attack chaindiagramframerequire**:
1. each attack chain must use standard borders (┌─┐, │, └─┘)
2. use arrows between steps (│, ▼) connect
3. Bottom must contain CVSS score、exploitability and impact summary
4. criticalcodelocationmustannotate (file.py:line)

---

## Verification Summary Template

```markdown
## Validation Method summary

### riskstatistics
| classification | Critical/High | Medium | Low | Total |------|---------------|--------|-----|-------| Spoofing | X | X | X | X | Tampering | X | X | X | X | Repudiation | X | X | X | X | Info Disclosure | X | X | X | X | Denial of Service | X | X | X | X | Elevation | X | X | X | X | **Total** | **XX** | **XX** | **XX** | **XX** |

### detailedvalidationresult
| riskID | Attack Path | CAPEC | ATT&CK | WSTG Tests | POCmethod | validationStatus |--------|---------|-------|--------|------------|--------|---------| VR-001 | bypassauthentication→dataaccess | CAPEC-151 | T1078 | WSTG-ATHN-04 | directaccess | verified | VR-002 | SQLinjection→data leakage | CAPEC-66 | T1190 | WSTG-INPV-05 | SQLMap | verified | VR-003 | permissionescalate→privilege escalationaccess | CAPEC-122 | T1087 | WSTG-AUTHZ-02 | IDtraverse | verified |

### Attack Pathsummary
| Path ID | Name | Involved Risks | Complexity | impact |--------|------|---------|--------|------| AP-001 | authenticationbypass to data leakage | VR-001, VR-005 | medium | severity | AP-002 | injectionattack chain | VR-002, VR-008 | low | severity |
```

---

## Checkpoint

Before proceeding to Phase 7, verify:
- [ ] All findings from P1-P5 consolidated and deduplicated
- [ ] Each threat has detailed risk analysis (Part 2)
- [ ] CAPEC and ATT&CK mappings complete
- [ ] Verification Set tests referenced (WSTG/MASTG)
- [ ] POC methods defined with step-by-step commands (Part 4)
- [ ] Attack paths constructed with chained risks (Part 3)
- [ ] Feasibility assessment completed (Part 5)
- [ ] KEV/CVE checked for exploitability context

**Reflection**: Review Attack Paths for realism. Prioritize threats with easier exploitation paths and verified POCs.

---

## Output File

**outputfile**: `.phase_working/P6-RISK-VALIDATION.md`

**File Header**:
```markdown
---
phase: 6
name: "RISK-VALIDATION"
project: "{PROJECT}"
session_id: "{SESSION_ID}"
completed_at: "{ISO_TIMESTAMP}"
framework_version: "v2.1.0"
---

# Phase 6: riskvalidation

[phasecontent...]
```

**→ Next**: Phase 7 (Mitigation Generation) - See `REPORT.md`
