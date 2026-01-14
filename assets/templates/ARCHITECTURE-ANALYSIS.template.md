<!-- Code-First Deep Threat Modeling Workflow | Version 2.1.0 | https://github.com/fr33d3m0n/skill-threat-modeling | License: BSD-3-Clause | Welcome to cite but please retain all sources and declarations -->

# Architecture AnalysisReport: {PROJECT_NAME}

> **Assessment Time**: {ASSESSMENT_DATETIME}
> **AnalysisAnalyst**: Claude (Deep Risk Analysis)
> **FrameworkVersion**: STRIDE-TM v1.0.2
> **ReportVersion**: {REPORT_VERSION}

---

## 1. Project Overview

### 1.1 Project Type and Goals

| Property | Value |
|------|-----|
| **Project Name** | {PROJECT_NAME} |
| **Project Type** | {PROJECT_TYPE} |
| **MainFunction** | {PRIMARY_FUNCTION} |
| **TargetUser** | {TARGET_USERS} |
| **DeploymentMode** | {DEPLOYMENT_MODE} |

**ProjectDescription**:

{PROJECT_DESCRIPTION}

### 1.2 Technology StackSummary

| LayerLevel | Technology Selection | Version | Description |
|------|---------|------|------|
| **ProgrammingLanguage** | {LANGUAGE} | {LANG_VERSION} | {LANG_NOTES} |
| **Runtime** | {RUNTIME} | {RUNTIME_VERSION} | {RUNTIME_NOTES} |
| **WebFramework** | {FRAMEWORK} | {FRAMEWORK_VERSION} | {FRAMEWORK_NOTES} |
| **ORM/DataLayer** | {ORM} | {ORM_VERSION} | {ORM_NOTES} |
| **Database** | {DATABASE} | {DB_VERSION} | {DB_NOTES} |
| **Cache** | {CACHE} | {CACHE_VERSION} | {CACHE_NOTES} |
| **MessageQueue** | {MQ} | {MQ_VERSION} | {MQ_NOTES} |
| **Container Tools** | {CONTAINER} | {CONTAINER_VERSION} | {CONTAINER_NOTES} |

### 1.3 Code Structure Overview

```
{CODEBASE_TREE}
```
<!--
Example:
project-root/
├── packages/
│ ├── cli/ # AfterEnd CLI and API
│ │ ├── src/
│ │ │ ├── controllers/ # API ControlTool
│ │ │ ├── services/ # BusinessLogic
│ │ │ ├── auth/ # AuthenticationModule ★
│ │ │ └── security/ # SecurityRelated ★
│ │ └── config/ # Configuration
│ ├── core/ # Core Engine
│ ├── editor-ui/ # BeforeEnd Vue Application
│ └── workflow/ # WorkflowDefinition
├── docker/ # Docker Configuration
└── scripts/ # Build Script

★ = Security-Related Modules
-->

**CodeStatistics**:

| Metric | Value |
|------|------|
| TotalFile Count | {TOTAL_FILES} |
| Lines of Code | {TOTAL_LOC} |
| Main Module Count | {MODULE_COUNT} |
| Security-Related Modules | {SECURITY_MODULES} |

---

## 2. ComponentTopology

### 2.1 HighLayerArchitectureDiagram

```
{HIGH_LEVEL_ARCHITECTURE_ASCII}
```
<!--
Example:
┌─────────────────────────────────────────────────────────────────────────────┐
│ External Layer │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ │
│ │ Browser │ │ Mobile App │ │ API Client │ │ Webhook │ │
│ └──────┬──────┘ └──────┬──────┘ └──────┬──────┘ └──────┬──────┘ │
└─────────┼────────────────┼────────────────┼────────────────┼────────────────┘
 │ │ │ │
 └────────────────┴────────────────┴────────────────┘
 │
 ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│ Edge Layer │
│ ┌─────────────────────────────────────────────────────────────────────┐ │
│ │ Load Balancer / CDN │ │
│ └─────────────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────────────┘
 │
 ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│ Application Layer │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ │
│ │ Web Server │ │ API Server │ │ Worker │ │ Scheduler │ │
│ └──────┬──────┘ └──────┬──────┘ └──────┬──────┘ └──────┬──────┘ │
│ │ │ │ │ │
│ └────────────────┴────────────────┴────────────────┘ │
│ │ │
└───────────────────────────────────┼──────────────────────────────────────────┘
 │
 ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│ Data Layer │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ │
│ │ PostgreSQL │ │ Redis │ │ S3/Storage │ │ Secrets │ │
│ └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘ │
└─────────────────────────────────────────────────────────────────────────────┘
-->

### 2.2 ModuleDependencyRelationship

```
{MODULE_DEPENDENCY_GRAPH}
```
<!--
Example:
┌─────────────────────────────────────────────────────────────────┐
│ Module Dependencies │
├─────────────────────────────────────────────────────────────────┤
│ │
│ editor-ui ──────────────────────────────────┐ │
│ │ │ │
│ ▼ ▼ │
│ api-types ◄─────── cli ◄─────── core ◄─────── workflow │
│ │ │ │ │
│ ▼ ▼ ▼ │
│ design-system db-models nodes-base │
│ │ │
│ ▼ │
│ config │
│ │
│ DiagramExample: A ──► B Table shows A Dependency B │
│ │
└─────────────────────────────────────────────────────────────────┘
-->

