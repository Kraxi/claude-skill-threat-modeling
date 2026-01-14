<!-- Code-First Deep Threat Modeling Workflow | Version 2.1.0 | https://github.com/fr33d3m0n/skill-threat-modeling | License: BSD-3-Clause | Welcome to cite but please retain all sources and declarations -->

# PenetrationTestingPlan: {PROJECT_NAME}

> **DocumentationVersion**: {REPORT_VERSION}
> **CreateDate**: {ASSESSMENT_DATETIME}
> **Category**: Confidential - SecurityTesting
> **AuthorizationScope**: OnlyLimitAuthorizationTestingEnvironment
> **ValidPeriod**: {VALIDITY_PERIOD}

---

## 1. TestingOverview

### 1.1 TestingTarget

| VulnerabilityNumber | Name | CVSS | STRIDE | ATT&CK | Target Component | Priority |
|----------|------|------|--------|--------|----------|--------|
{TEST_TARGETS_TABLE}
<!--
Format:
| V-001 | JWTWeakKey | 8.8 | S | T1078 | packages/cli/src/auth/ | P0 |
| V-002 | SQLInjection | 9.8 | T | T1190 | packages/core/src/db/ | P0 |
| V-003 | PermissionBypass | 7.5 | E | T1548 | packages/cli/src/authz/ | P1 |
-->

### 1.2 TestingScope

| Scope | Include | Exclude |
|------|------|------|
| ApplicationLayer | {APP_SCOPE_INCLUDE} | {APP_SCOPE_EXCLUDE} |
| NetworkLayer | {NET_SCOPE_INCLUDE} | {NET_SCOPE_EXCLUDE} |
| DataLayer | {DATA_SCOPE_INCLUDE} | {DATA_SCOPE_EXCLUDE} |

### 1.3 AuthorizationDeclaration

