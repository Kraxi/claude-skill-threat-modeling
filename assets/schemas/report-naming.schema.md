<!-- Code-First Deep Threat Modeling Workflow | Version 2.1.0 | https://github.com/fr33d3m0n/skill-threat-modeling | License: BSD-3-Clause | Welcome to cite but please retain all sources and declarations -->

# Report Naming Schema

> **Version**: 1.5.0
> **Last Updated**: 2026-01-02
> **Module**: Report Module v2.0.4

---

## 1. Overview

This document definesThreatmodelingreport standardNaming Convention、reportTypeandoutputlocation。

**design goal**:
- unifiedreportnamingFormat
- clarify 8 report type definitions
- standardize output directory Structure

---

## 2. Naming Convention

### 2.1 Basic Format

```
{PROJECT}-{REPORT_TYPE}.md
```

### 2.2 PROJECT Specification

```yaml
project_name:
 source: |
 1. user explicitly specifies
 2. from project_context.name extract
 3. from project root directory package.json/pyproject.toml extract
 4. fromprojectdirectoryname inference

 format:
 case: "UPPERCASE"
 separator: "-"
 max_length: 30
 allowed_chars: "[A-Z0-9-]"

 examples:
 - "N8N"
 - "MY-PROJECT"
 - "WEBAPP-V2"
 - "TRADING-PLATFORM"

 invalid_examples:
 - "my_project" # underscore not allowed
 - "MyProject" # mixed case not allowed
 - "my project" # spacenotallow
```

### 2.3 REPORT_TYPE standardize

```yaml
# v2.0.0 standard 8 typesreportType
report_types:
 - id: "main"
 type: "RISK-ASSESSMENT-REPORT"
 description: "main comprehensivereport"
 required: true
 template: "assets/templates/RISK-ASSESSMENT-REPORT.template.md"

 - id: "arch"
 type: "ARCHITECTURE-ANALYSIS"
 description: "architectureanalysisreport"
 required: false
 template: "assets/templates/ARCHITECTURE-ANALYSIS.template.md"

 - id: "dfd"
 type: "DFD-ANALYSIS"
 description: "dataflowanalysisreport"
 required: false
 template: "assets/templates/DFD-ANALYSIS.template.md"

 - id: "boundary"
 type: "TRUST-BOUNDARY-ANALYSIS"
 description: "trust boundaryanalysisreport"
 required: false
 template: "assets/templates/TRUST-BOUNDARY-ANALYSIS.template.md"

 - id: "design"
 type: "SECURITY-DESIGN-REVIEW"
 description: "securitydesign reviewreport"
 required: false
 template: "assets/templates/SECURITY-DESIGN-REVIEW.template.md"

 - id: "risk"
 type: "RISK-INVENTORY"
 description: "riskchecklistreport"
 required: true
 template: "assets/templates/RISK-INVENTORY.template.md"

 - id: "attack"
 type: "ATTACK-PATH-VALIDATION"
 description: "attack pathvalidationreport"
 required: false
 template: "assets/templates/ATTACK-PATH-VALIDATION.template.md"

 - id: "mitigation"
 type: "MITIGATION-MEASURES"
 description: "Mitigation Measurereport"
 required: true
 template: "assets/templates/MITIGATION-MEASURES.template.md"

 - id: "pentest"
 type: "PENETRATION-TEST-PLAN"
 description: "penetrationtestsolution"
 required: true
 template: "assets/templates/PENETRATION-TEST-PLAN.template.md"
```

---

## 3. standardreportdefinition

### 3.1 reportTypematrix (v2.0.0)

