<!-- Code-First Deep Threat Modeling Workflow | Version 2.1.1 | https://github.com/fr33d3m0n/skill-threat-modeling | License: BSD-3-Clause | Welcome to cite but please retain all sources and declarations -->

---
name: threat-modeling
description: |
 Code-first automated threat modeling toolkit. STRICT 8-PHASE WORKFLOW - DO NOT MODIFY.

 **MANDATORY: Create exactly 8 TodoWrite items with these EXACT names:**
 - Phase 1: Project Understanding
 - Phase 2: Call Flow & DFD Analysis
 - Phase 3: Trust Boundary Evaluation
 - Phase 4: Security Design Review
 - Phase 5: STRIDE Threat Analysis
 - Phase 6: Risk Validation ← NOT mitigation!
 - Phase 7: Mitigation Planning ← AFTER validation!
 - Phase 8: Report Generation ← Output to Risk_Assessment_Report/

 **MANDATORY OUTPUT (Phase 8):**
 - Directory: `{PROJECT_ROOT}/Risk_Assessment_Report/`
 - Main report: `{PROJECT}-RISK-ASSESSMENT-REPORT.md` (PROJECT=UPPERCASE)
 - Required: 4 reports + 6 phase docs (P1-P6)
 - ❌ FORBIDDEN: `THREAT-MODEL-REPORT.md` or reports in project root

 Use when: threat model, STRIDE, DFD, security assessment.
---

# Code-First Deep Risk Analysis v2.0

Code-first automated deep threat modeling with comprehensive security chain analysis.

## Execution Mode

**Full Assessment Only** - All 8 phases executed sequentially with maximum depth.

```
Phase 1 ──► Phase 2 ──► Phase 3 ──► Phase 4 ──► Phase 5 ──► Phase 6 ──► Phase 7 ──► Phase 8
Project Call Flow Trust Security STRIDE Risk Mitigation Report
Understanding DFD Boundaries Design Analysis Validation
```

**Strict Workflow Rules**:
1. Phases execute strictly in order (1→2→3→4→5→6→7→8)
2. Each phase output passes to next phase as input
3. Summary and reflection after each phase before proceeding
4. No skipping, reordering, or parallel execution of phases
5. Multi-risk analysis within phases can use parallel sub-agents

### Phase Todo Creation — CRITICAL REQUIREMENT

> ⚠️ **STOP AND READ**: Before ANY analysis, you MUST create EXACTLY 8 todo items.
> DO NOT proceed until you have created all 8 phases as separate todo items.
> DO NOT modify phase names or descriptions. Copy EXACTLY as shown below.

**MANDATORY TodoWrite Call (copy exactly, do not modify)**:

```json
[
 {"content": "Phase 1: Project Understanding", "status": "pending", "activeForm": "Analyzing project architecture and tech stack"},
 {"content": "Phase 2: Call Flow & DFD Analysis", "status": "pending", "activeForm": "Building data flow diagram"},
 {"content": "Phase 3: Trust Boundary Evaluation", "status": "pending", "activeForm": "Identifying trust boundaries"},
 {"content": "Phase 4: Security Design Review", "status": "pending", "activeForm": "Assessing security design"},
 {"content": "Phase 5: STRIDE Threat Analysis", "status": "pending", "activeForm": "Executing STRIDE analysis"},
 {"content": "Phase 6: Risk Validation", "status": "pending", "activeForm": "Validating risks and attack paths"},
 {"content": "Phase 7: Mitigation Planning", "status": "pending", "activeForm": "Developing mitigation measures"},
 {"content": "Phase 8: Report Generation", "status": "pending", "activeForm": "Generating threat modeling report"}
]
```

**VIOLATIONS (will cause incorrect analysis)**:
- ❌ Creating fewer than 8 phases
- ❌ Combining phases (e.g., "Phase 2-7: Complete analysis")
- ❌ Renaming phases (e.g., "Phase 6: Mitigation" instead of "Phase 6: Risk Validation")
- ❌ Skipping Phase 6 (Risk Validation) or Phase 7 (Mitigation Planning)
- ❌ Starting analysis before creating all 8 todo items

**CORRECT execution order**:
1. Phase 6 = Risk Validation — NOT mitigation
2. Phase 7 = Mitigation Planning — comes AFTER validation
3. Phase 8 = Report Generation — final phase, MUST exist

## Report Output Convention

### Output Directory Structure

```
{PROJECT_ROOT}/
└── Risk_Assessment_Report/ ← Final report output directory
 │
 │ ┌─ Required Reports (4 files) ─────────────────────────────────┐
 ├── {PROJECT}-RISK-ASSESSMENT-REPORT.md ← Risk Assessment Report (main)
 ├── {PROJECT}-RISK-INVENTORY.md ← Risk Inventory
 ├── {PROJECT}-MITIGATION-MEASURES.md ← Mitigation Measures
 ├── {PROJECT}-PENETRATION-TEST-PLAN.md ← Penetration Test Plan ✨ NEW
 │ └──────────────────────────────────────────────────────────────┘
 │
 │ ┌─ Phase Documentation (copied from.phase_working) ───────────┐
 ├── P1-PROJECT-UNDERSTANDING.md ← Phase 1 Project Understanding
 ├── P2-DFD-ANALYSIS.md ← Phase 2 DFD Analysis
 ├── P3-TRUST-BOUNDARY.md ← Phase 3 Trust Boundary
 ├── P4-SECURITY-DESIGN-REVIEW.md ← Phase 4 Security Design Review
 ├── P5-STRIDE-THREATS.md ← Phase 5 STRIDE Threat Analysis
 ├── P6-RISK-VALIDATION.md ← Phase 6 Risk Validation
 │ └──────────────────────────────────────────────────────────────┘
 │
 └──.phase_working/ ← Phase working directory (hidden)
 ├── _session_meta.yaml ← Session metadata
 ├── P1-PROJECT-UNDERSTANDING.md ← Phase 1 working doc
 ├── P2-DFD-ANALYSIS.md ← Phase 2 working doc
 ├── P3-TRUST-BOUNDARY.md ← Phase 3 working doc
 ├── P4-SECURITY-DESIGN-REVIEW.md ← Phase 4 working doc
 ├── P5-STRIDE-THREATS.md ← Phase 5 working doc
 ├── P6-RISK-VALIDATION.md ← Phase 6 working doc
 └── P7-MITIGATION-PLAN.md ← Phase 7 working doc
```

### File Naming Convention

**Format**: `{PROJECT}-{REPORT_TYPE}.md`

- **PROJECT**: Extracted from project name, uppercase, max 30 characters
 - Format: `^[A-Z][A-Z0-9-]{0,29}$`
 - Examples: `OPEN-WEBUI`, `MY-PROJECT`, `STRIDE-DEMO`
- **REPORT_TYPE**: Standard report type (uppercase)

| Report Type | Required | Filename Example |
|-------------|----------|------------------|
| Risk Assessment Report (main) | ✅ Always | `OPEN-WEBUI-RISK-ASSESSMENT-REPORT.md` |
| Risk Inventory | ✅ Always | `OPEN-WEBUI-RISK-INVENTORY.md` |
| Mitigation Measures | ✅ Always | `OPEN-WEBUI-MITIGATION-MEASURES.md` |
| Penetration Test Plan | ✅ Always | `OPEN-WEBUI-PENETRATION-TEST-PLAN.md` |
| Architecture Analysis | ⚪ Optional | `OPEN-WEBUI-ARCHITECTURE-ANALYSIS.md` |
| DFD Diagram | ⚪ Optional | `OPEN-WEBUI-DFD-DIAGRAM.md` |
| Compliance Mapping | ⚪ Optional | `OPEN-WEBUI-COMPLIANCE-MAPPING.md` |
| Attack Paths | ⚪ Optional | `OPEN-WEBUI-ATTACK-PATHS.md` |
| Executive Summary | ⚪ Optional | `OPEN-WEBUI-EXECUTIVE-SUMMARY.md` |