```
┌─────────────────────────────────────────────────────────────────────────────┐
│ AUTHORIZATION STATEMENT │
├─────────────────────────────────────────────────────────────────────────────┤
│ │
│ ThisPenetrationTestingPlanOnlyAuthorizationAtThe followingScopeWithinExecution: │
│ │
│ - TestingEnvironment: {AUTHORIZED_ENVIRONMENT} │
│ - TestingTime: {AUTHORIZED_TIMEFRAME} │
│ - AuthorizationPersonnel: {AUTHORIZED_PERSONNEL} │
│ │
│ Prohibited activitiesItem: │
│ - ❌ AtProductionEnvironmentExecutionAnyTesting │
│ - ❌ ForNon-AuthorizationTargetProceedActionTesting │
│ - ❌ ExecutionLikelyCauseServiceInterruptTesting │
│ - ❌ NotWith permissionCanDisclosureTestingResult │
│ │
│ AuthorizationSignature: ________________________ Date: _______________ │
│ │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. TechnologyArchitecture Analysis

### 2.1 SystemArchitectureDiagram

```
{SYSTEM_ARCHITECTURE_ASCII}
```
<!--
Example:
┌─────────────────────────────────────────────────────────────────────────────┐
│ Target System Architecture │
├─────────────────────────────────────────────────────────────────────────────┤
│ │
│ Internet │
│ │ │
│ [WAF] ◄──────┤ │
│ │ │
│ ═══════════════════════════════╪════════════════════════════════════════ │
│ │ DMZ │
│ ═══════════════════════════════╪════════════════════════════════════════ │
│ │ │
│ ┌────────────────────┼────────────────────┐ │
│ │ │ │ │
│ ▼ ▼ ▼ │
│ ┌─────────┐ ┌─────────┐ ┌─────────┐ │
│ │ Web UI │◄───────►│ API │◄───────►│ Worker │ │
│ │:443 │ │:8080 │ │:3000 │ │
│ │ [V-003] │ │[V-001,2]│ │ [V-004] │ │
│ └─────────┘ └────┬────┘ └─────────┘ │
│ │ │
│ ═══════════════════════════════╪════════════════════════════════════════ │
│ │ Internal │
│ ═══════════════════════════════╪════════════════════════════════════════ │
│ │ │
│ ┌────────────────────┼────────────────────┐ │
│ ▼ ▼ ▼ │
│ ┌─────────┐ ┌─────────┐ ┌─────────┐ │
│ │ Redis │ │PostgreSQL│ │ S3/Minio│ │
│ │:6379 │ │:5432 │ │:9000 │ │
│ │ [V-005] │ │[V-002,6]│ │ [V-007] │ │
│ └─────────┘ └─────────┘ └─────────┘ │
│ │
│ [V-XXX] = VulnerabilityNumber │
│ │
└─────────────────────────────────────────────────────────────────────────────┘
-->

### 2.2 Attack PathCanVisualization

```
{ATTACK_PATHS_VISUALIZATION}
```
<!--
Example:
┌─────────────────────────────────────────────────────────────────────────────┐
│ Planned Attack Paths │
├─────────────────────────────────────────────────────────────────────────────┤
│ │
│ Path 1: Authentication Bypass │
│ ════════════════════════════ │
│ │
│ [Attacker] ───► [Login API] ───► [JWT Analysis] ───► [Key Crack] ───► │
│ │
│ ───► [Token Forge] ───► [Admin Access] ───► [Data Exfil] │
│ │
│ Path 2: SQL Injection │
│ ═══════════════════════ │
│ │
│ [Attacker] ───► [Search API] ───► [SQL Inject] ───► [DB Dump] ───► │
│ │
│ ───► [Credential Harvest] ───► [Lateral Movement] │
│ │
└─────────────────────────────────────────────────────────────────────────────┘
-->

---

## 3. PenetrationTestingUseExample

{PENETRATION_TEST_CASES_SECTION}

<!--
=============================================================================
PenetrationTestingUseExampleTemplate (EachVulnerabilityOneCompleteSection)
=============================================================================

### 3.X V-{XXX}: {VULNERABILITY_NAME}

#### 3.X.1 VulnerabilityTechnologyAnalysis

**Basic Information**:

| Property | Value |
|------|-----|
| VulnerabilityNumber | V-{XXX} |
| Related Threats | T-{STRIDE}-{ELEMENT}-{SEQ} |
| CVSSScore | {CVSS_SCORE} |
| Severity | {SEVERITY} |
| ImpactComponent | {AFFECTED_COMPONENT} |
| ATT&CK ID | {ATTACK_TECHNIQUE_ID} |
| ATT&CK Tactic | {ATTACK_TACTIC} |

**SourceCodeKeyLocation**:

| File | Line Number | Function | SecurityRisk |
|------|------|------|---------|
| `{FILE_1}` | L{START}-L{END} | {FUNCTION_1} | {RISK_1} |
| `{FILE_2}` | L{START}-L{END} | {FUNCTION_2} | {RISK_2} |

**IssueCode**:

```{CODE_LANGUAGE}
// {FILE_PATH}:L{LINE_NUMBER}
{VULNERABLE_CODE}
```

#### 3.X.2 Attack VectorAnalysis

**Vector A: {VECTOR_A_NAME}**

| Property | Value |
|------|-----|
| AttackComplexity | {COMPLEXITY} |
| RequiredPermission | {PRIVILEGES} |
| UserInteraction | {USER_INTERACTION} |
| SuccessRateEstimate | {SUCCESS_RATE}% |

**AttackStep**:

1. {ATTACK_STEP_1}
2. {ATTACK_STEP_2}
3. {ATTACK_STEP_3}

**Vector B: {VECTOR_B_NAME}**

[Same as aboveFormat...]

#### 3.X.3 PenetrationTestingUseExample

##### TC-{XXX}-001: {TEST_CASE_NAME}

**Target**: {TEST_OBJECTIVE}

**Prerequisites**:

- [] {PRECONDITION_1}
- [] {PRECONDITION_2}
- [] {PRECONDITION_3}

**TestingStep**:

```{SCRIPT_LANGUAGE}
# Step 1: {STEP_1_DESC}
{STEP_1_CODE}