| # | reportID | filenaming pattern | maincontent | mainsourcePhase | required |
|---|--------|-----------|---------|-------------|------|
| 1 | main | `{PROJECT}-RISK-ASSESSMENT-REPORT.md` | completeThreatmodel，containplacehavesection | P1-P8 all | ✅ |
| 2 | risk | `{PROJECT}-RISK-INVENTORY.md` | completeriskchecklist + eachindividualrisk detailsblock | P5, P6 | ✅ |
| 3 | mitigation | `{PROJECT}-MITIGATION-MEASURES.md` | mitigationrecommendation、codeExample、prioritylevelroadmapfigure | P7 | ✅ |
| 4 | pentest | `{PROJECT}-PENETRATION-TEST-PLAN.md` | penetrationtestsolution、testuse case、ATT&CKmapping | P6, P7 | ✅ |
| 5 | arch | `{PROJECT}-ARCHITECTURE-ANALYSIS.md` | systemarchitecture、componenttopology、technologystack | P1, P2 | |
| 6 | dfd | `{PROJECT}-DFD-DIAGRAM.md` | data flow diagram、elementchecklist、datacategory | P2 | |
| 7 | compliance | `{PROJECT}-COMPLIANCE-REPORT.md` | complianceframeworkmapping、gapanalysis | P5, P7 | |
| 8 | attack | `{PROJECT}-ATTACK-PATH-VALIDATION.md` | attack path、POCmethod、validationresult | P6 | |

### 3.2 eachreportdetaileddefinition

#### report 1: mainreport (RISK-ASSESSMENT-REPORT)

```yaml
report_main:
 id: "main"
 filename: "{PROJECT}-RISK-ASSESSMENT-REPORT.md"
 description: "Threatmodel mainreport，containcompleteanalysisresult"

 primary_sources:
 - P1: project_context
 - P2: dfd_elements
 - P3: boundary_context
 - P4: security_gaps
 - P5: threat_inventory
 - P6: validated_threats
 - P7: mitigation_plan

 structure:
 - section: "1. executesummaryneed"
 subsections:
 - "1.1 projectOverview"
 - "1.2 assessmentconclusion (statisticstable)"
 - "1.3 keyFinding (Top 3-5)"
 - "1.4 immediaterowdynamicrecommendation"

 - section: "2. systemarchitectureoverview"
 subsections:
 - "2.1 componenttopology (ASCII)"
 - "2.2 data flow diagram (ASCII)"
 - "2.3 trust boundary"
 - "2.4 technologystack"

 - section: "3. securityfunctioncandesignassessment"
 subsections:
 - "3.1 assessmentmatrix (9domain)"
 - "3.2 keysecurityFindingdetails"

 - section: "4. STRIDE Threatanalysis"
 subsections:
 - "4.1 Threatsummarytable (bySTRIDEcategory)"
 - "4.2-4.7 eachSTRIDEcategorydetailedtableformat"
 - "4.X Threatdetailedanalysis (Critical/High risk)"

 - section: "5. Threatpriority matrix"
 subsections:
 - "5.1 riskassessmentmatrix"
 - "5.2 attack surfaceheatmapfigure"

 - section: "6. Mitigation Measurerecommendation"
 subsections:
 - "6.1 P0 - immediatefix"
 - "6.2 P1 - urgent"
 - "6.3 P2 - highprioritylevel"
 - "6.4 implementation roadmapfigure (priority onlylevel，nowhentime estimate)"

 - section: "7. compliancepropertymapping"
 subsections:
 - "7.1 OWASP Top 10 mapping"
 - "7.2 OWASP LLM Top 10 mapping (such asapplicable)"

 - section: "appendix"
 subsections:
 - "A. DFDelementcompletechecklist"
 - "B. Mermaid DFD source code"
 - "C. Threatcompletechecklist"
 - "D. knowledgelibraryqueryrecord"
 - "E. reference materials"

 length_estimate: "50-100 page"
```

#### report 2: architectureanalysis (ARCHITECTURE-ANALYSIS)

```yaml
report_arch:
 id: "arch"
 filename: "{PROJECT}-ARCHITECTURE-ANALYSIS.md"
 description: "systemarchitectureandtechnologystackanalysis"

 primary_sources:
 - P1: project_context
 - P2: dfd_elements

 structure:
 - section: "1. projectOverview"
 subsections:
 - "1.1 projectTypeandpurpose"
 - "1.2 technologystacksummaryneed"
 - "1.3 codeStructureoverview"

 - section: "2. componenttopology"
 subsections:
 - "2.1 highlayerarchitecturefigure (ASCII)"
 - "2.2 moduledepend onrelationship"
 - "2.3 externalservicesetbecome"

 - section: "3. technologystackdetails"
 subsections:
 - "3.1 programming languageandframework"
 - "3.2 datalibraryandstorage"
 - "3.3 message queueandcache"
 - "3.4 securityrelatedcomponent"

 - section: "4. securityrelatedmodule"
 subsections:
 - "4.1 authenticationmodule"
 - "4.2 authorizationmodule"
 - "4.3 encryptionmodule"
 - "4.4 logandauditmodule"

 - section: "5. initialattack surface"
 subsections:
 - "5.1 externalininterfacepoint"
 - "5.2 API endpoint"
 - "5.3 sensitivedatalocation"

 length_estimate: "15-25 page"
```

