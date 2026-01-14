<!-- Code-First Deep Threat Modeling Workflow | Version 2.1.0 | https://github.com/fr33d3m0n/skill-threat-modeling | License: BSD-3-Clause | Welcome to cite but please retain all sources and declarations -->

# Control Set 17: Agentic Security (AGENT)

**Domain**: AGENT - Agentic System and Agent-like Component Security
**Version**: 1.0
**Last Updated**: 2026-01-03
**Reference**: OWASP Top 10 for Agentic Applications 2026 (ASI01-ASI10)

---

## Overview

Agentic Security control set, applicable to complete AI Agent systems and agent-like components (Tools, Prompts, Skills). This control set focuses on Agent **behavior, intent, and autonomy** security, complementing ext-13 (AI/LLM Security):

- **ext-13**: Focuses on LLM model-level input/output security
- **ext-16**: Focuses on Agent behavior-level autonomy and collaboration security

**Core Security Paradigm**: Least-Agency Principle
> "Avoid giving agents unnecessary autonomy, not just unnecessary privileges." — OWASP ASI

---

## Trigger Conditions (On-demand Loading)

This control set is automatically loaded when the following conditions are detected:

### Complete Agent Systems
- `langchain` / `langgraph` import
- `crewai` / `autogen` / `autogpt` import
- `anthropic.Agent` / `openai.Assistant` API calls
- Agent workflow definition files (`agent.yaml`, `crew.yaml`)

### Agent-like Components
- MCP Server configuration (`mcp.json`, `claude_desktop_config.json`)
- Tool/Function definitions (`tools/*.py`, `functions/*.ts`)
- Skill definitions (`.claude/skills/`, `skills/*.md`)
- Prompt template libraries (`prompts/*.yaml`, `prompt_templates/`)
- Custom Instructions configuration

### Multi-Agent Systems
- A2A (Agent-to-Agent) protocol configuration
- Agent orchestration frameworks (Temporal workflows, Prefect flows)
- Multi-agent communication channels

---

## Core Controls

### AGENT-01: Goal & Intent Protection
**Control Requirement**: Agent goals and intents must be protected against tampering and hijacking

**Applicable Scope**: Agent systems, Prompt templates, Skill definitions

**Control Measures**:
- **Goal Integrity Verification**
  - System prompt integrity verification (hash/signature)
  - Goal definitions cannot be overwritten by user input
  - Use structured goal definitions instead of free text

- **Intent Alignment Monitoring**
  - Consistency detection between output and expected goals
  - Behavior deviation alerting mechanism
  - Intent reasoning logging

- **Goal Tampering Detection**
  - Detect prompts attempting to modify Agent goals
  - Goal drift detection in multi-turn conversations
  - Indirect goal injection detection (via tool return values)

**Cross-reference**: Complements ext-13 AI-01 (Prompt Injection Prevention)

**OWASP ASI Mapping**: ASI01 - Agent Goal Hijack

---

### AGENT-02: Tool & Skill Governance
**Control Requirement**: Tools and skills available to Agent must be controlled, auditable, and minimized

**Applicable Scope**: MCP Tools, Function Calling, Skills, Plugins

**Control Measures**:
- **Tool Registration & Verification**
  - Tool source verification and signature validation
  - Tool capability declaration review
  - Version management and change tracking
  - Prohibit dynamic loading of unreviewed tools

- **Capability Boundary Limits**
  - Tool permission whitelist mechanism
  - Resource access scope limits (file paths, network, API)
  - Operation type limits (read/write/execute/delete)
  - Call frequency and quota limits

- **Tool Call Chain Auditing**
  - Complete call chain logging
  - Tool parameter recording (sanitized)
  - Call result recording
  - Anomalous call pattern detection

- **MCP Server Security**
  - MCP Server source verification
  - Least privilege configuration
  - Communication encryption (stdio/SSE)
  - Server isolated execution

**Cross-reference**: Complements ext-13 AI-05 (Agent Action Control), ext-12 SUPPLY-04 (Source Verification)

**OWASP ASI Mapping**: ASI02 - Tool Misuse & Exploitation, ASI04 - Agentic Supply Chain