# Step 2: {STEP_2_DESC}
{STEP_2_CODE}

# Step 3: {STEP_3_DESC}
{STEP_3_CODE}
```

**Testing Payload**:

```
Payload 1 - {PAYLOAD_1_DESC}:
{PAYLOAD_1}

Payload 2 - {PAYLOAD_2_DESC}:
{PAYLOAD_2}

Payload 3 - {PAYLOAD_3_DESC}:
{PAYLOAD_3}
```

**Expected Result**: {EXPECTED_RESULT}

**ActualResult**: `[PendingFill in]`

**DeterminationStandard**:

- ✅ PASS: {PASS_CRITERIA}
- ❌ FAIL: {FAIL_CRITERIA}

**CutoffDiagram/Evidence**: `[PendingFill in]`

---

##### TC-{XXX}-002: {TEST_CASE_NAME_2}

[Same as aboveFormat...]

---

#### 3.X.4 TestingMatrix

| TestingUseExample | Attack Vector | Priority | RiskLevel | Status |
|----------|----------|--------|---------|------|
| TC-{XXX}-001 | {VECTOR_A} | P0 | Critical | ⬜ PendingTesting |
| TC-{XXX}-002 | {VECTOR_B} | P1 | High | ⬜ PendingTesting |

---

-->

---

## 4. TestingEnvironmentPrepare

### 4.1 IsolationTestingEnvironment

```yaml
# docker-compose.test.yml
version: '3.8'
services:
{DOCKER_COMPOSE_CONFIG}
```
<!--
Example:
 target-app:
 image: ${PROJECT_IMAGE}:${TEST_VERSION}
 ports:
 -"8080:8080"
 environment:
 - NODE_ENV=test
 - DB_HOST=postgres
 - REDIS_HOST=redis
 depends_on:
 - postgres
 - redis
 networks:
 - pentest-network

 postgres:
 image: postgres:15
 environment:
 - POSTGRES_DB=testdb
 - POSTGRES_USER=testuser
 - POSTGRES_PASSWORD=testpass
 volumes:
 -./init-test-db.sql:/docker-entrypoint-initdb.d/init.sql
 networks:
 - pentest-network

 redis:
 image: redis:7
 networks:
 - pentest-network

networks:
 pentest-network:
 driver: bridge
-->

### 4.2 TestingDataPrepare

```sql
-- init-test-db.sql
{TEST_DATA_SQL}
```
<!--
Example:
-- CreateTestingUser
INSERT INTO users (id, username, email, password_hash, role) VALUES
(1, 'test_user', 'test@example.com', '$2a$10$...', 'user'),
(2, 'test_admin', 'admin@example.com', '$2a$10$...', 'admin');

-- CreateTestingData
INSERT INTO sensitive_data (id, user_id, content) VALUES
(1, 1, 'Test sensitive data 1'),
(2, 1, 'Test sensitive data 2');

-- CreateTestingAPIKey
INSERT INTO api_keys (id, user_id, key_hash, scope) VALUES
(1, 1, '$2a$10$...', 'read'),
(2, 2, '$2a$10$...', 'admin');
-->

### 4.3 MonitoringConfiguration

```yaml
# prometheus.yml
{MONITORING_CONFIG}
```
<!--
Example:
global:
 scrape_interval: 5s

scrape_configs:
 - job_name: 'pentest-target'
 static_configs:
 - targets: ['target-app:8080']
 metrics_path: /metrics

 - job_name: 'pentest-traffic'
 static_configs:
 - targets: ['traffic-monitor:9090']
-->

### 4.4 TestingToolConfiguration

| Tool | ConfigurationFile | UsePath |
|------|---------|------|
{TOOLS_CONFIG_TABLE}
<!--
Format:
| Burp Suite | burp-project.json | WebApplicationTesting |
| sqlmap | sqlmap.conf | SQLInjectionFromAutomation |
| jwt-tool | jwt-config.json | JWTTesting |
-->

---

## 5. RiskAssessment and Mitigation

### 5.1 TestingRiskMatrix

| RiskType | Description | Likelihood | Impact | Mitigation Measures |
|----------|------|--------|------|---------|
| ServiceInterrupt | TestingCauseTargetServiceCrash | Medium | High | UsageIsolationEnvironment，MonitoringServiceStatus |
| DataDamage | SQLInjectionTestingDamageTestingData | Medium | Medium | UsageDataSnapshot，TestingBeforeBackup |
| InformationDisclosure | TestingArtifactIncludeSensitiveInformation | Low | High | TestingAfterCleanup，EncryptionStorage |
| ScopeExceed | IntentOutsideTestingNon-AuthorizationTarget | Low | High | StrictlyLimitDefineTargetIP/DomainName |
{ADDITIONAL_RISKS}

### 5.2 TestingTerminateCondition

| Condition | TriggerSituation | ProcessingMethod |
|------|---------|---------|
| 🔴 ImmediateTerminate | DetectionToProductionEnvironmentImpact | StopAllTesting，NotificationTeam |
| 🔴 ImmediateTerminate | FindingExceedAuthorizationScopeAccess | StopTesting，RecordAndReport |
| 🟠 PauseAssessment | ServiceResponseException | PauseTesting，CheckEnvironmentStatus |
| 🟠 PauseAssessment | FindingHighRiskVulnerability | PauseExtendedTesting，PriorityRecordPOC |
| 🟡 ContinueMonitoring | TestingTime Exceeds Expected Period | AssessmentProgress，AdjustPlan |

### 5.3 FindingReportProcess

```
┌─────────────────────────────────────────────────────────────────────────────┐
│ Discovery Reporting Flow │
├─────────────────────────────────────────────────────────────────────────────┤
│ │
│ ┌───────────┐ ┌───────────┐ ┌───────────┐ ┌───────────┐ │
│ │ FindingVulnerability │────►│ StopTesting │────►│ RecordReproduce │────►│ AssessmentCVSS │ │
│ └───────────┘ └───────────┘ │ Step │ └─────┬─────┘ │
│ └───────────┘ │ │
│ ▼ │
│ ┌───────────────────────────────────────────────┐ │
│ │ RiskAssessmentLevelJudgment │ │
│ └───────────────────────────────────────────────┘ │
│ │ │ │
│ ┌────────────┘ └────────────┐ │
│ ▼ ▼ │
│ ┌───────────────────────┐ ┌───────────────────────┐ │
│ │ Critical/High │ │ Medium/Low │ │
│ │ 48SmallWhenWithinReport │ │ TestingCompleteAfterSummaryReport │ │
│ └───────────┬───────────┘ └───────────┬───────────┘ │
│ │ │ │
│ ▼ ▼ │
│ ┌───────────────────────┐ ┌───────────────────────┐ │
│ │ PreparePOC │ │ RecordToTestingReport │ │
│ │ NotificationSecurityTeam │ │ ContinueOtherTesting │ │
│ └───────────────────────┘ └───────────────────────┘ │
│ │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 6. ExecutionPlan