### 2.3 ExternalServiceIntegration

| ServiceType | ServiceName | IntegrationMethod | AuthenticationMethod | SecurityNote Items |
|---------|---------|---------|---------|-------------|
| {SVC_TYPE_1} | {SVC_NAME_1} | {INTEGRATION_1} | {AUTH_1} | {SECURITY_NOTE_1} |
| {SVC_TYPE_2} | {SVC_NAME_2} | {INTEGRATION_2} | {AUTH_2} | {SECURITY_NOTE_2} |
| {SVC_TYPE_3} | {SVC_NAME_3} | {INTEGRATION_3} | {AUTH_3} | {SECURITY_NOTE_3} |

---

## 3. Technology StackDetails

### 3.1 ProgrammingLanguage and Framework

#### MainLanguage

| Language | Version | UsageScope | Security Features |
|------|------|---------|---------|
| {LANG_1} | {VER_1} | {SCOPE_1} | {SECURITY_1} |
| {LANG_2} | {VER_2} | {SCOPE_2} | {SECURITY_2} |

#### Framework

| Framework | Version | UsePath | Built-inSecurityFunction |
|------|------|------|------------|
| {FW_1} | {FW_VER_1} | {FW_USE_1} | {FW_SECURITY_1} |
| {FW_2} | {FW_VER_2} | {FW_USE_2} | {FW_SECURITY_2} |

### 3.2 Database and Storage

| Type | Technology | Version | UsePath | EncryptionStatus |
|------|------|------|------|---------|
| Main Database | {MAIN_DB} | {MAIN_DB_VER} | {MAIN_DB_USE} | {MAIN_DB_ENC} |
| Cache | {CACHE_DB} | {CACHE_DB_VER} | {CACHE_DB_USE} | {CACHE_DB_ENC} |
| FileStorage | {FILE_STORE} | {FILE_STORE_VER} | {FILE_STORE_USE} | {FILE_STORE_ENC} |

### 3.3 MessageQueue and Cache

| Component | Technology | Version | Configuration | SecurityConfiguration |
|------|------|------|------|---------|
| {MQ_COMPONENT_1} | {MQ_TECH_1} | {MQ_VER_1} | {MQ_CONFIG_1} | {MQ_SECURITY_1} |
| {MQ_COMPONENT_2} | {MQ_TECH_2} | {MQ_VER_2} | {MQ_CONFIG_2} | {MQ_SECURITY_2} |

### 3.4 SecurityRelatedComponent

| ComponentType | Library/Service | Version | ConfigurationLocation | Status |
|---------|--------|------|---------|------|
| Authentication | {AUTH_LIB} | {AUTH_VER} | `{AUTH_PATH}` | {AUTH_STATUS} |
| Encryption | {CRYPTO_LIB} | {CRYPTO_VER} | `{CRYPTO_PATH}` | {CRYPTO_STATUS} |
| KeyManagement | {KMS_LIB} | {KMS_VER} | `{KMS_PATH}` | {KMS_STATUS} |
| Log | {LOG_LIB} | {LOG_VER} | `{LOG_PATH}` | {LOG_STATUS} |
| Audit | {AUDIT_LIB} | {AUDIT_VER} | `{AUDIT_PATH}` | {AUDIT_STATUS} |

---

## 4. Security-Related Modules

### 4.1 AuthenticationModule

**Location**: `{AUTH_MODULE_PATH}`

**ImplementationMethod**: {AUTH_IMPLEMENTATION}

**AuthenticationMechanism**:
| Mechanism | Implementation Status | Configuration |
|------|---------|------|
| UserName/Password | {USERPASS_STATUS} | {USERPASS_CONFIG} |
| OAuth 2.0 | {OAUTH_STATUS} | {OAUTH_CONFIG} |
| SAML | {SAML_STATUS} | {SAML_CONFIG} |
| LDAP | {LDAP_STATUS} | {LDAP_CONFIG} |
| MFA/2FA | {MFA_STATUS} | {MFA_CONFIG} |
| API Key | {APIKEY_STATUS} | {APIKEY_CONFIG} |
| JWT | {JWT_STATUS} | {JWT_CONFIG} |

**KeyCode Location**:
{AUTH_CODE_LOCATIONS}
<!--
Format:
- `src/auth/jwt.service.ts:L45-L120` - JWT Signature and Validation
- `src/auth/password.ts:L20-L80` - Password Hash
-->

### 4.2 AuthorizationModule

**Location**: `{AUTHZ_MODULE_PATH}`

**ImplementationMethod**: {AUTHZ_IMPLEMENTATION}

**PermissionModel**:
| ModelType | Implementation Status | Description |
|---------|---------|------|
| RBAC | {RBAC_STATUS} | {RBAC_NOTES} |
| ABAC | {ABAC_STATUS} | {ABAC_NOTES} |
| ACL | {ACL_STATUS} | {ACL_NOTES} |