---

### AGENT-03: Non-Human Identity (NHI) Management
**Control Requirement**: Agent and component identities must be explicit, traceable, and access-controlled

**Applicable Scope**: Agent service accounts, API credentials, delegation chains

**Control Measures**:
- **Agent Identity Authentication**
  - Unique Agent identifiers
  - Secure Agent credential storage
  - Automatic credential rotation
  - Multi-factor authentication (high-risk operations)

- **Delegation Chain Verification**
  - Complete authorization chain: User → Agent → Tool
  - Delegation scope limits
  - Delegation time limits
  - Delegation chain audit logs

- **Service Account Least Privilege**
  - Agent-specific service accounts
  - Permission separation by function
  - Prohibit shared credentials
  - Regular permission reviews

- **Identity Lifecycle Management**
  - Agent instance creation/destruction records
  - Credential lifecycle management
  - Permission change auditing
  - Zombie Agent detection and cleanup

**Cross-reference**: Complements ext-15 CLOUD-01 (IAM Least Privilege), Control-01 (Authentication)

**OWASP ASI Mapping**: ASI03 - Identity & Privilege Abuse

---

### AGENT-04: Memory & Context Security
**Control Requirement**: Agent persistent memory and context must be protected against pollution and leakage

**Applicable Scope**: Agent Memory, Context Window, Session State, RAG

**Control Measures**:
- **Persistent Memory Protection**
  - Memory storage encryption
  - Memory access control
  - Memory content classification (sensitive/normal)
  - Memory version control and rollback

- **Context Pollution Detection**
  - Detect maliciously injected history messages
  - Context integrity verification
  - Anomalous context pattern alerting
  - Session history auditing

- **Session Isolation**
  - Strict isolation between user sessions
  - Context isolation between Agent instances
  - Tenant-level data isolation
  - Cross-session information leakage protection

- **Memory Cleanup Policies**
  - Sensitive data automatic expiration
  - User-requested deletion capability
  - Regular memory review
  - Compliance data retention

**Cross-reference**: Complements ext-13 AI-04 (Data Isolation), Control-10 (Data Protection)

**OWASP ASI Mapping**: ASI06 - Memory & Context Poisoning

---

### AGENT-05: Multi-Agent Communication Security
**Control Requirement**: Agent-to-Agent communication must be secure, verifiable, and boundary-controlled

**Applicable Scope**: Multi-Agent systems, A2A protocols, Agent orchestration

**Control Measures**:
- **A2A Protocol Security**
  - Agent-to-Agent communication encryption
  - Message authentication and integrity
  - Replay attack prevention
  - Protocol version compatibility verification

- **Inter-Agent Trust Verification**
  - Agent mutual identity authentication
  - Trust level stratification
  - Trust transitivity rules
  - Malicious Agent detection

- **Message Content Security**
  - Message format validation
  - Sensitive data sanitization
  - Message size limits
  - Malicious payload detection

- **Collaboration Boundary Control**
  - Agent collaboration scope limits
  - Cross-Agent data flow control
  - Collaboration pattern whitelist
  - Collaboration behavior auditing

**Cross-reference**: Complements Control-09 (API Security), ext-11 INFRA-04 (Network Segmentation)

**OWASP ASI Mapping**: ASI07 - Insecure Inter-Agent Communication

---

### AGENT-06: Behavioral Monitoring & Alignment
**Control Requirement**: Agent behavior must be continuously monitored to ensure alignment with expected goals

**Applicable Scope**: All Agent systems and agent-like components

**Control Measures**:
- **Behavior Deviation Detection**
  - Expected behavior baseline establishment
  - Real-time behavior deviation analysis
  - Anomalous behavior pattern recognition
  - Behavior drift trend monitoring

- **Autonomy Boundary Control**
  - Autonomous decision scope limits
  - Human confirmation for high-risk decisions
  - Dynamic autonomy level adjustment
  - Emergency autonomy downgrade

- **Behavior Audit Logging**
  - Complete decision process recording
  - Reasoning chain tracing
  - Behavior attribution analysis
  - Tamper-proof audit logs

