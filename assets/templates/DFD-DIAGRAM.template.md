<!-- Code-First Deep Threat Modeling Workflow | Version 2.1.0 | https://github.com/fr33d3m0n/skill-threat-modeling | License: BSD-3-Clause | Welcome to cite but please retain all sources and declarations -->

# Data Flow DiagramReport: {PROJECT_NAME}

> **Assessment Time**: {ASSESSMENT_DATETIME}
> **AnalysisAnalyst**: Claude (Deep Risk Analysis)
> **FrameworkVersion**: STRIDE-TM v1.0.2
> **ReportVersion**: {REPORT_VERSION}

---

## 1. DFD Overview

### 1.1 Level 0 Context Diagram

```
{LEVEL0_CONTEXT_DIAGRAM}
```
<!--
Example:
 ┌─────────────────────────────┐
 │ │
 ┌─────────────┐ │ ┌───────────────┐ │ ┌─────────────┐
 │ │ │ │ │ │ │ │
 │ User │◄────►│ │ {SYSTEM} │◄────►│ │ External │
 │ (Browser) │ │ │ │ │ │ Service │
 │ │ │ └───────────────┘ │ │ │
 └─────────────┘ │ │ └─────────────┘
 │ Trust Boundary │
 └─────────────────────────────┘
-->

### 1.2 Level 1 SystemDiagram

```
{LEVEL1_SYSTEM_DIAGRAM}
```
<!--
Example:
┌─────────────────────────────────────────────────────────────────────────────────┐
│ Level 1: System Data Flow │
├─────────────────────────────────────────────────────────────────────────────────┤
│ │
│ ┌─────────┐ │
│ │ EI01 │ │
│ │ User │ │
│ └────┬────┘ │
│ │ DF01: HTTP Request (credentials) │
│ ▼ │
│ ══════════════════════════════════ TB01: Network Boundary ════════════════ │
│ │ │
│ ▼ │
│ ┌─────────┐ DF02: Auth Request ┌─────────┐ │
│ │ P01 │──────────────────────────────►│ P02 │ │
│ │ Web App │ │Auth Svc │ │
│ └────┬────┘ └────┬────┘ │
│ │ │ │
│ │ DF03: Data Request │ DF04: Token Verify │
│ ▼ ▼ │
│ ┌─────────┐ DF05: Query ┌─────────┐ │
│ │ P03 │──────────────────────────────►│ DS01 │ │
│ │ API Svc │◄──────────────────────────────│Database │ │
│ └─────────┘ DF06: Results └─────────┘ │
│ │
│ ══════════════════════════════════ TB02: Process Boundary ════════════════ │
│ │
│ ┌─────────┐ │
│ │ EI02 │ │
│ │External │◄─── DF07: Webhook ──────────────────────────────────────────── │
│ │ API │ │
│ └─────────┘ │
│ │
└─────────────────────────────────────────────────────────────────────────────────┘
-->

### 1.3 DFD Statistics

| Element Type | Count | ThreatAssociationCount |
|---------|------|-----------|
| Process (Process) | {PROCESS_COUNT} | {PROCESS_THREATS} |
| DataStorage (Data Store) | {DS_COUNT} | {DS_THREATS} |
| DataFlow (Data Flow) | {DF_COUNT} | {DF_THREATS} |
| ExternalEntity (External Entity) | {EI_COUNT} | N/A |
| TrustBoundary (Trust Boundary) | {TB_COUNT} | N/A |
| **Total** | **{TOTAL_ELEMENTS}** | **{TOTAL_THREATS}** |

---

## 2. DFD ElementList

### 2.1 Process (Processes)

| ID | Name | Description | Technology | Authentication | Authorization | ThreatCount |
|----|------|------|------|------|------|--------|
{PROCESSES_TABLE}
<!--
Format:
| P01 | Web Application | BeforeEndWebApplication | React/Next.js | Session | RBAC | 5 |
| P02 | Authentication Service | AuthenticationService | Node.js/Express | N/A | N/A | 8 |
| P03 | API Service | AfterEndAPIService | Node.js/Express | JWT | RBAC | 12 |
-->

#### ProcessDetails

{PROCESS_DETAILS_SECTION}
<!--
Format:
##### P01: Web Application

**Description**: {DESCRIPTION}

**Technology Stack**: {TECH_STACK}

**SecurityProperty**:
| Property | Value |
|------|-----|
| Code Type | Web |
| Running As | User |
| Isolation Level | AppContainer |
| Implements Authentication | Yes |
| Implements Authorization | Yes |
| Input Sanitization | Yes |
| Output Encoding | Yes |