**Legend**: ✅ Required | ⚪ Optional

### Phase Output Persistence

**At completion of each phase**:
1. Write phase output to `.phase_working/P{N}-*.md`
2. Update `phases_completed` in `_session_meta.yaml`

**Session metadata** (`_session_meta.yaml`):
```yaml
session_id: "20260103-120000"
project_name: "OPEN-WEBUI"
project_path: "/path/to/project"
started_at: "2026-01-03T12:00:00+08:00"
phases_completed: [1, 2, 3] # Completed phases
current_phase: 4
skill_version: "2.1.0"
```

### Session Recovery

When starting a new session, check `.phase_working/`:
- Exists and `project_name` matches → Prompt: "Continue previous session" or "Overwrite and restart"
- Exists but `project_name` different → Clear directory and start new session
- Does not exist → Create directory and start new session

> **Detailed specification**: See `WORKFLOW.md` Phase 8 section
> **Examples**: See `EXAMPLES.md`

## Language Adaptation Rules

**Note**: This skill has been converted to English-only for personal use. The original bilingual functionality has been removed.

### Language Detection Logic

```
English Only Mode
└── All outputs in English (filenames, content, reports)
```

### Output Scope

| Element | Language | Example |
|---------|----------|---------|
| Report Filenames | English | `PROJECT-RISK-ASSESSMENT-REPORT.md` |
| Phase Output Filenames | English | `P1-PROJECT-UNDERSTANDING.md` |
| Report Content | English | English content |
| Directory Names | English | `Risk_Assessment_Report/` |
| Template Placeholders | English | English (internal use) |


## Skill Path Resolution

**Issue**: Scripts use relative paths `scripts/unified_kb_query.py`, but Claude may work in project root.

**Solution**: Resolve Skill installation path before executing scripts.

### Path Detection Algorithm

```
Priority Order:
1. $SKILL_PATH environment variable (explicit override)
2. Script self-location (when running from skill directory)
3. Project-local paths (multi-platform):
 -.claude/skills/{threat-modeling|skill-threat-modeling}/
 -.agents/skills/{name}/ (Portable/XDG standard)
 -.qwen/agents/{name}/ (Qwen Code)
 -.codex/skills/{name}/ (OpenAI Codex)
 -.github/skills/{name}/ (GitHub Copilot)
 -.goose/skills/{name}/ (Goose)
4. Global paths (multi-platform):
 - ~/.claude/skills/{name}/
 - ~/.config/agents/skills/{name}/ (XDG Portable)
 - ~/.qwen/agents/{name}/
 - ~/.codex/skills/{name}/
 - ~/.config/goose/skills/{name}/
```

**Supported Directory Names**: `threat-modeling`, `skill-threat-modeling` (both work)

### Claude Invocation Pattern

**Step 1**: Detect and cache SKILL_PATH at session start:
```bash
# Use the skill_path.sh helper (recommended - supports all platforms)
SKILL_PATH=$(bash skill_path.sh)

# Or set environment variable explicitly
export SKILL_PATH=/path/to/skill-threat-modeling
```

**Step 2**: Execute scripts using detected path:
```bash
# Standard invocation pattern
python "$SKILL_PATH/scripts/unified_kb_query.py" --stride spoofing

# Or cd to skill directory
cd "$SKILL_PATH" && python scripts/unified_kb_query.py --stride spoofing
```

### Shortcut 1: kb wrapper (Recommended)

Skill includes `kb` wrapper script for invocation from any directory:
```bash
# Use absolute path to invoke kb wrapper
$SKILL_PATH/kb --stride spoofing
$SKILL_PATH/kb --full-chain CWE-89
$SKILL_PATH/kb --all-llm

# Or add to PATH
export PATH="$SKILL_PATH:$PATH"
kb --stride spoofing
```

### Shortcut 2: skill_path.sh

Get skill path for other operations:
```bash
# Get skill path
SKILL_PATH=$(bash skill_path.sh)

# One-liner invocation
python "$(bash skill_path.sh)/scripts/unified_kb_query.py" --stride spoofing
```

### Development Mode

Use source path directly during development:
```bash
# Development path (non-installed mode)
cd /path/to/threat-modeling
python scripts/unified_kb_query.py --stride spoofing
```

### Script Invocation Convention

All `python scripts/unified_kb_query.py...` examples in this document assume:
1. `cd $SKILL_PATH` has been executed, or
2. Use `$SKILL_PATH/kb...` as replacement

**Claude should detect SKILL_PATH at session start and use `kb` wrapper or cd mode.**

---

## Security Knowledge Architecture

### Three Knowledge Sets

The security knowledge system consists of three complementary sets:

```
┌───────────────────────────────────────────────────────────────────────────────────────────────┐
│ Security Knowledge Architecture │
├───────────────────────────────────────────────────────────────────────────────────────────────┤
│ │
│ ┌───────────────────────────────────────────┐ │
│ │ Security Principles │ │
│ │ (Foundation - Guides All Phases) │ │
│ │ DID │ LP │ ZT │ FS │ SOD │ SBD │ CM │ EOM │ OD │ IV │ LA │
│ └───────────────────────────────────────────┘ │
│ │ │
│ ┌─────────────────────────┴─────────────────────────┐ │
│ │ │ │
│ ▼ ▼ │
│ ┌─────────────────────────────────────┐ ┌─────────────────────────────────────┐ │
│ │ Security Control Set │ │ Threat Pattern Set │ │
│ │ (What to do & How to do) │ │ (What to know & Validate) │ │
│ ├─────────────────────────────────────┤ ├─────────────────────────────────────┤ │
│ │ Security Domains (16) │ │ CWE Weakness Types (974) │ │
│ │ │ │ │ │ │ │
│ │ ▼ │ │ ▼ │ │
│ │ Control Sets (18 files, 107) │ │ CAPEC Attack Patterns (615) │ │
│ │ │ │ │ │ │ │
│ │ ▼ │ │ ▼ │ │
│ │ OWASP References (74) │ │ ATT&CK Techniques (835) │ │
│ │ │ │ │ │ │ │
│ │ ▼ │ │ ▼ │ │
│ │ Compliance Frameworks (14) │ │ CVE/KEV Vulnerabilities (323K+) │ │
│ └──────────────┬──────────────────────┘ └──────────────┬──────────────────────┘ │
│ │ │ │
│ │ ┌─────────────────────────────┐ │ │
│ │ │ Verification Set │ │ │
│ │ │ (How to verify & test) │ │ │
│ └─────▶│ │◀───────┘ │
│ │ WSTG Tests (121) │ │
│ │ MASTG Tests (206) │ │
│ │ ASVS Requirements (345) │ │
│ └─────────────────────────────┘ │
│ │ │
│ ▼ │
│ Used in: Phase 6 / Phase 7 / Phase 8 │
│ │
└───────────────────────────────────────────────────────────────────────────────────────────────┘
```

### Security Principles (11)

Core security principles that guide all security design decisions across all 8 phases.

| Code | Principle | Definition |
|------|-----------|------------|
| **DID** | Defense in Depth | Multiple independent security controls; single point failure doesn't compromise system |
| **LP** | Least Privilege | Grant only minimum permissions required to complete task |
| **ZT** | Zero Trust | Never trust, always verify; assume network is compromised |
| **FS** | Fail Secure | Default to most secure state on error |
| **SOD** | Separation of Duties | Critical operations require multiple parties; prevent single-person abuse |
| **SBD** | Secure by Default | Default configuration is secure; user must actively reduce security |
| **CM** | Complete Mediation | Every access must be verified; no bypass paths |
| **EOM** | Economy of Mechanism | Security mechanisms should be simple and auditable; complexity is security's enemy |
| **OD** | Open Design | Security doesn't depend on algorithm or design secrecy |
| **IV** | Input Validation | All input must be validated before processing; default deny |
| **LA** | Least Agency | Limit AI agent autonomy, tool access, and decision scope to minimum required |