#### report 3: DFD figure (DFD-DIAGRAM)

```yaml
report_dfd:
 id: "dfd"
 filename: "{PROJECT}-DFD-DIAGRAM.md"
 description: "data flow diagramandtrust boundaryanalysis"

 primary_sources:
 - P2: dfd_elements
 - P3: boundary_context

 structure:
 - section: "1. DFD overview"
 subsections:
 - "1.1 Level 0 contextfigure"
 - "1.2 Level 1 systemfigure"

 - section: "2. DFD elementchecklist"
 subsections:
 - "2.1 process (Processes)"
 - "2.2 datastorage (Data Stores)"
 - "2.3 dataflow (Data Flows)"
 - "2.4 externalentity (External Entities)"

 - section: "3. trust boundary"
 subsections:
 - "3.1 edgeboundarydefinition"
 - "3.2 edgeboundary crossinganalysis"
 - "3.3 keyinterface"

 - section: "4. sensitivedataflow"
 subsections:
 - "4.1 PII dataflow"
 - "4.2 credentialsdataflow"
 - "4.3 other sensitivedata"

 - section: "appendix"
 subsections:
 - "A. Mermaid DFD source code"
 - "B. elementattributedetails"

 length_estimate: "10-20 page"
```

#### report 4: riskchecklist (RISK-INVENTORY)

```yaml
report_risk:
 id: "risk"
 filename: "{PROJECT}-RISK-INVENTORY.md"
 description: "completeriskchecklistandeachindividualrisk detailsblock"

 primary_sources:
 - P5: threat_inventory
 - P6: validated_threats

 structure:
 - section: "1. Risk Statisticssummaryneed"
 subsections:
 - "1.1 byseveritystatistics"
 - "1.2 by STRIDE categorystatistics"
 - "1.3 bycomponentdistribution statistics"

 - section: "2. risksummarytable"
 content: "placehaverisk tableformatlist"

 - section: "3. risk details"
 content: "eachindividualrisk completedetailsblock (by risk-detail.schema.md)"
 per_risk_sections:
 - "Basic Information"
 - "riskdescription"
 - "locationlocate"
 - "reasonanalysis"
 - "attack path"
 - "impactassessment"
 - "Mitigation Measure"

 notes:
 - "eachindividual Critical/High riskmusthavecompletedetailsblock"
 - "Medium/Low riskcanbyusebriefizeFormat"

 length_estimate: "30-80 page (dependinriskquantity)"
```

#### report 5: Mitigation Measure (MITIGATION-MEASURES)

```yaml
report_mitigation:
 id: "mitigation"
 filename: "{PROJECT}-MITIGATION-MEASURES.md"
 description: "Mitigation Measurerecommendationandimplementation roadmapfigure"

 primary_sources:
 - P7: mitigation_plan

 structure:
 - section: "1. mitigationpriority matrix"
 subsections:
 - "1.1 byprioritylevelgroupstatistics"
 - "1.2 riskreducelowestimate"

 - section: "2. P0 - immediatefixmeasure"
 per_measure:
 - "forThreat"
 - "currentstatus"
 - "recommendedcontrol"
 - "implementationcodeExample"
 - "implementation steps"
 - "depend onitem"

 - section: "3. P1 - urgentmeasure"
 content: "same P0 Format"

 - section: "4. P2 - highprioritylevelmeasure"
 content: "same P0 Format"

 - section: "5. P3 - mediumprioritylevelmeasure"
 content: "same P0 Format"

 - section: "6. implementation roadmapfigure"
 subsections:
 - "6.1 byprioritylevelsort"
 - "6.2 depend onrelationship"
 - "6.3 defense in depthdeeparchitecturefigure"

 - section: "7. compliancemapping"
 content: "measureandcomplianceframeworktoshouldtable"

 notes:
 - "onlycontainprioritylevel，notcontainwhentime estimate"
 - "codeExamplemustiscandirectuseproductioncode"

 length_estimate: "20-40 page"
```