### 6.1 TestingPhase

| Phase | Content | EstimatedTime | Dependency |
|------|------|---------|------|
| Prepare | EnvironmentBuild，ToolConfiguration | {PREP_TIME} | None |
| Reconnaissance | InformationCollect，Attack SurfaceAnalysis | {RECON_TIME} | PrepareComplete |
| Testing | ExecutionTestingUseExample | {TEST_TIME} | ReconnaissanceComplete |
| Validation | POCValidation，Reproduce | {VERIFY_TIME} | TestingComplete |
| Report | OrganizeReport，Recommendation | {REPORT_TIME} | ValidationComplete |

### 6.2 TestingUseExampleExecutionSequential

| Sequential | VulnerabilityNumber | TestingUseExample | Priority | EstimatedTime |
|------|---------|---------|--------|---------|
{TEST_EXECUTION_ORDER}
<!--
Format:
| 1 | V-001 | TC-001-001, TC-001-002 | P0 | 2h |
| 2 | V-002 | TC-002-001 | P0 | 1.5h |
| 3 | V-003 | TC-003-001, TC-003-002, TC-003-003 | P1 | 3h |
-->

### 6.3 Resource Requirements

| ResourceSource | Requirement | Description |
|------|------|------|
| TestingPersonnel | {PERSONNEL_COUNT} | {PERSONNEL_SKILLS} |
| TestingEnvironment | {ENV_SPECS} | {ENV_NOTES} |
| ToolAllowCan | {TOOL_LICENSES} | {LICENSE_NOTES} |