**Phase References**:
- Phase 1: DID, LP, ZT, LA (architecture assessment, agent scope)
- Phase 2: CM, IV, ZT (data flow security)
- Phase 3: ZT, SOD, LP, LA (trust boundaries, agent boundaries)
- Phase 4: All 11 principles (security function completeness)

> Detailed definitions in `assets/knowledge/security-principles.yaml`

### Security Control Set

Defines "what to do" and "how to do it" from a defensive perspective.

```
Security Domains ──▶ Control Sets ──▶ OWASP References ──▶ Compliance Frameworks
 │ │ │ │
 security- control-set- reference-set- YAML + SQLite
 design.yaml *.md *.md (compliance tables)
```

**Security Domains (16 total)**:

| Seq | Code | Name | STRIDE | Description |
|-----|------|------|--------|-------------|
| 01 | AUTHN | Authentication & Session | S | Identity verification and session lifecycle |
| 02 | AUTHZ | Authorization & Access Control | E | Access permission enforcement |
| 03 | INPUT | Input Validation | T | External input validation and sanitization |
| 04 | OUTPUT | Output Encoding | T,I | Context-aware output encoding |
| 05 | CLIENT | Client-Side Security | S,T,I | Browser and client-side security |
| 06 | CRYPTO | Cryptography & Transport | I | Data encryption in transit and at rest |
| 07 | LOG | Logging & Monitoring | R | Security event logging and audit |
| 08 | ERROR | Error Handling | I | Secure error handling and information control |
| 09 | API | API & Service Security | S,T,I,D,E | API endpoint and service communication security |
| 10 | DATA | Data Protection | I | Sensitive data and credential protection |
| ext-11 | INFRA | Infrastructure Security | - | Container and orchestration security |
| ext-12 | SUPPLY | Supply Chain Security | - | Dependency and pipeline security |
| ext-13 | AI | AI/LLM Security | - | LLM-specific threats (OWASP LLM Top 10) |
| ext-14 | MOBILE | Mobile Security | - | Mobile app security |
| ext-15 | CLOUD | Cloud Security | - | Cloud-native security controls |
| ext-16 | AGENT | Agentic Security | S,T,R,I,D,E | AI Agent security (OWASP Agentic Top 10) |

### Threat Pattern Set

Defines "what to know" and "what to validate" from an offensive perspective.

```
CWE Weaknesses ──▶ CAPEC Patterns ──▶ ATT&CK Techniques ──▶ CVE/KEV Vulnerabilities
 │ │ │ │
 SQLite:cwe SQLite:capec SQLite:attack_* SQLite:cve + API
 (974 entries) (615 entries) (835 entries) (323K+ entries)
```

**STRIDE to CWE Mapping**:

| STRIDE | Primary CWEs | Attack Patterns |
|--------|--------------|-----------------|
| S (Spoofing) | CWE-287, 290, 307 | CAPEC-151, 194, 600 |
| T (Tampering) | CWE-20, 77, 78, 89 | CAPEC-66, 88, 248 |
| R (Repudiation) | CWE-117, 223, 778 | CAPEC-93 |
| I (Information Disclosure) | CWE-200, 209, 311 | CAPEC-116, 157 |
| D (Denial of Service) | CWE-400, 770, 918 | CAPEC-125, 227 |
| E (Elevation of Privilege) | CWE-269, 284, 862 | CAPEC-122, 233 |

### Verification Set (Cross-Cutting)

Bridges Security Control Set and Threat Pattern Set, providing test procedures for Phase 6/7/8.

| Component | Tests/Requirements | Usage |
|-----------|-------------------|-------|
| WSTG (Web Security Testing Guide) | 121 tests | Phase 6: Risk validation |
| MASTG (Mobile App Security Testing Guide) | 206 tests | Phase 6: Mobile risk validation |
| ASVS (Application Security Verification Standard) | 345 requirements | Phase 7-8: Mitigation & compliance |

**STRIDE to Verification Mapping**:

| STRIDE | Verification Categories | Test Count |
|--------|------------------------|------------|
| S (Spoofing) | WSTG-ATHN/IDNT/SESS, MASTG-AUTH, ASVS-V6/V7/V9/V10 | 240 |
| T (Tampering) | WSTG-INPV/CONF/CLNT, MASTG-PLATFORM/RESILIENCE, ASVS-V1/V2/V3 | 402 |
| R (Repudiation) | WSTG-BUSL, ASVS-V7/V16 | 46 |
| I (Information Disclosure) | WSTG-INFO/ERRH/CRYP, MASTG-STORAGE/NETWORK, ASVS-V11/V12/V14 | 442 |
| D (Denial of Service) | WSTG-BUSL/APIT, MASTG-RESILIENCE | 49 |
| E (Elevation of Privilege) | WSTG-AUTHZ, MASTG-PLATFORM, ASVS-V8 | 90 |
| **Total** | All verification tests mapped to STRIDE | **1,269** |

---

## 8-Phase Workflow Overview

| Phase | Name | Type | Knowledge Sets Used | Key Output |
|-------|------|------|---------------------|------------|
| **1** | Project Understanding | Exploratory | Security Principles | findings_1: project_context |
| **2** | Call Flow & DFD | Constructive | Principles + security-design.yaml | findings_2: dfd_elements |
| **3** | Trust Boundaries | Evaluative | Principles + security-design.yaml | findings_3: boundary_context |
| **4** | Security Design | Evaluative | Control Sets + References | findings_4: security_gaps |
| **5** | STRIDE Analysis | Enumerative | CWE → CAPEC (Threat Pattern Set) | findings_5: threat_inventory |
| **6** | Risk Validation | Verification | Threat Pattern Set + Verification Set | validated_risks |
| **7** | Mitigation | Prescriptive | Control Sets + CWE Mitigations + ASVS | mitigation_plan |
| **8** | Report | Comprehensive | All outputs + Compliance + ASVS | RISK-ASSESSMENT-REPORT.md |

---

## Core Data Model ⚠️ CRITICAL

> **Design Principle**: Starting from data flow and transformation essence, define clear entities, relationships and transformation rules

### Entity Definitions

```
┌─────────────────────────────────────────────────────────────────┐
│ Core Entity Model │
├─────────────────────────────────────────────────────────────────┤
│ │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ Finding (Finding) │ │
│ │ ───────────────── │ │
│ │ Source: Phase 1-4 │ │
│ │ ID: F-P{N}-{Seq} Example: F-P1-001, F-P4-003 │ │
│ │ Nature: Securityrelatedobservation、defect、Riskpoint │ │
│ │ Quantity: Usually 10-30 │ │
│ └─────────────────────────────────────────────────────────┘ │
│ │ │
│ ▼ (Input Phase 5) │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ Threat (Threat) │ │
│ │ ───────────────── │ │
│ │ Source: Phase 5 STRIDE Analysis │ │
│ │ ID: T-{STRIDE}-{ElementID}-{Seq} Example: T-T-P13-002 │ │
│ │ Nature: For DFD ElementpotentialAttackvector │ │
│ │ Quantity: Usually 50-200 (perElementmultiple) │ │
│ │ Status: identified (identified) │ │
│ └─────────────────────────────────────────────────────────┘ │
│ │ │
│ ▼ (Validation Phase 6) │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ ValidatedRisk (ValidationRisk) │ │
│ │ ───────────────── │ │
│ │ Source: Phase 6 Risk Validation │ │
│ │ ID: VR-{Seq} Example: VR-001 │ │
│ │ Nature: AfterValidation、exploitableRisk │ │
│ │ Quantity: Usually 5-30 (Threatmerged/filtered) │ │
│ │ ⚠️ Required: threat_refs[] Trace back to originalThreat │ │
│ └─────────────────────────────────────────────────────────┘ │
│ │ │
│ ▼ (Mitigation Phase 7) │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ Mitigation (Mitigation Measures) │ │
│ │ ───────────────── │ │
│ │ Source: Phase 7 Mitigation Planning │ │
│ │ ID: M-{Seq} Example: M-001 │ │
│ │ Nature: ForValidationRiskremediation plan │ │
│ │ Quantity: Usually 5-20 (can be one-to-many) │ │
│ │ Contains: risk_refs[] Traceability to Validated Risk │ │
│ └─────────────────────────────────────────────────────────┘ │
│ │
└─────────────────────────────────────────────────────────────────┘
```