#### report 6: compliancereport (COMPLIANCE-REPORT)

```yaml
report_compliance:
 id: "compliance"
 filename: "{PROJECT}-COMPLIANCE-REPORT.md"
 description: "riskandcomplianceframework mappinganalysis"

 primary_sources:
 - P5: threat_inventory
 - P7: compliance_mapping

 structure:
 - section: "1. complianceOverview"
 subsections:
 - "1.1 applicable complianceframework"
 - "1.2 compliance gap summaryneed"

 - section: "2. OWASP Top 10 mapping"
 content: "riskand OWASP Top 10 toshouldtable"

 - section: "3. OWASP LLM Top 10 mapping"
 condition: "onlywhenprojectcontain AI/LLM componentwhen"

 - section: "4. CWE mapping"
 content: "by CWE group riskchecklist"

 - section: "5. NIST CSF mapping"
 content: "controlmeasureand NIST controltoshould"

 - section: "6. gapanalysis"
 subsections:
 - "6.1 notcoverage control"
 - "6.2 partialimplementation control"
 - "6.3 recommendationimprovement"

 length_estimate: "15-25 page"
```

#### report 7: attackvalidation (ATTACK-PATH-VALIDATION)

```yaml
report_attack:
 id: "attack"
 filename: "{PROJECT}-ATTACK-PATH-VALIDATION.md"
 description: "attack pathvalidationand POC method"

 primary_sources:
 - P6: validated_threats
 - P6: attack_paths

 structure:
 - section: "1. validationOverview"
 subsections:
 - "1.1 validationrange"
 - "1.2 validationmethod"
 - "1.3 resultsummaryneed"

 - section: "2. alreadyvalidationattack path"
 per_attack:
 - "attack path ID"
 - "targetThreat"
 - "attackchaindescription"
 - "prerequisitescondition"
 - "validationstep"
 - "POC method/command"
 - "validationresult"
 - "canexploitpropertyassessment"

 - section: "3. attack surfaceanalysis"
 subsections:
 - "3.1 externalattack surface"
 - "3.2 internalattack surface"
 - "3.3 highriskininterfacepoint"

 - section: "4. attackchaincanvisualization"
 content: "attackchainfigure (ASCII/Mermaid)"

 - section: "5. excludeitem"
 content: "validationexcluded false positives"

 length_estimate: "15-30 page"
```

#### report 8: penetrationtestplan (PENETRATION-TEST-PLAN)

```yaml
report_pentest:
 id: "pentest"
 filename: "{PROJECT}-PENETRATION-TEST-PLAN.md"
 description: "completepenetrationtestplanandtestuse case"

 primary_sources:
 - P6: validated_threats
 - P7: mitigation_plan

 structure:
 - section: "1. testOverview"
 subsections:
 - "1.1 testtarget"
 - "1.2 testrange"
 - "1.3 authorizationdeclaration"

 - section: "2. technologyarchitectureanalysis"
 content: "from attack perspectivearchitecturefigure"

 - section: "3. penetrationtestuse case"
 per_vulnerability:
 - "vulnerabilitytechnologyanalysis"
 - "attack vectoranalysis"
 - "testuse case (TC-XXX-NNN)"
 - "test Payload"
 - "expectedresult"
 - "determinationstandard"
 - "testmatrix"

 - section: "4. testenvironmentprepare"
 subsections:
 - "4.1 isolationtestenvironment"
 - "4.2 testdataprepare"
 - "4.3 monitoringconfiguration"

 - section: "5. riskassessmentandmitigation"
 subsections:
 - "5.1 testriskmatrix"
 - "5.2 testterminatecondition"
 - "5.3 Findingreportprocess"

 - section: "appendix"
 subsections:
 - "A. CVSS scoring"
 - "B. related CVE"

 classification: "confidential - securitytest"
 length_estimate: "25-50 page"
```

---

## 4. outputlocationstandardize

### 4.1 Directory Structure

