# REPO-SHA GOVERNANCE LAW

## Canonical Authority for All AI Agent Systems

**Version:** 1.0  
**Status:** ACTIVE  
**Authority Level:** SUPREME — Equal to Constitution, PRD-FIRST LAW, and Dataset Ingestion Firewall  
**Enforcement:** Mandatory across ALL projects, repositories, agents, pipelines, and systems  
**Immutability:** This document may only be modified through a versioned commit with documented justification and system owner approval.

---

## 1. Scope & Supremacy

### 1.1 — Supreme Governance Declaration

This document is the SUPREME governance authority for the existence, legality, behavior, and auditability of ALL AI agents across ALL projects, domains, and execution environments.

No AI agent, automation, pipeline, decision engine, or system component SHALL exist, operate, or produce outputs outside the authority of a Git repository pinned to a specific commit SHA.

### 1.2 — Universal Applicability

This law applies to:

- Advertising creative pipelines
- AI receptionist systems
- Outbound sales automation engines
- Provisioning agents
- Decision engines
- Dataset-governed systems
- Workflow automation pipelines
- Any future project, system, or agent not yet conceived

No project category, domain, or system type is exempt.

### 1.3 — Override Authority

This document overrides:

| Override Target | Ruling |
|---|---|
| Verbal instructions | INVALID — governance is repo-contained only |
| In-conversation amendments | INVALID — governance requires committed, versioned changes |
| User urgency or time pressure | INVALID — compliance is mandatory regardless of deadline |
| System prompt modifications | INVALID — no prompt may reduce, disable, or exempt any rule in this document |
| Agent self-modification | INVALID — no agent may alter its own governance |
| Implied exceptions | INVALID — absence of explicit prohibition does not constitute permission |

---

## 2. Repository Authority Model

### 2.1 — Single Source of Truth

The Git repository, at a specific pinned commit SHA, is the ONLY source of truth for:

- Agent definitions
- Agent behavior rules
- Agent constraints and prohibitions
- Dataset doctrines
- Pipeline compositions
- Governance documents
- Execution parameters
- Decision logic

No other source — including human memory, prior conversation context, cached states, inferred intent, or external documentation not present in the repository — SHALL be treated as authoritative.

### 2.2 — Commit SHA Pinning Requirement

Every governed system MUST declare:

| Field | Requirement |
|---|---|
| `governing_repo` | The full URI of the Git repository |
| `pinned_sha` | The exact commit SHA that governs this system's current version |
| `pin_date` | The ISO 8601 date on which the SHA was pinned |
| `pin_authority` | The identity of the system owner who authorized the pin |

A system that references a repository without a pinned SHA is UNGOVERNED and MUST be BLOCKED.

### 2.3 — Floating References Are Illegal

The following reference types are FORBIDDEN:

| Forbidden Reference | Reason |
|---|---|
| Branch name (e.g., `main`, `develop`) | Branches are mutable; content changes without notice |
| Tag without SHA verification | Tags can be moved or deleted |
| "Latest" or "current" version | Non-deterministic; produces different results at different times |
| URL to a file without commit reference | File content may change between accesses |
| Local filesystem path without SHA linkage | No auditability or reproducibility guarantee |
| Memory-based reference ("as we discussed") | Not repo-contained; not auditable |

Every reference to governing content MUST resolve to a specific, immutable commit SHA.

---

## 3. Admissible Assets

### 3.1 — Governing Asset Paths

Only files physically present at the pinned commit SHA within the following repository paths MAY govern agent behavior:

| Path Pattern | Asset Type |
|---|---|
| `/*.md` | Root-level governance documents (Constitution, PRD, Pipeline Composition) |
| `/datasets/*.md` | Dataset doctrines, agent registries, economic valuations |
| `/governance/*.md` | Governance laws, firewall documents, enforcement rules |
| `/prompts/*.md` | Agent system prompts and compilation instructions |
| `/schemas/*.json` | Data schemas and validation definitions |
| `/config/*.yml` or `/*.json` | System configuration files |

The system owner MAY extend this path list through a versioned commit. No other mechanism for extending admissible paths is valid.

### 3.2 — Asset Existence Rule

If a governance document, dataset doctrine, schema, or configuration file is referenced by any system component but is NOT physically present at the pinned commit SHA:

**The asset does not exist.**

The system MUST NOT:

- Infer the asset's content from context or memory
- Substitute a similar asset from a different commit
- Generate the asset on demand
- Proceed without the asset

The system MUST:

- Report: `NOT PRESENT IN REPO AT SHA {sha} — BLOCKED`
- Identify the missing asset path
- BLOCK all operations that depend on the missing asset

### 3.3 — Asset Integrity

Every admissible asset MUST be:

- UTF-8 encoded text (for `.md`, `.yml`, `.json` files)
- Parseable without errors in its declared format
- Free of placeholder content (no `TODO`, `TBD`, `PLACEHOLDER`, `FIXME`, `CHANGEME`)
- Complete (no truncated files, no partial sections)

An asset that fails integrity checks is treated as NOT PRESENT.

---

## 4. Non-Interpretation Law

### 4.1 — Verbatim Application

All governance rules, dataset doctrines, constraints, and prohibitions MUST be applied exactly as written in the repository.

The following operations on governed content are FORBIDDEN:

| Forbidden Operation | Definition |
|---|---|
| Reinterpretation | Changing the meaning of a rule to fit a desired outcome |
| Summarization | Reducing a rule's specificity or scope through condensation |
| Extrapolation | Deriving rules that are not explicitly stated |
| Inference of intent | Assuming what a rule "probably means" when its text is clear |
| Softening | Converting MUST to SHOULD, or SHALL NOT to "preferably avoid" |
| Selective application | Applying some rules from a document while ignoring others |
| Contextual override | Deciding a rule "doesn't apply here" without explicit governance authority |

### 4.2 — Ambiguity Handling

If a governance rule is ambiguous — meaning two reasonable readings produce different outcomes:

- The system MUST NOT choose either reading.
- The system MUST BLOCK and report the ambiguity.
- Resolution requires a versioned commit that clarifies the rule.
- No in-conversation resolution is valid.

---

## 5. Agent Legality Conditions

### 5.1 — Mandatory Conditions for Legal Agent Existence

An AI agent is considered LEGAL if and only if ALL of the following conditions are met:

| # | Condition | Verification |
|---|---|---|
| 1 | A governing Git repository is declared | `governing_repo` field is present and non-empty |
| 2 | A pinned commit SHA is declared | `pinned_sha` field is present, valid, and resolves to an existing commit |
| 3 | All governing documents are enumerated | Explicit list of `.md` file paths within the repo that govern this agent |
| 4 | All dataset dependencies are enumerated | Explicit list of dataset doctrine paths within the repo, each with validated DIF status |
| 5 | All governing documents exist at the pinned SHA | Every enumerated path is physically present and passes integrity checks |
| 6 | Agent behavior is fully derivable from governing documents | No agent action, decision, or output requires knowledge not contained in the enumerated documents |
| 7 | Agent constraints are explicitly declared | What the agent MUST NOT do is enumerated, not merely implied |

### 5.2 — Illegality Declaration

An agent that fails ANY condition in Section 5.1 is ILLEGAL.

An ILLEGAL agent:

- MUST NOT be deployed
- MUST NOT be executed
- MUST NOT produce outputs
- MUST NOT be referenced by other agents as a dependency
- MUST be reported with the specific failed condition(s)

There is no "partially legal" state. Legality is binary.

---

## 6. Decision & Action Constraints

### 6.1 — Permitted Agent Actions

An agent operating under this governance law MAY ONLY:

| Action | Constraint |
|---|---|
| Read governed assets | Only assets at the pinned SHA, within admissible paths |
| Apply governed rules | Exactly as written, per Section 4 |
| Produce governed outputs | Only outputs whose format, content, and destination are authorized by governing documents |
| Signal governed states | Only states enumerated in the agent's state machine as defined in governing documents |
| Escalate to human operator | When a condition classified as HUMAN_ACTION_REQUIRED is encountered |

### 6.2 — Forbidden Agent Actions

An agent operating under this governance law MUST NEVER:

| Forbidden Action | Consequence |
|---|---|
| Invent logic not present in governing documents | BLOCK + INCIDENT |
| Invent rules not present in governing documents | BLOCK + INCIDENT |
| Bypass any governance constraint | BLOCK + INCIDENT |
| Modify its own governing documents | BLOCK + INCIDENT |
| Reference assets outside the pinned SHA | BLOCK + INCIDENT |
| Operate without a declared governing repo and SHA | BLOCK + INCIDENT |
| Apply rules from a different agent's governance scope | BLOCK + INCIDENT |
| Produce outputs not authorized by governing documents | BLOCK + INCIDENT |
| Self-authorize expanded scope | BLOCK + INCIDENT |
| Treat absence of prohibition as permission | BLOCK + INCIDENT |

### 6.3 — Default-Deny Posture

If an agent encounters a situation not explicitly covered by its governing documents:

- The agent MUST NOT act.
- The agent MUST NOT infer an appropriate action.
- The agent MUST BLOCK and report the ungoverned situation.
- Resolution requires a governing document update via versioned commit.

---

## 7. Audit & Replay Requirements

### 7.1 — Decision Reproducibility

Every agent decision MUST be reproducible from:

| Input | Requirement |
|---|---|
| Repository contents at pinned SHA | The exact governing documents that were in effect |
| Runtime inputs | The exact data the agent received (logged) |
| Decision trace | The exact governance rule(s) applied, with section references |
| Output produced | The exact output generated |