### Entity Relationships

```
DFD Element 1:N ┌──────────┐
(P01, DS01, DF01...) ─────────────────────▶│ Threat │
 │ (T-xxx) │
 └────┬─────┘
 │
 │ N:1 (merged)
 ▼
 ┌─────────────────┐
Finding ────────────────────────────────▶│ ValidatedRisk │
(F-xxx) merged │ (VR-xxx) │
 │ │
 │ threat_refs: │
 │ [T-T-P13-001, │
 │ T-T-P13-002, │
 │ T-E-P13-001] │
 └────────┬────────┘
 │
 │ N:1 (coverage)
 ▼
 ┌─────────────────┐
 │ Mitigation │
 │ (M-xxx) │
 │ │
 │ risk_refs: │
 │ [VR-001, │
 │ VR-002] │
 └─────────────────┘

Key relationships:
• Threat N:1 ValidatedRisk (MultipleThreatmergedto oneRisk)
• ValidatedRisk N:1 Mitigation (MultipleRiskcan be covered by the same measure)
• All relationships through *_refs[] explicit traceability
```

### Unified ID Convention

| Entity Type | ID Format | Examples | Phase |
|---------|--------|------|------|
| Finding | F-P{N}-{Seq:03d} | F-P1-001 | P1-P4 |
| Threat | T-{STRIDE}-{Element}-{Seq} | T-T-P13-002 | P5 |
| ValidatedRisk | VR-{Seq:03d} | VR-001 | P6 |
| Mitigation | M-{Seq:03d} | M-001 | P7 |
| POC | POC-{Seq:03d} | POC-001 | P6 |
| AttackPath | AP-{Seq:03d} | AP-001 | P6 |
| AttackChain | AC-{Seq:03d} | AC-001 | P6 |

**❌ Prohibited ID Format** (no longer used):
- `RISK-{Seq}` → use instead `VR-{Seq}`
- `T-E-RCE-001` → use instead `T-E-P13-001` (Keep ElementID)
- `SD-{Seq}` → use instead `F-P4-{Seq}`

### Count Conservation Rules ⚠️ CRITICAL

```yaml
# Threatprocessing conservation formula
count_conservation:
 p5_output: "threat_inventory.total = T" # Example: 120
 p6_processing:
 verified: V # ValidationconfirmedThreatcount
 theoretical: Th # theoretically feasibleThreatcount
 pending: P # pendingValidationThreatcount
 excluded: E # excludedThreatcount (with reason)

 conservation_formula: "V + Th + P + E = T"

 traceability_rule: |
 FOR each threat T in p5_output:
 T MUST appear in exactly one VR.threat_refs[]
 OR T.status = 'excluded' with documented reason

 report_consistency:
 RISK-INVENTORY.count = "len(VR where status != 'excluded')"
 MAIN-REPORT.risk_count = "RISK-INVENTORY.count"

# Validationcheckpoint
checkpoints:
 cp1_p5_to_p6: "P6.input_count = P5.threat_inventory.summary.total"
 cp2_p6_output: "sum(verified, theoretical, pending, excluded) = input_count"
 cp3_report_gen: "RISK-INVENTORY.count = P6.final_risk_count"
```

### ValidatedRisk Structure

```yaml
ValidatedRisk:
 # === Identifier ===
 id:
 format: "VR-{Seq:03d}"
 example: "VR-001"

 # === Traceability (MANDATORY!) ===
 threat_refs:
 type: array[Threat.id]
 description: "ThisRiskSourceallThreat ID"
 example: ["T-T-P13-001", "T-T-P13-002", "T-E-P13-001"]
 requirement: "MANDATORY - must list all source Threats"

 finding_refs:
 type: array[Finding.id]
 description: "ThisRiskSource P1-P4 Finding"
 example: ["F-P4-003"]
 requirement: "OPTIONAL - if relatedFinding"

 # === RiskAssessment ===
 severity:
 cvss_score: float # 0.0-10.0
 priority: "P0|P1|P2|P3"
 stride_types: ["T", "E"] # can contain multiple STRIDE Type

 # === ValidationStatus ===
 validation:
 status: "verified|theoretical|pending|excluded"
 poc_available: boolean
 poc_id: "POC-{Seq}" # if POC exists
```

> **Detailed Design**: See `tmp_data/DATA-ARCHITECTURE-DESIGN.md`

### Phase Data Flow