```yaml
output_locations:
 # finalreportdirectory
 final_reports:
 path: "{PROJECT_ROOT}/Risk_Assessment_Report/"
 description: "riskassessmentreportdirectory"
 use_case: "Phase 8 generatedformal deliverable"
 auto_create: true

 # Phaseartifactdirectory (persist)
 phase_outputs:
 path: "{PROJECT_ROOT}/Risk_Assessment_Report/.phase_working/"
 description: "Phasemediumbetweenartifact (persistworkdirectory)"
 use_case: "Phase 1-7 output，supportsessionrecoveryandaudit traceability"
 auto_create: true
 hidden: true # hiddendirectory，notworktodeliverableitem

 # archivedirectory
 archive:
 path: "{PROJECT_ROOT}/Risk_Assessment_Report/archive/{VERSION}/"
 description: "archivedirectory"
 use_case: "historyVersionarchive"
```

### 4.2 outputTypedistinction

| Type | location | filenameFormat | Description |
|------|------|-----------|------|
| **finalreport** | `Risk_Assessment_Report/` | `{PROJECT}-{REPORT_TYPE}.md` | Phase 8 outputformal deliverable |
| **Phaseartifact** | `Risk_Assessment_Report/.phase_working/` | `P{N}-{PHASE_NAME}.md` | Phase 1-7 persistmediumintermediate results |
| **archivereport** | `Risk_Assessment_Report/archive/` | `{PROJECT}-{REPORT_TYPE}.md` | historyVersionarchive |

### 4.3 Phaseartifactstandardize

**designprinciple**: phase artifacts must be persisted to prevent context loss, session interruption, information not complete.

**cachepolicy**: single copyrootcache，onlypreservecurrent/latest onetimeanalysissession Phaseartifact

**Phaseartifactfilelist**:
| Phase | filename | maincontent |
|-------|--------|---------|
| - | `_session_meta.yaml` | sessionelementdata (required) |
| P1 | `P1-PROJECT-UNDERSTANDING.md` | projectcontext、technologystack、securityrelatedmodule |
| P2 | `P2-DFD-ANALYSIS.md` | DFDelementchecklist、datacategory、Mermaidsource code |
| P3 | `P3-TRUST-BOUNDARY.md` | trust boundarydefinition、edgeboundary crossing、keyinterface |
| P4 | `P4-SECURITY-DESIGN-REVIEW.md` | 9domainassessmentmatrix、securityFinding、design flaw |
| P5 | `P5-STRIDE-ANALYSIS.md` | STRIDEmatrix、Threatchecklist、KBqueryrecord |
| P6 | `P6-RISK-VALIDATION.md` | validationresult、attack path、POCmethod、false positive exclusion |
| P7 | `P7-MITIGATION-PLANNING.md` | mitigation plan, priority matrix, defense architecture |

**sessionelementdata (_session_meta.yaml)** - required:
```yaml
session:
 project_name: "N8N" # project name
 session_id: "20251230-100000" # sessionID (YYYYMMDD-HHMMSS)
 started_at: "2025-12-30T10:00:00+08:00" # ISO 8601 whenbetweenstamp
 last_updated: "2025-12-30T14:32:15+08:00" # Last Updatedwhenbetween
 framework_version: "v2.1.0" # frameworkVersion
 analyst: "Claude (Deep Risk Analysis)"
 status: "in_progress" # in_progress | completed | failed

phases:
 P1: { status: "completed", completed_at: "2025-12-30T10:15:32+08:00" }
 P2: { status: "completed", completed_at: "2025-12-30T10:45:18+08:00" }
 P3: { status: "in_progress", started_at: "2025-12-30T11:00:00+08:00" }
 P4: { status: "pending" }
 P5: { status: "pending" }
 P6: { status: "pending" }
 P7: { status: "pending" }
 P8: { status: "pending" }
```

**Phaseartifactfileheader (YAML front matter)**:
```yaml
---
phase: 1
name: "PROJECT-UNDERSTANDING"
project: "N8N"
session_id: "20251230-100000"
completed_at: "2025-12-30T10:15:32+08:00"
framework_version: "v2.1.0"
---
```

**cachemanagementrule**:
| scenario | rowto |
|------|------|
| newsession + directorynotexistin | createdirectory，initialization `_session_meta.yaml` |
| newsession + sameoneproject | prompt：continueuptimesession / overwrite restart |
| newsession + notsameproject | cleardirectory，startnewsession |
| sessioncompletionafter | preserve `.phase_working/` worktoaudit record |

### 4.4 completeoutputExample