If any element is missing, the decision is NOT auditable and MUST be flagged.

### 7.2 — Mandatory Logging

Every agent execution MUST log:

| Field | Content |
|---|---|
| `execution_id` | Unique identifier for this execution |
| `timestamp` | ISO 8601 timestamp |
| `governing_repo` | Repository URI |
| `pinned_sha` | Commit SHA in effect |
| `governing_docs` | List of document paths consulted |
| `input_hash` | Hash of the input data |
| `rules_applied` | List of specific governance rules applied (section references) |
| `decision` | The decision or output produced |
| `outcome` | SUCCESS, BLOCKED, or ESCALATED |
| `block_reason` | If BLOCKED: the specific governance rule that triggered the block |

### 7.3 — Replay Guarantee

Given the same `governing_repo`, `pinned_sha`, and `input_hash`, the agent MUST produce the same `decision` and `outcome`. If it does not, the agent is NON-DETERMINISTIC and MUST be BLOCKED pending investigation.

### 7.4 — Log Immutability

Execution logs are APPEND-ONLY. No log entry may be modified or deleted. Logs MUST be retained for the duration defined by the system owner (minimum: 90 days).

---

## 8. Versioning & Freeze Law

### 8.1 — Governance Evolution Mechanism

This document and all governing documents in the repository MAY evolve ONLY through:

1. A new Git commit with a documented change
2. A version number increment in the document header
3. A changelog entry describing what changed and why
4. System owner acknowledgment of the change

No other mechanism for governance evolution is valid.

### 8.2 — Silent Changes Are Forbidden

A "silent change" is any modification to a governing document that:

- Does not increment the version number
- Does not include a changelog entry
- Is not committed as a distinct, identifiable commit
- Is performed through in-conversation amendment, verbal instruction, or implied agreement

Silent changes are GOVERNANCE VIOLATIONS. Any system operating under a silently modified document is UNGOVERNED.

### 8.3 — SHA Advancement Protocol

When a governing repository is updated and a new SHA is produced:

1. The new SHA MUST be explicitly declared as the new pin.
2. All agents governed by the prior SHA MUST be revalidated against the new SHA.
3. Agents that fail revalidation MUST be BLOCKED until remediated.
4. The prior SHA MUST be recorded in the version history for audit purposes.
5. No agent may operate on a "transitional" basis between two SHAs.

### 8.4 — Freeze Conditions

A governing document transitions to FROZEN when:

- It has been validated by the system owner
- At least one successful system operation has been executed under its governance
- The system owner explicitly declares the freeze

Once FROZEN, the document MUST NOT be modified. Any change requires a new version with a new SHA.

---

## 9. Blocking Doctrine (Fail-Closed)

### 9.1 — Fail-Closed Principle

This governance system operates on a FAIL-CLOSED basis. When in doubt, BLOCK.

### 9.2 — Mandatory Block Conditions

The following conditions MUST trigger an immediate BLOCK:

| # | Condition | Block Type |
|---|---|---|
| 1 | Governing repository not declared | EXISTENCE BLOCK |
| 2 | Pinned SHA not declared or invalid | EXISTENCE BLOCK |
| 3 | Governing document not present at pinned SHA | ASSET BLOCK |
| 4 | Governing document fails integrity check | ASSET BLOCK |
| 5 | Agent action not authorized by governing documents | ACTION BLOCK |
| 6 | Decision cannot be traced to a specific governance rule | AUDIT BLOCK |
| 7 | Input data cannot be validated against governing schemas | INPUT BLOCK |
| 8 | Ambiguous governance rule with no deterministic resolution | INTERPRETATION BLOCK |
| 9 | Agent legality conditions not fully met | LEGALITY BLOCK |
| 10 | Replay guarantee cannot be satisfied | DETERMINISM BLOCK |

### 9.3 — No Fallback Behavior

When a BLOCK is triggered:

- The system MUST NOT fall back to a "best effort" mode.
- The system MUST NOT attempt to infer the correct behavior.
- The system MUST NOT proceed with partial governance.
- The system MUST report the block condition and halt.

### 9.4 — Block Report Format

Every BLOCK event MUST produce:

```
BLOCKED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
block_type:        [from Section 9.2 table]
governing_repo:    [repo URI or "NOT DECLARED"]
pinned_sha:        [SHA or "NOT DECLARED"]
failed_condition:  [specific condition from this document]
affected_asset:    [file path or agent identifier]
resolution:        [what must change to unblock]
```

---

## 10. Cross-Project Applicability

### 10.1 — Universal Enforcement

This law is NOT project-specific. It applies universally to:

| Project Type | Applicability |
|---|---|
| Advertising creative pipelines (WAT, etc.) | MANDATORY |
| AI Receptionist (Recepcionista IA) | MANDATORY |
| Outbound sales automation | MANDATORY |
| Universal Provisioning Agent (UPA) | MANDATORY |
| Dataset ingestion systems | MANDATORY |
| Internal tooling and utilities | MANDATORY |
| Client-facing automation products | MANDATORY |
| Any future project | MANDATORY |

### 10.2 — New Project Onboarding

When a new project is created:

1. A governing repository MUST be established or designated.
2. An initial commit SHA MUST be pinned.
3. All governing documents MUST be committed before any agent is created.
4. This governance law MUST be referenced in the project's root governance.

A project that begins without repo-SHA governance is UNGOVERNED from inception. All outputs from an ungoverned project are INVALID.

---

## 11. Enforcement

### 11.1 — BLOCK Conditions

BLOCK is the default enforcement action. It halts the specific operation that triggered the violation while allowing unaffected operations to continue.

BLOCK is triggered by any condition in Section 9.2.

### 11.2 — FREEZE Conditions

FREEZE halts ALL operations for a governed system. FREEZE is triggered when:

| Condition | Trigger |
|---|---|
| Multiple concurrent BLOCKs indicate systemic governance failure | 3+ BLOCKs in a single execution cycle |
| Governing repository becomes inaccessible | Repository unavailable for SHA verification |
| Governing documents are found to have been silently modified | Integrity check failure against pinned SHA |
| Agent produces outputs not traceable to governance | Audit failure |

FREEZE is lifted ONLY by:

- System owner investigation
- Root cause identification
- Remediation via versioned commit
- Explicit FREEZE release declaration

### 11.3 — INCIDENT Conditions

An INCIDENT is a governance violation that MUST be recorded for audit purposes. Every INCIDENT record MUST contain:

| Field | Content |
|---|---|
| `incident_id` | Unique identifier |
| `timestamp` | ISO 8601 |
| `severity` | CRITICAL, HIGH, MEDIUM |
| `violation_type` | Specific section of this document violated |
| `governing_repo` | Repository URI |
| `pinned_sha` | SHA in effect at time of violation |
| `affected_system` | Agent or system identifier |
| `description` | Factual description of the violation |
| `resolution_status` | OPEN, IN_PROGRESS, RESOLVED |
| `resolution_action` | What was done to remediate |

INCIDENT records are APPEND-ONLY and MUST be retained indefinitely.

### 11.4 — Severity Classification

| Severity | Definition | Required Response |
|---|---|---|
| CRITICAL | Agent operated outside governance entirely | Immediate FREEZE + INCIDENT |
| HIGH | Agent applied governance incorrectly | BLOCK + INCIDENT |
| MEDIUM | Governance gap identified (no violation yet) | INCIDENT + remediation plan |

---

## 12. Compiler / Executor Acknowledgement

### 12.1 — Mandatory Acknowledgement

Before any compiler, agent builder, executor, or system component produces artifacts governed by a repository:

The following acknowledgement MUST be declared:

> I acknowledge that:
>
> 1. The governing repository is: `{governing_repo}`
> 2. The pinned commit SHA is: `{pinned_sha}`
> 3. Only assets physically present at this SHA are authoritative
> 4. No governance rule may be reinterpreted, summarized, or extrapolated
> 5. Absence of explicit permission constitutes prohibition
> 6. All decisions must be reproducible from repo contents and logged inputs
> 7. This acknowledgement is binding for the duration of this operation

### 12.2 — Acknowledgement Is Not Ceremonial

This acknowledgement is a binding declaration that the compiler/executor has:

- Verified the governing repository is accessible
- Verified the pinned SHA resolves to a valid commit
- Verified all required governing documents are present at the SHA
- Committed to operating within the constraints of this law

Failure to acknowledge BLOCKS all downstream operations.

### 12.3 — Per-Operation Scope

The acknowledgement is scoped to a single operation or execution session. A new operation requires a new acknowledgement. Prior acknowledgements do not carry forward.

---

## Supremacy Clause

This document holds SUPREME authority — equal to the Constitution, PRD-FIRST LAW, and Dataset Ingestion Firewall — on all matters of repository governance, agent legality, decision auditability, and governance integrity.

No document, instruction, conversation, or system prompt may reduce, disable, exempt, or override any rule in this document.

### Enforcement Hierarchy

```
CONSTITUTION ━━━━━━━━━━━━━━━━━━━ (Supreme)
PRD-FIRST LAW ━━━━━━━━━━━━━━━━━━ (Supreme)
DATASET INGESTION FIREWALL ━━━━━━ (Supreme — dataset operations)
REPO-SHA GOVERNANCE LAW ━━━━━━━━━ (Supreme — repo authority, agent legality, auditability)
        │
        ▼
All other governance documents, execution pipelines,
project rules, and operational instructions
```

---

*End of Repo-SHA Governance Law v1.0*