**ThreatAssociation**: T-S-P01-001, T-T-P01-001,...
-->

### 2.2 DataStorage (Data Stores)

| ID | Name | Type | Description | SensitiveData | Encryption | ThreatCount |
|----|------|------|------|---------|------|--------|
{DATA_STORES_TABLE}
<!--
Format:
| DS01 | PostgreSQL | RDBMS | Main Database | PII, Credentials | StaticEncryption | 6 |
| DS02 | Redis | Cache | SessionCache | Session | None | 3 |
| DS03 | S3 Bucket | Object Store | FileStorage | Documentation | ServiceEndEncryption | 2 |
-->

#### DataStorageDetails

{DATA_STORE_DETAILS_SECTION}
<!--
Format:
##### DS01: PostgreSQL Database

**Description**: {DESCRIPTION}

**Technology**: PostgreSQL {VERSION}

**SecurityProperty**:
| Property | Value |
|------|-----|
| Stores Credentials | Yes |
| Stores Logs | No |
| Encrypted | Yes (AES-256) |
| Signed | N/A |
| Backup Enabled | Yes |

**SensitiveDataCategory**:
- PII: User Name、Email、Phone
- Credentials: Password Hash、APIKey
- BusinessData: Order、Transaction Record

**ThreatAssociation**: T-T-DS01-001, T-I-DS01-001,...
-->

### 2.3 DataFlow (Data Flows)

| ID | Name | Source | Target | Protocol | Encryption | Authentication | ThreatCount |
|----|------|-----|------|------|------|------|--------|
{DATA_FLOWS_TABLE}
<!--
Format:
| DF01 | User Request | EI01 | P01 | HTTPS | TLS 1.3 | Session | 3 |
| DF02 | Auth Request | P01 | P02 | gRPC | mTLS | Internal | 2 |
| DF03 | DB Query | P03 | DS01 | TCP | None | Password | 4 |
-->

#### DataFlowDetails

{DATA_FLOW_DETAILS_SECTION}
<!--
Format:
##### DF01: User HTTP Request

**Description**: User BrowserToWebApplicationHTTPRequest

**Path**: EI01 (User) → P01 (Web App)

**SecurityProperty**:
| Property | Value |
|------|-----|
| Protocol | HTTPS |
| Encrypted | Yes (TLS 1.3) |
| Authenticated | Yes (Session/JWT) |
| Data Classification | User Input |

**TransmissionData**:
- UserCredentials (LoginWhen)
- UserInput
- FileUpload

**ThreatAssociation**: T-T-DF01-001, T-I-DF01-001,...
-->

### 2.4 ExternalEntity (External Entities)

| ID | Name | Type | Description | TrustLevel |
|----|------|------|------|---------|
{EXTERNAL_ENTITIES_TABLE}
<!--
Format:
| EI01 | User | Human | SystemUser（Browser Tool） | Untrusted |
| EI02 | Admin | Human | ManagementMemberUser | Partially Trusted |
| EI03 | External API | System | Third-partyAPIService | Partially Trusted |
| EI04 | Webhook Sender | System | Webhook Caller | Untrusted |
-->

---

## 3. TrustBoundary

### 3.1 BoundaryDefinition

| ID | Name | Type | Description |
|----|------|------|------|
{TRUST_BOUNDARIES_TABLE}
<!--
Format:
| TB01 | Internet-DMZ | Network | Internet to DMZ Zone |
| TB02 | DMZ-Internal | Network | DMZToInternalNetwork |
| TB03 | Application-Database | Process | ApplicationLayerToDataLayer |
| TB04 | User-Admin | User | CommonUserToManagementMember |
-->

#### BoundaryDetails

{TRUST_BOUNDARY_DETAILS_SECTION}
<!--
Format:
##### TB01: Internet-DMZ Boundary

**Type**: Network

**Description**: Public Network to Internal DMZ ZoneNetworkBoundary

**InternalElement**:
- P01: Web Application
- P02: API Gateway

**CrossingDataFlow**:
| DataFlow | Direction | SecurityControl |
|--------|------|---------|
| DF01 | Inbound | TLS, WAF, Rate Limit |
| DF10 | Outbound | TLS |

**SecurityControl**:
- [] Firewall
- [] WAF
- [] DDoS Protection
- [] TLS Terminate
-->

### 3.2 BoundaryCrossingAnalysis