```
┌─────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ Phase Data Flow Architecture │
├─────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ │
│ Phase 1 Phase 2 Phase 3 Phase 4 Phase 5 │
│ ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐ │
│ │ P1 │─findings1─▶│ P2 │─findings2─▶│ P3 │─findings3─▶│ P4 │─findings4─▶│ P5 │ │
│ └──┬──┘ └──┬──┘ └──┬──┘ └──┬──┘ └──┬──┘ │
│ │ ▼ ▼ │ │ │
│ │ security- security- │ │ │
│ │ design.yaml design.yaml │ │ │
│ │ ▼ ▼ │
│ │ control-set-*.md CWE → CAPEC │
│ │ reference-set-*.md │
│ ▼ │ │
│ ┌────────────────────────────────────────────────────────────────────────────────────────┐ │
│ │ Phase 6: Risk Validation │ │
│ │ INPUT: findings_1 + findings_2 + findings_3 + findings_4 + findings_5 │ │
│ │ (ALL issues consolidated and deduplicated) │ │
│ │ KNOWLEDGE: CAPEC → ATT&CK → CVE/KEV + WSTG + MASTG │ │
│ │ OUTPUT: validated_risks │ │
│ │ ├── risk_summary (counts, categorization) │ │
│ │ ├── risk_details (per-item: location, analysis, root cause, test cases) │ │
│ │ └── attack_paths (chains, step-by-step with commands/POC) │ │
│ └───────────────────────────────────────────────────────────────────────────────────────┘ │
│ │ │
│ ▼ │
│ ┌───────────────────────────────────────────────────────────────────────────────────────┐ │
│ │ Phase 7: Mitigation Planning │ │
│ │ INPUT: validated_risks (complete Phase 6 output) │ │
│ │ KNOWLEDGE: Control Sets + OWASP References + CWE Mitigations + ASVS │ │
│ │ OUTPUT: mitigation_plan (per-risk: immediate, short-term, long-term) │ │
│ └───────────────────────────────────────────────────────────────────────────────────────┘ │
│ │ │
│ ▼ │
│ ┌───────────────────────────────────────────────────────────────────────────────────────┐ │
│ │ Phase 8: Report Generation │ │
│ │ INPUT: ALL phase outputs (findings_1 → mitigation_plan) │ │
│ │ KNOWLEDGE: Compliance Frameworks + ASVS │ │
│ │ CRITICAL: Must include COMPLETE Phase 6 and Phase 7 outputs (no omission) │ │
│ └───────────────────────────────────────────────────────────────────────────────────────┘ │
│ │
└─────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

### Phase Context Protocol

**Core Principle**: Each phase must explicitly declare Input Context and Output Context for cross-phase data continuity.

| Phase | Context Name | Key Fields |
|-------|--------------|------------|
| P1→P2 | `project_context` | project_type, modules[], integrations[], security_design{} |
| P2→P3 | `dfd_elements` | elements[{id,type,name}], flows[{id,source,target,data}], dfd_diagram |
| P3→P4 | `boundary_context` | boundaries[], interfaces[], data_nodes[], cross_boundary_flows[] |
| P4→P5 | `security_gaps` | gaps[{domain,severity,description}], design_matrix{} |
| P5→P6 | `threat_inventory` | threats[{id,element_id,stride,cwe,priority}] |
| P6→P7 | `validated_risks` | risk_summary{}, risk_details[], attack_paths[] |
| P7→P8 | `mitigation_plan` | mitigations[{risk_id,measures,implementation}], roadmap{} |

### Element ID Naming Convention

**DFD Element ID Format** (Phase 2 generates, subsequent phases must reference):

| Element Type | Prefix | Format | Example |
|--------------|--------|--------|---------|
| External Interactor | EI | EI{NN} | EI01, EI02 |
| Process | P | P{NN} | P01, P02, P03 |
| Data Store | DS | DS{NN} | DS01, DS02 |
| Data Flow | DF | DF{NN} | DF01, DF02 |
| Trust Boundary | TB | TB{NN} | TB01, TB02 |

**Threat ID Format** (Phase 5 generates):
```
T-{STRIDE}-{ElementID}-{Seq}
```
- STRIDE: S/T/R/I/D/E (single letter)
- ElementID: From P2 element ID
- Seq: Three-digit sequence (001-999)
- Example: `T-S-P01-001` (First Spoofing threat for Process 01)

---

## Phase Details

### Phase 1: Project Understanding <ultrathink><critical thinking>

#### 1.1 Core Analysis Goal
> **Goal**: Comprehensively understand project architecture, functional modules, tech stack, and security design decisions.
> This is an **exploratory** task where LLM needs to understand overall project structure through code reading.

#### 1.2 Input Context
**Input**: Project path/codebase

#### 1.3 Knowledge Reference
**Security Principles**: `assets/knowledge/security-principles.yaml`
- Evaluate if project embodies core security principles (DID, LP, ZT)
- Identify obvious security design flaws

#### 1.4 Script Support
```bash
# Get project structure with type detection
python $SKILL_PATH/scripts/list_files.py <path> --categorize --detect-type --pretty
```

#### 1.5 Output Context
**→ P2**: `project_context` {project_type, modules[], integrations[], security_design{}}

**Required Output**:
```markdown
## Project Summary
- Project Type: [Web/API/Microservice/AI-LLM/Hybrid]
- Primary Language: [Language]
- Framework: [Frameworks]

## Functional Description
- Core Functions: [...]
- User Roles: [...]

## Major Modules
| Module | Responsibility | Location |
|--------|---------------|----------|

## Key Security Design
- Authentication: [...]
- Data Storage: [...]
- External Integrations: [...]
```

**Scenario Confirmation** (based on Phase 1 analysis):

| Scenario Type | Trigger Condition | Enable Extension |
|--------------|-------------------|------------------|
| Standard Web/API | No AI/No Cloud-Native | Standard STRIDE flow |
| AI/LLM Application | Model calls/RAG/Prompt processing detected | `--all-llm`, `--ai-component` |
| Cloud-Native App | AWS/Azure/GCP/K8s detected | `--cloud {provider}` |
| Microservices | Multi-service/Docker/K8s | Cross-service threat analysis |
| Hybrid | Multiple features | Combined extensions |

**Checkpoint**: Summarize and reflect before Phase 2.

---

### Phase 2: Call Flow & DFD <ultrathink><critical thinking>

#### 2.1 Core Analysis Goal
> **Goal**: Build Data Flow Diagram (DFD), trace complete data path from entry to storage.
> This is a **constructive** task where LLM needs to understand code call relationships and visualize data flow.

#### 2.2 Input Context
**← P1**: `project_context`

#### 2.3 Knowledge Reference
**Security Principles**: `assets/knowledge/security-principles.yaml`
- Apply Complete Mediation (CM) to identify access checkpoints
- Apply Input Validation (IV) to mark validation points

**Security Design**: `assets/knowledge/security-design.yaml`
- Reference 16 security domains to identify security-relevant DFD elements

#### 2.4 Output Context
**→ P3**: `dfd_elements` {elements[], flows[], dfd_diagram, dfd_issues[]}

**Output Requirements**:
- ASCII DFD diagram (in body)
- Mermaid DFD source (in appendix)
- Element inventory table

**Checkpoint**: Summarize and reflect before Phase 3.

---

### Phase 3: Trust Boundaries <ultrathink><critical thinking>

#### 3.1 Core Analysis Goal
> **Goal**: Based on DFD, identify trust boundaries, key interfaces, and data nodes; evaluate security posture.
> This is an **evaluative** task where LLM needs to identify security boundaries and assess cross-boundary risks.

#### 3.2 Input Context
**← P1/P2**: `project_context`, `dfd_elements`

#### 3.3 Knowledge Reference
**Security Principles**: Apply ZT, SOD, LP principles
**Security Design**: `assets/knowledge/security-design.yaml` - AUTHN, AUTHZ, API domains

#### 3.4 Output Context
**→ P4**: `boundary_context` {boundaries[], interfaces[], data_nodes[], boundary_issues[]}

**Checkpoint**: Summarize and reflect before Phase 4.

---

### Phase 4: Security Design Assessment <ultrathink><critical thinking>

#### 4.1 Core Analysis Goal
> **Goal**: Evaluate project's design maturity across all security domains, identify gaps.
> This is an **evaluative** task requiring LLM to understand code security implementation and compare with best practices.

#### 4.2 Input Context
**← P1/P2/P3**: All cumulative findings

#### 4.3 Knowledge Reference (Progressive Loading)
1. Load `security-design.yaml` - Get all 16 domains with core requirements
2. For each relevant domain, load corresponding `control-set-*.md`
3. When specific implementation details needed, load `reference-set-*.md`

**Query Commands**:
```bash
# Get security domain details
$SKILL_PATH/kb --control authentication
$SKILL_PATH/kb --stride-controls S
```

#### 4.4 Output Context
**→ P5**: `security_gaps` {gaps[], design_matrix{}}

**Required Output**:
```markdown
## Security Design Assessment Matrix
| Domain | Current Implementation | Assessment | Gap | Risk Level | KB Reference |
|--------|----------------------|------------|-----|------------|--------------|
| AUTHN | [...] | Yes/No/Partial | [...] | High/Medium/Low | control-set-01 |
```

**Checkpoint**: Summarize and reflect before Phase 5.

---

### Phase 5: STRIDE Analysis <ultrathink><critical thinking>

#### 5.1 Core Analysis Goal
> **Goal**: Apply STRIDE method to DFD elements, generate complete threat inventory.
> This is an **enumerative** task where LLM systematically identifies potential threats for each element.

#### 5.2 Input Context
**← P2/P4**: `dfd_elements`, `security_gaps`

#### 5.3 Knowledge Reference
**Threat Pattern Set**: CWE → CAPEC mapping

**Query Commands**:
```bash
$SKILL_PATH/kb --stride spoofing # STRIDE category details
$SKILL_PATH/kb --full-chain CWE-XXX # Complete chain: STRIDE→CWE→CAPEC→ATT&CK
$SKILL_PATH/kb --all-llm # LLM threats (AI components)
```

#### 5.4 STRIDE per Element Matrix

| Element Type | Applicable STRIDE |
|--------------|-------------------|
| Process | S, T, R, I, D, E (all 6) |
| Data Store | T, R, I, D |
| Data Flow | T, I, D |
| External Entity (as source) | S, R |

#### 5.5 Output Context
**→ P6**: `threat_inventory` {threats[{id, element_id, stride, cwe, priority}]}

**Checkpoint**: Summarize and reflect before Phase 6.

---

### Phase 6: Risk Validation <ultrathink><critical thinking>

> **📄 Detailed Workflow**: See `@VALIDATION.md` for complete Phase 6 workflow, consolidation process, and POC templates.

#### 6.1 Core Analysis Goal
> **Goal**: Consolidate ALL findings from P1-P5, perform deep validation, design attack paths and POC.
> This is a **verification** task where LLM thinks from attacker's perspective.

#### 6.2 Input Context
**← ALL P1-P5**: findings_1 + findings_2 + findings_3 + findings_4 + findings_5

**CRITICAL**: Phase 6 must consolidate ALL previous findings, not just Phase 5 threats.

#### 6.3 Knowledge Reference
**Threat Pattern Set**: CAPEC → ATT&CK → CVE/KEV
**Verification Set**: WSTG + MASTG (test generation)

**Query Commands**:
```bash
# Attack path analysis
$SKILL_PATH/kb --capec CAPEC-XXX --attack-chain
$SKILL_PATH/kb --attack-technique TXXX
$SKILL_PATH/kb --check-kev CVE-XXXX