```
project-root/
├── Risk_Assessment_Report/ # ✅ reportoutputdirectory
│ │
│ │ ┌─ required report (4copy) ──────────────────────────────────────────────┐
│ ├── N8N-RISK-ASSESSMENT-REPORT.md # riskassessmentreport (mainreport)
│ ├── N8N-RISK-INVENTORY.md # riskchecklist
│ ├── N8N-MITIGATION-MEASURES.md # Mitigation Measure
│ ├── N8N-PENETRATION-TEST-PLAN.md # penetrationtestsolution
│ │ └──────────────────────────────────────────────────────────────┘
│ │
│ │ ┌─ Optionalreport (byneedgenerate) ─────────────────────────────────────────┐
│ ├── N8N-ARCHITECTURE-ANALYSIS.md # architectureanalysis
│ ├── N8N-DFD-DIAGRAM.md # DFDfigure
│ ├── N8N-COMPLIANCE-REPORT.md # compliancereport
│ ├── N8N-ATTACK-PATH-VALIDATION.md # attack pathvalidation
│ │ └──────────────────────────────────────────────────────────────┘
│ │
│ │ ┌─ phase process document (automaticrelease，preserveEnglish name) ─────────────────────────┐
│ ├── P1-PROJECT-UNDERSTANDING.md # Phase 1 projectunderstanding
│ ├── P2-DFD-ANALYSIS.md # Phase 2 DFDanalysis
│ ├── P3-TRUST-BOUNDARY.md # Phase 3 trust boundary
│ ├── P4-SECURITY-DESIGN-REVIEW.md # Phase 4 securitydesignassessment
│ ├── P5-STRIDE-THREATS.md # Phase 5 STRIDEThreatanalysis
│ ├── P6-RISK-VALIDATION.md # Phase 6 riskvalidation
│ │ └──────────────────────────────────────────────────────────────┘
│ │
│ ├── .phase_working/ # Phaseworkdirectory (hidden)
│ │ ├── P1-PROJECT-UNDERSTANDING.md
│ │ ├── P2-DFD-ANALYSIS.md
│ │ ├── P3-TRUST-BOUNDARY.md
│ │ ├── P4-SECURITY-DESIGN-REVIEW.md
│ │ ├── P5-STRIDE-THREATS.md
│ │ ├── P6-RISK-VALIDATION.md
│ │ ├── P7-MITIGATION-PLAN.md
│ │ └── _session_meta.yaml # sessionelementdata
│ │
│ └── archive/ # archivedirectory
│ └── v1.0.0/
│ └── N8N-RISK-ASSESSMENT-REPORT.md
└── src/ # projectsourcecode (byassessmenttoobject)
```

### 4.5 namingvalidationrule

```yaml
naming_validation:
 # === finalreportnamingrule (Risk_Assessment_Report/) ===
 final_report:
 project_name:
 regex: "^[A-Z][A-Z0-9-]{0,29}$"
 description: "starts with uppercase, can contain uppercase letters, numbers, hyphens"
 max_length: 30
 examples:
 valid: ["N8N", "OPEN-WEBUI", "MY-PROJECT-V2"]
 invalid: ["n8n", "my_project", "MyProject"]

 report_type:
 allowed_values:
 - "RISK-ASSESSMENT-REPORT"
 - "RISK-INVENTORY"
 - "MITIGATION-MEASURES"
 - "PENETRATION-TEST-PLAN"
 - "ARCHITECTURE-ANALYSIS"
 - "DFD-DIAGRAM"
 - "COMPLIANCE-REPORT"
 - "ATTACK-PATH-VALIDATION"

 file_pattern:
 regex: "^[A-Z][A-Z0-9-]{0,29}-(RISK-ASSESSMENT-REPORT|RISK-INVENTORY|MITIGATION-MEASURES|PENETRATION-TEST-PLAN|ARCHITECTURE-ANALYSIS|DFD-DIAGRAM|COMPLIANCE-REPORT|ATTACK-PATH-VALIDATION)\\.md$"

 # === Phaseartifactnamingrule (.phase_working/) ===
 phase_output:
 file_pattern:
 regex: "^P[1-7]-[A-Z][A-Z0-9-]+\\.md$"
 description: "P{N}-{PHASE_NAME}.md Format"
 allowed_files:
 - "P1-PROJECT-UNDERSTANDING.md"
 - "P2-DFD-ANALYSIS.md"
 - "P3-TRUST-BOUNDARY.md"
 - "P4-SECURITY-DESIGN-REVIEW.md"
 - "P5-STRIDE-ANALYSIS.md"
 - "P6-RISK-VALIDATION.md"
 - "P7-MITIGATION-PLANNING.md"
 - "_session_meta.yaml"

 # === Prohibitednamingpattern (onlysuitableused forfinalreportdirectory) ===
 forbidden_in_final_reports:
 - "P{N}-*.md" # Phasenumberstart → shouldin .phase_working/
 - "PHASE{N}-*.md" # PHASE start
 - "*-PHASE-*.md" # contain PHASE
 - "phase*.md" # lowercase phase
 - "*_*.md" # underscore separated
```