| DataFlow | CrossingBoundary | Direction | DataCategory | SecurityControl | RiskLevel |
|--------|---------|------|---------|---------|---------|
{BOUNDARY_CROSSING_TABLE}
<!--
Format:
| DF01 | TB01 | Inbound | User Input | TLS, Validation | Medium |
| DF02 | TB02 | Internal | Auth Token | mTLS | Low |
| DF03 | TB03 | Internal | SQL Query | None | High |
-->

### 3.3 KeyInterface

| Interface | Type | Boundary | SecurityMechanism | RiskLevel |
|------|------|------|---------|---------|
{CRITICAL_INTERFACES_TABLE}
<!--
Format:
| API Gateway | HTTP | TB01 | JWT, Rate Limit | High |
| Database Connection | TCP | TB03 | Password Auth | Critical |
| Cache Connection | TCP | TB02 | None | High |
-->

---

## 4. SensitiveDataFlow

### 4.1 PII DataFlow

```
{PII_DATA_FLOW_DIAGRAM}
```
<!--
Example:
PII Data Flow Path:

 EI01 (User Input)
 │
 │ DF01: name, email, phone
 ▼
 P01 (Web App) ─────► P02 (API) ─────► DS01 (Database)
 │
 │ stored: hashed
 ▼
 [Encrypted at Rest]
-->

**PII Data Type**:
| Data Type | FromSource | ProcessingLocation | StorageLocation | EncryptionStatus |
|---------|------|---------|---------|---------|
{PII_DATA_TABLE}

### 4.2 CredentialsDataFlow

```
{CREDENTIAL_DATA_FLOW_DIAGRAM}
```

**CredentialsType**:
| CredentialsType | FromSource | TransmissionEncryption | StorageEncryption | AccessControl |
|---------|------|---------|---------|---------|
{CREDENTIAL_DATA_TABLE}

### 4.3 OtherSensitiveData

| Data Type | SensitiveLevel | DataFlowPath | ProtectionMeasure |
|---------|---------|-----------|---------|
{OTHER_SENSITIVE_DATA_TABLE}

---

## 5. SecurityFindingSummary (Phase 2-3)

### 5.1 FindingStatistics

| Severity | Phase 2 | Phase 3 | Total |
|---------|---------|---------|------|
| Critical | {P2_CRITICAL} | {P3_CRITICAL} | {TOTAL_CRITICAL} |
| High | {P2_HIGH} | {P3_HIGH} | {TOTAL_HIGH} |
| Medium | {P2_MEDIUM} | {P3_MEDIUM} | {TOTAL_MEDIUM} |
| Low | {P2_LOW} | {P3_LOW} | {TOTAL_LOW} |

### 5.2 FindingList

| FindingID | Phase | Type | Title | Location | Severity |
|--------|------|------|------|------|---------|
{P2_P3_FINDINGS_TABLE}

### 5.3 Phase Reflection

**Phase 2 Key Findings**:
{P2_KEY_FINDINGS}

**Phase 3 Key Findings**:
{P3_KEY_FINDINGS}

**Pass to Phase 4-5**:
{P2_P3_HANDOVER}

---

## Appendices

### Appendices A: Mermaid DFD SourceCode

#### Level 0 Context Diagram

```mermaid
{MERMAID_LEVEL0}
```

#### Level 1 System Diagram

```mermaid
{MERMAID_LEVEL1}
```

### Appendices B: ElementPropertyDetails

#### B.1 ProcessProperty

{PROCESS_ATTRIBUTES_TABLE}
<!--
Format:
| ID | Code Type | Running As | Isolation | Auth | Authz | Input San. | Output Enc. |
|----|-----------|------------|-----------|------|-------|------------|-------------|
| P01 | Web | User | AppContainer | Yes | Yes | Yes | Yes |
-->

#### B.2 DataStorageProperty

{DATA_STORE_ATTRIBUTES_TABLE}
<!--
Format:
| ID | Stores Creds | Stores Logs | Encrypted | Signed | Backup |
|----|--------------|-------------|-----------|--------|--------|
| DS01 | Yes | No | Yes | N/A | Yes |
-->

#### B.3 DataFlowProperty

{DATA_FLOW_ATTRIBUTES_TABLE}
<!--
Format:
| ID | Protocol | Encrypted | Authenticated | Data Classification |
|----|----------|-----------|---------------|---------------------|
| DF01 | HTTPS | Yes (TLS 1.3) | Yes | User Input |
-->

---

**ReportEnd**