# Verification tests
$SKILL_PATH/kb --stride-tests S # STRIDE category tests
$SKILL_PATH/kb --cwe-tests CWE-89 # CWE-specific tests
$SKILL_PATH/kb --wstg-category ATHN # WSTG tests by category
```

#### 6.4 Output Structure (5 Parts)

> **Schema Reference**: `assets/schemas/risk-detail.schema.md` defines complete risk detail format.
> **Template Reference**: `assets/templates/RISK-ASSESSMENT-REPORT.template.md` Section 5-6 for output format.

**Priority Mapping** (from schema):

| CVSS Score | Severity | Priority | Action Required |
|-----------|---------|--------|----------|
| 9.0 - 10.0 | Critical | P0 | Immediate fix |
| 7.0 - 8.9 | High | P1 | Urgent handling (24h) |
| 4.0 - 6.9 | Medium | P2 | HighPriority (7d) |
| 0.1 - 3.9 | Low | P3 | Planned (30d) |

**POC Verification Status Types**:

| Status Indicator | Meaning | Criteria |
|---------|------|---------|
| ✅ **Validated** | POC executed successfully | successfully reproducedAttackbehavior and obtained expected results |
| ⚠️ **Needs Validation** | theoretically feasiblebut requires manualValidation | requires specific environmentorpermission toValidation |
| 📋 **theoretically feasible** | Based on codeAnalysisderivation | Code path exists but not actually tested |
| ❌ **excluded** | Validationconfirmed not exploitable after | existsMitigation Measuresorconditions not met |

```yaml
validated_risks:
 # Part 1: Risk Summary (Validation Coverage Statistics)
 risk_summary:
 total_identified: N
 total_verified: N # ✅ Validated
 total_pending: N # ⚠️ Needs Validation
 total_theoretical: N # 📋 theoretically feasible
 total_excluded: N # ❌ excluded
 verification_rate: "N%"
 risk_by_severity: {critical: N, high: N, medium: N, low: N}
 risk_by_stride: {S: N, T: N, R: N, I: N, D: N, E: N}

 # Part 2: POC Details (Each Critical/High Threatone complete block)
 poc_details:
 - poc_id: "POC-001" # Format: POC-{SEQ:03d}
 threat_ref: "T-S-P01-001" # RelatedThreatID
 stride_type: "S" # STRIDEType
 verification_status: "verified" # verified|pending|theoretical|excluded
 exploitation_difficulty: "medium" # low|medium|high
 prerequisites: # Prerequisites
 - "ValidUser session"
 - "Know target userID"
 vulnerability_location:
 file_path: "src/api/auth.py"
 function_name: "verify_token"
 line_number: 45
 vulnerable_code: | # VulnerabilityCode snippet
 def verify_token(token):
 # Missing signatureValidation
 return jwt.decode(token, options={"verify_signature": False})
 exploitation_steps: # Exploitation steps
 - "Construct maliciousJWT Token"
 - "Send request to authentication endpoint"
 - "Bypass identity files Validation"
 poc_code: | # POCCode (Complete executable)
 import jwt
 # Construct malicioustoken
 malicious_token = jwt.encode({"user_id": "admin"}, "any_key", algorithm="HS256")
 # SendRequest...
 expected_result: | # Expected result
 {"status": "authenticated", "user": "admin"}
 verification_log: "..." # ValidationLog/Screenshot
 risk_assessment:
 complexity: "medium"
 attack_vector: "network"
 impact_scope: "user_data"
 data_sensitivity: "high"

 # Part 3: Risk Details (per item) - See assets/schemas/risk-detail.schema.md
 risk_details:
 - risk_id: "VR-001" # Format: VR-{SEQ:03d}
 original_refs: ["T-S-P01-001", "SD-001"] # From multiple phases
 priority: "P1" # P0/P1/P2/P3
 location: {files: [], elements: [], trust_boundary: ""}
 detailed_analysis: {...}
 root_cause: {...}
 related_cwe: "CWE-XXX" # Required field
 related_poc: "POC-001" # Link to POC detail
 validation:
 test_cases: []
 poc_available: true
 cvss_score: 8.8
 verification_status: "verified"

 # Part 4: Attack Path Feasibility Matrix (Feasibility score)
 attack_path_matrix:
 - path_id: "AP-001"
 path_name: "authentication Bypass→DatabaseAccess"
 entry_point: "API Gateway"
 key_nodes: ["Auth Service"]
 final_target: "Database"
 feasibility_score: 8.5 # 0.0-10.0
 detection_difficulty: "low" # low|medium|high
 priority_fix: true

 # Part 5: Attack Chains (Complete Attack Chain Analysis)
 attack_chains:
 - chain_id: "AC-001"
 chain_name: "Privilege escalationAttackChain"
 entry_point: "PublicAPIEndpoint"
 target: "AdministratorPermission"
 impact_scope: "CompleteSystemControl"
 difficulty: "medium"
 related_threats: ["T-E-P01-001", "T-S-P02-001"]
 steps:
 - step: 1
 title: "InitialAccess"
 source: "AttackActor"
 target: "APIGateway"
 action: "SendMaliciousRequest"
 code_location: "api/routes.py:120"
 data_change: "Obtain sessiontoken"
 - step: 2
 title: "Privilege escalation"
 source: "APIGateway"
 target: "authentication Service"
 action: "ExploitJWTVulnerability"
 code_location: "auth/jwt.py:45"
 data_change: "ObtainAdministratorRole"
 # ASCII AttackChainDiagram (Must be displayed in report)
 attack_flow_diagram: |
 ┌─────────────────────────────────────────────────────────────────┐
 │ AttackChain: Privilege escalationAttack │
 ├─────────────────────────────────────────────────────────────────┤
 │ Step 1: InitialAccess │
 │ ┌─────────────────────────────────────────────────────────┐ │
 │ │ AttackActor ──→ APIGateway │ │
 │ │ Action: SendMaliciousRequest │ │
 │ │ CodeLocation: api/routes.py:120 │ │
 │ └─────────────────────────────────────────────────────────┘ │
 │ │ │
 │ ▼ │
 │ Step 2: Privilege escalation │
 │ ┌─────────────────────────────────────────────────────────┐ │
 │ │ APIGateway ──→ authentication Service │ │
 │ │ Action: ExploitJWTVulnerabilityObtainAdministratorRole │ │
 │ │ CodeLocation: auth/jwt.py:45 │ │
 │ └─────────────────────────────────────────────────────────┘ │
 │ │ │
 │ ▼ │
 │ Result: ObtainAdministratorPermission，CompleteSystemControl │
 └─────────────────────────────────────────────────────────────────┘
 prerequisites:
 - "NetworkAccessPermission"
 - "basic user account"
 exploitation_commands: |
 # Step 1: ObtainInitialtoken
 curl -X POST https://target/api/login -d '{"user":"test","pass":"test"}'
 # Step 2: constructPrivilege elevationtoken
 python3 jwt_exploit.py --token $TOKEN --role admin
 ioc_indicators:
 - "AbnormalJWT tokenStructure"
 - "In short timeRoleChange"
 defense_recommendations:
 - cutpoint: "Step 1"
 recommendation: "ImplementRequestRateLimit andAbnormaldetection"
 - cutpoint: "Step 2"
 recommendation: "EnableJWTsignature Validation，Use strong keys"