- **Rogue Agent Detection**
  - Malicious behavior pattern library
  - Behavior consistency detection
  - Goal deviation alerting
  - Automatic isolation mechanism

**Cross-reference**: Complements Control-07 (Logging), ext-13 AI-05 (Agent Action Control)

**OWASP ASI Mapping**: ASI10 - Rogue Agents

---

### AGENT-07: Cascading Failure Prevention
**Control Requirement**: Agent systems must have fault isolation and degradation capabilities to prevent cascading failures

**Applicable Scope**: Multi-Agent orchestration, Agent workflows, Tool call chains

**Control Measures**:
- **Fault Isolation**
  - Agent instance-level isolation
  - Tool call failure isolation
  - Error boundary settings
  - Fault domain partitioning

- **Error Propagation Prevention**
  - Error type classification handling
  - Error propagation chain interruption
  - Poisoned input detection
  - Cascade trigger detection

- **Degradation & Circuit Breaking**
  - Service degradation strategy
  - Circuit breaker pattern implementation
  - Graceful degradation paths
  - Degradation state monitoring

- **Orchestration Layer Fault Tolerance**
  - Workflow retry strategies
  - Compensating transaction mechanisms
  - Timeout control
  - Deadlock detection and recovery

**Cross-reference**: Complements Control-08 (Error Handling), ext-11 INFRA-03 (Resource Limits)

**OWASP ASI Mapping**: ASI08 - Cascading Failures

---

### AGENT-08: Human-Agent Trust Boundary
**Control Requirement**: Clear human-agent trust boundaries must be established; high-risk operations require human confirmation

**Applicable Scope**: All Agent systems

**Control Measures**:
- **Human-in-the-Loop (HITL)**
  - High-risk operations must have human confirmation
  - Confirmation mechanism cannot be bypassed
  - Confirmation timeout auto-rejection
  - Batch confirmation risk control

- **Trust Level Stratification**
  - Operation risk level classification
  - Dynamic trust threshold adjustment
  - User trust level management
  - Context-aware trust assessment

- **Transparency Requirements**
  - Agent behavior explainability
  - Decision process traceability
  - User informed consent
  - Clear capability boundary disclosure

- **Revocation & Rollback**
  - Operations must be revocable
  - State rollback capability
  - Revocation time limits
  - Revocation audit records

**Cross-reference**: Complements ext-13 AI-05 (Agent Action Control) LLM08 (Excessive Agency)

**OWASP ASI Mapping**: ASI05 - Unexpected Code Execution, ASI09 - Human-Agent Trust Exploitation

---

### AGENT-09: Prompt & Skill Template Security
**Control Requirement**: Prompt templates and Skill definitions must be securely managed to prevent malicious tampering and injection

**Applicable Scope**: Prompt Templates, Skills, Custom Instructions

**Control Measures**:
- **Template Integrity**
  - Template version control
  - Template signature verification
  - Change approval workflow
  - Template source verification

- **Injection Protection**
  - Clear variable boundary definitions
  - User input isolation from templates
  - Special character escaping
  - Nested injection detection

- **Skill Security Assessment**
  - Skill capability declaration review
  - Skill behavior testing
  - Skill dependency review
  - Skill sandbox execution

- **Template Access Control**
  - Template read/write permission separation
  - Sensitive template encrypted storage
  - Template usage auditing
  - Template leakage detection

**Cross-reference**: Complements ext-13 AI-01 (Prompt Injection Prevention), ext-12 SUPPLY-04 (Source Verification)

**OWASP ASI Mapping**: ASI01 - Agent Goal Hijack (via Prompt), ASI04 - Agentic Supply Chain (Skills)

---

### AGENT-10: Agentic Supply Chain Security
**Control Requirement**: Agent-related component supply chains must be secure and controlled

**Applicable Scope**: Agent Frameworks, MCP Servers, Tools, Skills, Plugins

**Control Measures**:
- **Framework Security**
  - Agent framework vulnerability monitoring
  - Framework version management
  - Timely security patch application
  - Framework configuration auditing