**KeyCode Location**:
{AUTHZ_CODE_LOCATIONS}

### 4.3 EncryptionModule

**Location**: `{CRYPTO_MODULE_PATH}`

**UsageEncryptionAlgorithm**:
| UsePath | Algorithm | Key Length | Status |
|------|------|---------|------|
| DataEncryption | {DATA_ENC_ALG} | {DATA_ENC_KEY} | {DATA_ENC_STATUS} |
| Password Hash | {PASS_HASH_ALG} | N/A | {PASS_HASH_STATUS} |
| TokenSignature | {TOKEN_SIGN_ALG} | {TOKEN_SIGN_KEY} | {TOKEN_SIGN_STATUS} |
| TLS | {TLS_VERSION} | N/A | {TLS_STATUS} |

**KeyCode Location**:
{CRYPTO_CODE_LOCATIONS}

### 4.4 Log and AuditModule

**Location**: `{LOG_MODULE_PATH}`

**LogConfiguration**:
| LogType | Implementation | SensitiveDataProcessing | StorageLocation |
|---------|------|-------------|---------|
| ApplicationLog | {APP_LOG_IMPL} | {APP_LOG_SENSITIVE} | {APP_LOG_STORAGE} |
| SecurityLog | {SEC_LOG_IMPL} | {SEC_LOG_SENSITIVE} | {SEC_LOG_STORAGE} |
| AuditLog | {AUDIT_LOG_IMPL} | {AUDIT_LOG_SENSITIVE} | {AUDIT_LOG_STORAGE} |
| AccessLog | {ACCESS_LOG_IMPL} | {ACCESS_LOG_SENSITIVE} | {ACCESS_LOG_STORAGE} |

---

## 5. InitialAttack Surface

### 5.1 ExternalEntry Point

| Entry PointType | Endpoint/Path | AuthenticationRequirement | Exposure Level | RiskLevel |
|-----------|---------|---------|---------|---------|
| HTTP API | {API_ENDPOINT_1} | {API_AUTH_1} | {API_EXPOSURE_1} | {API_RISK_1} |
| WebSocket | {WS_ENDPOINT} | {WS_AUTH} | {WS_EXPOSURE} | {WS_RISK} |
| Webhook | {WEBHOOK_ENDPOINT} | {WEBHOOK_AUTH} | {WEBHOOK_EXPOSURE} | {WEBHOOK_RISK} |
| CLI | {CLI_ENDPOINT} | {CLI_AUTH} | {CLI_EXPOSURE} | {CLI_RISK} |

### 5.2 API Endpoint

| Endpoint | Method | Authentication | Authorization | SensitiveOperation | SecurityNote |
|------|------|------|------|---------|---------|
{API_ENDPOINTS_TABLE}
<!--
Format:
| /api/v1/auth/login | POST | None | None | Is | Brute forceCrackRisk |
| /api/v1/users | GET | JWT | RBAC | No | InformationDisclosureRisk |
-->

### 5.3 SensitiveDataLocation

| Data Type | StorageLocation | EncryptionStatus | AccessControl | RiskLevel |
|---------|---------|---------|---------|---------|
| UserCredentials | {CRED_LOCATION} | {CRED_ENC} | {CRED_ACCESS} | {CRED_RISK} |
| APIKey | {APIKEY_LOCATION} | {APIKEY_ENC} | {APIKEY_ACCESS} | {APIKEY_RISK} |
| PIIData | {PII_LOCATION} | {PII_ENC} | {PII_ACCESS} | {PII_RISK} |
| BusinessSensitive | {BIZ_LOCATION} | {BIZ_ENC} | {BIZ_ACCESS} | {BIZ_RISK} |
| LogData | {LOG_LOCATION} | {LOG_ENC} | {LOG_ACCESS} | {LOG_RISK} |

---

## 6. SecurityFindingSummary (Phase 1)

### 6.1 FindingStatistics

| Severity | Count | Percentage |
|---------|------|--------|
| Critical | {SF_CRITICAL} | {SF_CRITICAL_PCT}% |
| High | {SF_HIGH} | {SF_HIGH_PCT}% |
| Medium | {SF_MEDIUM} | {SF_MEDIUM_PCT}% |
| Low | {SF_LOW} | {SF_LOW_PCT}% |
| **Total** | **{SF_TOTAL}** | **100%** |

### 6.2 FindingList

| FindingID | Type | Title | Location | Severity | Follow-up Phase |
|--------|------|------|------|---------|---------|
{SECURITY_FINDINGS_TABLE}

### 6.3 Risk Indicators

| Indicator Description | RelatedFinding | RecommendationAnalysis Depth |
|-----------|---------|-------------|
{RISK_INDICATORS_TABLE}

### 6.4 Phase Reflection

**Key Findings**:
{KEY_FINDINGS_LIST}

**Needs Attention**:
{ATTENTION_AREAS_LIST}

**Pass toBelowPhase**:
{HANDOVER_NOTES_LIST}

---

**ReportEnd**