```

#### 6.5 Output Quality Requirements

**CRITICAL**: Phase 6 output MUST include:
1. **POC Details**: Every Critical/High threat must have a complete POC block with executable code
2. **Attack Chains**: At least one detailed attack chain diagram per high-risk attack path
3. **Feasibility Matrix**: All attack paths must have feasibility scores (0.0-10.0)
4. **ASCII Diagrams**: Attack chains must include ASCII box diagrams in `attack_flow_diagram` field

**Checkpoint**: Summarize and reflect before Phase 7.

---

### Phase 7: Mitigation Planning <ultrathink><critical thinking>

> **📄 Detailed Workflow**: See `@REPORT.md` for complete Phase 7-8 workflow with content aggregation instructions.

#### 7.1 Core Analysis Goal
> **Goal**: Design specific mitigation measures and implementation plans for each validated risk.
> This is a **prescriptive** task requiring LLM to design feasible security controls for the tech stack.

#### 7.2 Input Context
**← P6**: `validated_risks` (complete Phase 6 output)

#### 7.3 Knowledge Reference
**Security Control Set**: Control Sets + OWASP References
**Threat Pattern Set**: CWE Mitigations
**Verification Set**: ASVS (requirement verification)

**Query Commands**:
```bash
$SKILL_PATH/kb --cwe CWE-XXX --mitigations # CWE mitigations
$SKILL_PATH/kb --control authentication # Security controls
$SKILL_PATH/kb --asvs-level L2 # ASVS requirements
$SKILL_PATH/kb --asvs-chapter V4 # ASVS by chapter
```

#### 7.4 Output Context
**→ P8**: `mitigation_plan` {mitigations[], roadmap{}}

**Required Output for Each Risk**:
```markdown
### Mitigation Measures
| Risk ID | CWE | Recommended Measure | Implementation | Priority | Effort |
|---------|-----|---------------------|----------------|----------|--------|
| VR-XXX | CWE-XXX | [Measure] | [Code/Config] | Critical/High/Medium/Low | [Est.] |
```

**Checkpoint**: Summarize and reflect before Phase 8.

---

### Phase 8: Comprehensive Report <ultrathink><critical thinking>

> **📄 Detailed Workflow**: See `@REPORT.md` for complete Phase 8 workflow with **mandatory content aggregation rules**.
> **⚠️ CRITICAL**: Phase 8 MUST read all phase files and copy content completely — do NOT summarize from memory!

#### 8.1 Core Analysis Goal
> **Goal**: Synthesize all phase outputs into complete threat model report.
> This is a **comprehensive** task where LLM integrates all 7 phases of analysis.

#### 8.2 Input Context
**← P1-P7**: ALL preceding phase outputs

#### 8.3 Knowledge Reference
**Compliance Frameworks** + **ASVS** (compliance verification)

```bash
$SKILL_PATH/kb --compliance nist-csf
$SKILL_PATH/kb --asvs-level L2 --chapter V1
```

#### 8.4 ⚠️ MANDATORY: Output Directory Setup

**Before generating any reports，Must execute the followingStep**:

1. **Determine PROJECT Name**: Extracted from project name，transformationasUppercase
 - Examples: `open-webui` → `OPEN-WEBUI`
 - Format: `^[A-Z][A-Z0-9-]{0,29}$`

2. **Create output directory**:
 ```bash
 mkdir -p {PROJECT_ROOT}/Risk_Assessment_Report/
 ```

3. **All reports must output to this directory**:
 - Main Report: `Risk_Assessment_Report/{PROJECT}-RISK-ASSESSMENT-REPORT.md`
 - Risk Inventory: `Risk_Assessment_Report/{PROJECT}-RISK-INVENTORY.md`
 - Mitigation Measures: `Risk_Assessment_Report/{PROJECT}-MITIGATION-MEASURES.md`

⚠️ **Prohibited**: Create report files directly in project root！

#### 8.5 Report Structure (9 Sections + Appendix)
**CRITICAL**: Sections 5, 6, and 8 must include COMPLETE Phase 6 and Phase 7 outputs without omission.

> **Template Reference**: `assets/templates/RISK-ASSESSMENT-REPORT.template.md`

```markdown
# {PROJECT}-RISK-ASSESSMENT-REPORT.md

## 1. Executive Summary
- Threat Statistics、STRIDE Distribution、Key Findings、Immediate Action Recommendations

## 2. System Architecture
- Component Topology ASCII Diagram、Data Flow Diagram (DFD)、Trust Boundaries、Technology Stack
- (from findings_1, findings_2, findings_3)

## 3. Security Design Assessment
- 9Security Domain Assessment Matrix、Key Security Findings Details
- (from findings_4)

## 4. STRIDE Threat Analysis
- Threat Summary Table、STRIDE Classification Table、Detailed Threat Analysis
- (from findings_5)

## 5. Risk Validation & POC ← CRITICAL
- POC Validation Methodology、Validation Coverage Statistics、POC Validation Details、POC Summary Table
- EachCritical/HighThreatMust have completePOCCode block
- (from validated_risks.poc_details)

## 6. Attack Path Analysis ← CRITICAL
- Attack Path Feasibility Matrix、Attack Chain Detailed Analysis、Attack Surface Heatmap、Priority Ranking
- Each high-riskAttack PathsMust haveASCIIAttackChainDiagram
- (from validated_risks.attack_chains, validated_risks.attack_path_matrix)

## 7. Threat Priority Matrix
- Risk Assessment Matrix、Threat Distribution Matrix、Attack Surface Heatmap

## 8. Mitigation Recommendations ← CRITICAL
- P0/P1/P2Tiered Measures、Implementation Roadmap、Defense-in-Depth Architecture
- (from mitigation_plan)

## 9. Compliance Mapping
- OWASP Top 10Mapping、OWASP LLM Top 10Mapping(if applicable)

## Appendices
- A: DFD ElementsComplete List
- B: Mermaid DFDSource Code
- C: Complete Threat List
- D: Knowledge Base Query Records
- E: References
```

#### 8.6 Output Files

**Output directory**: `{PROJECT_ROOT}/Risk_Assessment_Report/`

**Required Reports** (AlwaysGenerate):
| Number | Report file | Description |
|------|---------|------|
| 1 | `{PROJECT}-RISK-ASSESSMENT-REPORT.md` | Risk Assessment Report (Main Report) |
| 2 | `{PROJECT}-RISK-INVENTORY.md` | Risk Inventory |
| 3 | `{PROJECT}-MITIGATION-MEASURES.md` | Mitigation Measures |
| 4 | `{PROJECT}-PENETRATION-TEST-PLAN.md` | Penetration Test Plan |

#### 8.7 ⚠️ MANDATORY: Phase Output Publication

**After generating all reports，Must executePhase DocumentationPublication**:

Copy `.phase_working/` inphase outputs to `Risk_Assessment_Report/` Directory，**KeepEnglish filenames**:

```yaml
phase_output_publication:
 source_dir: ".phase_working/"
 target_dir: "Risk_Assessment_Report/"
 files: # Direct copy，KeepOriginal filenames
 - P1-PROJECT-UNDERSTANDING.md # Phase 1 Project Understanding
 - P2-DFD-ANALYSIS.md # Phase 2 DFD Analysis
 - P3-TRUST-BOUNDARY.md # Phase 3 Trust Boundaries
 - P4-SECURITY-DESIGN-REVIEW.md # Phase 4 Security Design Assessment
 - P5-STRIDE-THREATS.md # Phase 5 STRIDE Threat Analysis
 - P6-RISK-VALIDATION.md # Phase 6 Risk Validation