---

## Appendices

### Appendices A: CVSS ScoreDetails

| VulnerabilityNumber | CVSSVector | Score |
|----------|---------|------|
{CVSS_DETAILS_TABLE}
<!--
Format:
| V-001 | CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:N | 8.8 |
| V-002 | CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H | 9.8 |
-->

### Appendices B: Related CVE/CWE

| VulnerabilityNumber | CWE | RelatedCVE | Reference |
|----------|-----|---------|------|
{CVE_CWE_TABLE}
<!--
Format:
| V-001 | CWE-347 | CVE-2023-XXXXX | https://... |
| V-002 | CWE-89 | CVE-2022-XXXXX | https://... |
-->

### Appendices C: TestingCheckList

```
[] TestingEnvironmentAlreadyIsolation
[] TestingDataAlreadyPrepare
[] MonitoringAlreadyConfiguration
[] ToolAlreadyConfiguration
[] AuthorizationAlreadyConfirm
[] BackupAlreadyCreate
[] TerminateConditionAlreadyUnderstanding
[] ReportProcessAlreadyUnderstanding
```

### Appendices D: ContactMethod

| Role | SurnameName | ContactMethod | Remark |
|------|------|---------|------|
| TestingOwner | {TEST_LEAD} | {TEST_LEAD_CONTACT} | |
| SecurityTeam | {SECURITY_TEAM} | {SECURITY_CONTACT} | UrgentSituation |
| DevelopmentTeam | {DEV_TEAM} | {DEV_CONTACT} | TechnologyIssue |
| OperationsTeam | {OPS_TEAM} | {OPS_CONTACT} | EnvironmentIssue |

### Appendices E: MITRE ATT&CK TechnologyMapping

#### E.1 STRIDE → ATT&CK MappingReference

| STRIDE | ATT&CK Tactic | Common Techniques | Description |
|--------|--------------|-----------------|------|
| **S**poofing | Initial Access | T1078 (Valid Accounts) | UsageValidCredentialsObtainAccess |
| | Initial Access | T1566 (Phishing) | through PhishingObtainCredentials |
| | Credential Access | T1110 (Brute Force) | Brute forceCrackAuthentication |
| | Credential Access | T1539 (Steal Web Session Cookie) | TheftSessionCredentials |
| **T**ampering | Impact | T1485 (Data Destruction) | DestructionDataCompleteCapability |
| | Impact | T1565 (Data Manipulation) | TamperingData |
| | Initial Access | T1190 (Exploit Public-Facing App) | ExploitUseApplicationVulnerability |
| | Persistence | T1505 (Server Software Component) | ImplantBackdoor |
| **R**epudiation | Defense Evasion | T1070 (Indicator Removal) | RemoveLogTrace |
| | Defense Evasion | T1036 (Masquerading) | MasqueradeActivity |
| | Defense Evasion | T1562 (Impair Defenses) | DisabledAuditFunction |
| **I**nfo Disclosure | Collection | T1005 (Data from Local System) | LocalDataCollect |
| | Collection | T1039 (Data from Network Shared Drive) | NetworkDataCollect |
| | Exfiltration | T1048 (Exfiltration Over Alternative Protocol) | DataLeakage |
| | Discovery | T1083 (File and Directory Discovery) | SensitiveFileFinding |
| **D**enial of Service | Impact | T1499 (Endpoint DoS) | EndpointDenialService |
| | Impact | T1498 (Network DoS) | NetworkDenialService |
| | Impact | T1489 (Service Stop) | ServiceStop |
| **E**levation | Privilege Escalation | T1068 (Exploitation for Privilege Escalation) | VulnerabilityPrivilege escalation |
| | Privilege Escalation | T1548 (Abuse Elevation Control Mechanism) | AbuseUsePrivilege escalationMechanism |
| | Privilege Escalation | T1134 (Access Token Manipulation) | TokenOperation |