- **MCP Server Supply Chain**
  - MCP Server source verification
  - Server code review
  - Dependency scanning
  - Malicious Server detection

- **Tool/Skill Supply Chain**
  - Tool source whitelist
  - Skill publisher verification
  - Code signature verification
  - Malicious code scanning

- **Third-party Integration Security**
  - Third-party service security assessment
  - API credential security management
  - Integration point monitoring
  - Data flow security

**Cross-reference**: Fully complements ext-12 (Supply Chain Security)

**OWASP ASI Mapping**: ASI04 - Agentic Supply Chain Vulnerabilities

---

## Control Applicability Matrix

| Control | Agent Systems | MCP Tools | Skills | Prompts | Multi-Agent |
|---------|:-------------:|:---------:|:------:|:-------:|:-----------:|
| AGENT-01 | ● | ○ | ● | ● | ● |
| AGENT-02 | ● | ● | ● | ○ | ● |
| AGENT-03 | ● | ● | ○ | ○ | ● |
| AGENT-04 | ● | ○ | ○ | ○ | ● |
| AGENT-05 | ● | ○ | ○ | ○ | ● |
| AGENT-06 | ● | ○ | ● | ○ | ● |
| AGENT-07 | ● | ○ | ○ | ○ | ● |
| AGENT-08 | ● | ● | ● | ○ | ● |
| AGENT-09 | ○ | ○ | ● | ● | ○ |
| AGENT-10 | ● | ● | ● | ○ | ● |

**Legend**: ● Strongly relevant | ○ Weakly relevant or as-needed

---

## L4 References

Detailed practice guides in the `references/` directory:
- reference-set-17-agentic-security.md
- reference-set-17-mcp-security.md
- reference-set-17-multi-agent-patterns.md
- reference-set-17-skill-security.md

Internal references:
- agentic-threats.yaml
- llm-threats.yaml (cross-reference)

External references:
- OWASP Top 10 for Agentic Applications 2026
- OWASP Non-Human Identities (NHI) Top 10
- MITRE ATLAS (Adversarial Threat Landscape for AI Systems)

---

## STRIDE Mapping

| STRIDE | Applicable Controls | Threat Examples |
|--------|---------------------|-----------------|
| S | AGENT-03, AGENT-05 | Agent identity spoofing, A2A message forgery |
| T | AGENT-01, AGENT-04, AGENT-09 | Goal tampering, memory poisoning, template injection |
| R | AGENT-06, AGENT-08 | Untraceable behavior, unauditable operations |
| I | AGENT-04, AGENT-05 | Memory leakage, inter-Agent data leakage |
| D | AGENT-07 | Cascading failures causing service unavailability |
| E | AGENT-01, AGENT-02, AGENT-06 | Goal hijacking privilege escalation, tool abuse, Rogue Agent |

---

## Relationship with ext-13 (AI/LLM Security)

```
┌─────────────────────────────────────────────────────────────────────────┐
│                     Security Control Layer Relationship                  │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  Application Layer                                                       │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │  ext-16: Agentic Security (AGENT)                                │   │
│  │  ───────────────────────────────────────────────────────────    │   │
│  │  • Agent behavior and intent security                            │   │
│  │  • Tool/Skill governance                                         │   │
│  │  • Multi-Agent collaboration security                            │   │
│  │  • Human-Agent trust boundary                                    │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                              │ Depends on                                │
│                              ▼                                          │
│  Model Layer                                                             │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │  ext-13: AI/LLM Security (AI)                                    │   │
│  │  ───────────────────────────────────────────────────────────    │   │
│  │  • Prompt injection protection                                   │   │
│  │  • Output validation                                             │   │
│  │  • Model access control                                          │   │
│  │  • Data isolation                                                │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                          │
│  Relationship: ext-16 EXTENDS ext-13 (Agent builds on LLM)              │
│  Cross-references: AGENT-01↔AI-01, AGENT-02↔AI-05, AGENT-04↔AI-04      │
│                                                                          │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-01-03 | Initial release based on OWASP Agentic Top 10 2026 |