```

**Execute commands** (Examples):
```bash
# CopyPhase Documentationto report directory (Keep English names)
cp.phase_working/P1-PROJECT-UNDERSTANDING.md Risk_Assessment_Report/
cp.phase_working/P2-DFD-ANALYSIS.md Risk_Assessment_Report/
cp.phase_working/P3-TRUST-BOUNDARY.md Risk_Assessment_Report/
cp.phase_working/P4-SECURITY-DESIGN-REVIEW.md Risk_Assessment_Report/
cp.phase_working/P5-STRIDE-THREATS.md Risk_Assessment_Report/
cp.phase_working/P6-RISK-VALIDATION.md Risk_Assessment_Report/
```

**Value explanation**:
- Phase DocumentationRecord completeAnalysisProcess
- Support auditTraceabilityand qualityValidation
- Facilitate team understandingThreatModeling derivation logic
- Keep English namesEnsure naming convention consistency

---

## Scripts Reference

| Script | Purpose | Key Commands |
|--------|---------|--------------|
| `list_files.py` | Phase 1: File listing | `--categorize`, `--detect-type` |
| `stride_matrix.py` | Phase 5: STRIDE matrix | `--element`, `--generate-id` |
| `unified_kb_query.py` | **Phase 4-8: KB queries** | See below |

### unified_kb_query.py - Complete Parameter Reference

#### STRIDE Queries
```bash
--stride {spoofing|tampering|repudiation|information_disclosure|denial_of_service|elevation_of_privilege}
--all-stride # All STRIDE categories overview
--element {process|data_store|data_flow|external_interactor}
```

#### CWE Queries
```bash
--cwe CWE-XXX # Query specific CWE
--cwe CWE-XXX --mitigations # Include detailed mitigations
--full-chain CWE-XXX # Complete chain: STRIDE→CWE→CAPEC→ATT&CK
```

#### CAPEC Attack Patterns
```bash
--capec CAPEC-XXX # Query specific CAPEC
--capec CAPEC-XXX --attack-chain # Include ATT&CK technique mapping
```

#### ATT&CK Techniques
```bash
--attack-technique TXXX # Query ATT&CK technique
--attack-mitigation MXXX # Query ATT&CK mitigation
--attack-search "keyword" # Search ATT&CK techniques
```

#### CVE Queries
```bash
--cve CVE-XXXX-XXXXX # Direct CVE query
--cve-for-cwe CWE-XXX # CVEs by CWE
--cve-severity {CRITICAL|HIGH|MEDIUM|LOW}
--check-kev CVE-XXXX # Check Known Exploited Vulnerability
```

#### Verification Set Queries (NEW)
```bash
--stride-tests {S|T|R|I|D|E} # Get verification tests for STRIDE category
--cwe-tests CWE-XXX # Get verification tests for CWE
--asvs-level {L1|L2|L3} # Get ASVS requirements by level
--asvs-chapter {V1|V2|...} # Get ASVS requirements by chapter
--wstg-category {ATHN|AUTHZ|...} # Get WSTG tests by category
```

#### Cloud & LLM Extensions
```bash
--cloud {aws|azure|gcp|alibaba|tencent}
--category {compute|storage|database|networking|identity|serverless}
--llm LLM01 # Query OWASP LLM Top 10
--all-llm # All OWASP LLM Top 10 threats
--ai-component {llm_inference_service|rag_retrieval|...}
```

#### Semantic Search
```bash
--semantic-search "query" # Natural language search
--search-type {cwe|capec|all}
```

---

## Knowledge Base Layers

| Layer | Source | Content | Use Case |
|-------|--------|---------|----------|
| **L1: Curated** | YAML + Markdown | Security domains, controls, references | Phase 2-4 |
| **L2: Indexed** | SQLite (18MB) | 974 CWEs, 615 CAPECs, 835 ATT&CK | Phase 5-7 |
| **L3: Extension** | SQLite (304MB) | 323K+ CVEs | Phase 6 CVE lookup |
| **L4: Live** | NVD/KEV API | Real-time CVE/KEV | Exploit context |
| **L5: Verification** | SQLite | WSTG, MASTG, ASVS | Phase 6-8 |

---

## Parallel Sub-Agent Pattern <ultrathink><critical thinking>

For Phases 5/6/7 with multiple risks:

```
Main Agent Sub-Agents (Parallel)
 │ ┌─────────────────┐
 │──► Risk 1 ──────────────► Agent 1 ──► KB Query ──► Result 1
 │ └─────────────────┘
 │ ┌─────────────────┐
 │──► Risk 2 ──────────────► Agent 2 ──► KB Query ──► Result 2
 │ └─────────────────┘
 │ ┌─────────────────┐
 │──► Risk N ──────────────► Agent N ──► KB Query ──► Result N
 │ └─────────────────┘
 │
 ◄───────────────── Aggregate Results ──────────────────
```

---

## Large Project Handling

| Scale | File Count | Module Count | Strategy |
|-------|------------|--------------|----------|
| Small | <50 | <5 | Standard 8-phase analysis |
| Medium | 50-200 | 5-15 | Module-priority (key modules deep) |
| Large | 200-500 | 15-30 | Subsystem split + merge |
| Very Large | >500 | >30 | Layered analysis + parallel sub-agents |

**Subsystem Threat ID**: `T-{STRIDE}-{SubsysID}-{ElementID}-{Seq}`

---

## Common Pitfalls

| Pitfall | Solution |
|---------|----------|
| Skipping phases | Execute all 8 phases in order |
| Not using KB queries | Use `unified_kb_query.py` for every risk |
| Generic mitigations | Query CWE-specific mitigations from KB |
| Missing attack paths | Use CAPEC + ATT&CK for verification |
| No reflection | Summarize and reflect after each phase |
| Parallel phase execution | Phases are strictly serial |
| Incomplete Phase 6 consolidation | Must include ALL P1-P5 findings |
| Phase 8 omissions | Must include COMPLETE P6 and P7 outputs |

---

## Reference Files

**Workflow & Phase Details** (load progressively):
- `@WORKFLOW.md` - Phase 1-5 detailed workflow
- `@VALIDATION.md` - Phase 6 (Risk Validation) complete workflow, consolidation process, POC templates
- `@REPORT.md` - Phase 7-8 (Mitigation & Report) with **mandatory content aggregation rules**
- `EXAMPLES.md` - Real-world threat modeling examples

**Schemas** (format specifications):
- `assets/schemas/risk-detail.schema.md` - Risk detail format, priority mapping (P0-P3), required fields
- `assets/schemas/phase-risk-summary.schema.md` - Phase output summary format (if exists)

**Knowledge Base** (query via scripts—do NOT load directly):
- `assets/knowledge/security_kb.sqlite` - Core database
- `assets/knowledge/security_kb_extension.sqlite` - CVE extension
- `assets/knowledge/*.yaml` - Curated mappings
- `assets/knowledge/security-controls/*.md` - Control sets
- `assets/knowledge/security-controls/references/*.md` - OWASP references