#### E.2 ThisProject ATT&CK TechnologyList

| VulnerabilityNumber | ATT&CK ID | TechnologyName | Tactic | DetectionMethod |
|----------|-----------|---------|--------|---------|
{ATTACK_TECHNIQUES_TABLE}
<!--
Format:
| V-001 | T1078.001 | Valid Accounts: Default Accounts | Initial Access | MonitoringDefaultCredentialsUsage |
| V-002 | T1190 | Exploit Public-Facing Application | Initial Access | WAFLogAnalysis |
| V-003 | T1548.002 | Abuse Elevation Control Mechanism: Bypass UAC | Privilege Escalation | ProcessMonitoring |
-->

#### E.3 ATT&CK Attack ChainAnalysis

```
{ATTACK_CHAIN_DIAGRAM}
```
<!--
Example:
┌─────────────────────────────────────────────────────────────────────────────┐
│ ATT&CK Attack Chain Analysis │
├─────────────────────────────────────────────────────────────────────────────┤
│ │
│ Attack Chain 1: CredentialsTheft → PermissionElevation → DataLeakage │
│ ════════════════════════════════════════════ │
│ │
│ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ │
│ │ Initial │ │Credential│ │Privilege │ │Exfiltrat │ │
│ │ Access │───►│ Access │───►│Escalation│───►│ ion │ │
│ │ T1078 │ │ T1539 │ │ T1548 │ │ T1048 │ │
│ │ V-001 │ │ V-004 │ │ V-003 │ │ V-007 │ │
│ └──────────┘ └──────────┘ └──────────┘ └──────────┘ │
│ │
│ Attack Chain 2: ApplicationVulnerability → CodeExecution → Persistence │
│ ═════════════════════════════════════════ │
│ │
│ ┌──────────┐ ┌──────────┐ ┌──────────┐ │
│ │ Initial │ │Execution │ │Persistence│ │
│ │ Access │───►│ │───►│ │ │
│ │ T1190 │ │ T1059 │ │ T1505 │ │
│ │ V-002 │ │ V-005 │ │ V-006 │ │
│ └──────────┘ └──────────┘ └──────────┘ │
│ │
└─────────────────────────────────────────────────────────────────────────────┘
-->

#### E.4 ATT&CK Detection and ResponseRecommendation

| ATT&CK ID | DetectionDataSource | DetectionRuleExample | ResponseMeasure |
|-----------|-----------|-------------|---------|
{ATTACK_DETECTION_TABLE}
<!--
Format:
| T1078 | AuthenticationLog | ExceptionLoginLocation/Time | StrongSystemMFA，AccountLocked |
| T1190 | WAFLog | SQLInjection/XSSMode | BlockRequest，SecurityAlert |
| T1548 | ProcessMonitoring | PermissionElevationAttempt | TerminateProcess，SecurityAudit |
-->

---

**DocumentationEnd**

---

> **SecurityDeclaration**: ThisPenetrationTestingPlanIncludeSensitiveSecurityInformation，IncludeAttackMethod and POC Code。
> Please Strictly Limit AccessScope，OnlySupplyAuthorizationSecurityTestingPersonnelUsage。
> AllTestingActivityMustAtAuthorizationScopeWithinProceedAction。
> NotThroughAuthorizationDo not distribute or UseInNon-legal Purpose。