---

## 5. reportelementdata

### 5.1 genericelementdataFormat

```yaml
# eachcopyreportheadermustcontain
report_metadata:
 project_name: string
 report_type: string
 version: string
 assessment_datetime: "YYYY-MM-DD HH:MM:SS"
 analyst: "Claude (Deep Risk Analysis)"
 framework_version: "STRIDE-TM v1.0.2"

 # statisticssummaryneed (according toreportType)
 summary:
 total_threats: integer # totalThreatcount
 critical: integer # Critical quantity
 high: integer # High quantity
 medium: integer # Medium quantity
 low: integer # Low quantity
```

### 5.2 Markdown elementdatablocktemplate

```markdown
# {reportType}: {PROJECT}

> **assessment time**: YYYY-MM-DD HH:MM:SS
> **analysisexpert**: Claude (Deep Risk Analysis)
> **frameworkVersion**: STRIDE-TM v1.0.2
> **report version**: 1.0

---
```

---

## 6. reportgeneraterule

### 6.1 required reports

The following reports are generated in each completeassessmentmediummustgenerate:

| report | condition |
|------|------|
| RISK-ASSESSMENT-REPORT | alwaysgenerate |
| RISK-INVENTORY | alwaysgenerate |
| MITIGATION-MEASURES | alwaysgenerate |
| PENETRATION-TEST-PLAN | alwaysgenerate (based onP6riskvalidationdata) |

### 6.2 Optionalreport

followingreportaccording toconditiongenerate:

| report | generatecondition |
|------|---------|
| ARCHITECTURE-ANALYSIS | user requestorcomplexsystem |
| DFD-DIAGRAM | user requestor DFD element > 10 |
| COMPLIANCE-REPORT | user requestorhavecomplianceneedrequire |
| ATTACK-PATH-VALIDATION | have Critical/High Threat |

### 6.3 reportbetweendepend on

```
┌─────────────────────────────────────────────────────────────────┐
│ Report Dependencies │
├─────────────────────────────────────────────────────────────────┤
│ │
│ RISK-ASSESSMENT-REPORT (mainreport) │
│ │ │
│ ├──→ reference ARCHITECTURE-ANALYSIS (section 2) │
│ ├──→ reference DFD-DIAGRAM (section 2.2, appendix A/B) │
│ ├──→ reference RISK-INVENTORY (section 4) │
│ ├──→ reference MITIGATION-MEASURES (section 6) │
│ └──→ reference COMPLIANCE-REPORT (section 7) │
│ │
│ independentreport: │
│ - ATTACK-PATH-VALIDATION │
│ - PENETRATION-TEST-PLAN │
│ │
└─────────────────────────────────────────────────────────────────┘
```

---

## 7. Versionhistory

| Version | date | changeDescription |
|------|------|---------|
| 1.4.0 | 2025-12-31 | penetration test solution upgraded to required report; phase process document release preserve English name; regroup report type table |
| 1.3.0 | 2025-12-30 | single copyrootcachepolicy：addwhenbetweenstamp/Versionelementdata，definition `_session_meta.yaml` andfileheaderstandardize |
| 1.2.0 | 2025-12-30 | Phaseartifactpersistdesign：add `.phase_working/` directory，definition P{N}-{NAME}.md Naming Convention |
| 1.1.0 | 2025-12-30 | updateoutputdirectoryto `Risk_Assessment_Report/`，addnamingvalidationrule，distinctionPhaseartifactandfinalreport |
| 1.0.0 | 2025-12-26 | initialVersion，definition 8 typesreportNaming Convention |

---

**documentend**
