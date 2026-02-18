# UNIVERSAL HARDENING DOCTRINE

## Anti-Recurrence System · All Projects · All Domains

**Version:** 1.0  
**Date:** 2026-02-10  
**Authority Level:** SUPREME — Equal to Constitution, Universal Workflow Governance, and Repo-SHA Governance Law  
**Origin:** Extracted from COEE v1.4 Governance Hardening Report GH-001. Every law in this document exists because a system failed.  
**Enforcement:** Mandatory across ALL NexumOps projects, repositories, agents, pipelines, and systems  
**Immutability:** Versioned changes only, with documented justification and system owner approval.

---

## Table of Contents

1. [Purpose & Scope](#1-purpose--scope)
2. [n8n Expression & Data Flow Laws](#2-n8n-expression--data-flow-laws)
3. [n8n Infrastructure & URL Laws](#3-n8n-infrastructure--url-laws)
4. [n8n Switch & IF Schema Laws](#4-n8n-switch--if-schema-laws)
5. [Messaging Platform Laws](#5-messaging-platform-laws)
6. [Outbound Message Branding Laws](#6-outbound-message-branding-laws)
7. [Conversational Interface Laws](#7-conversational-interface-laws)
8. [Threshold & Constant Governance Laws](#8-threshold--constant-governance-laws)
9. [Observational Subsystem Laws](#9-observational-subsystem-laws)
10. [Dataset Lifecycle Laws](#10-dataset-lifecycle-laws)
11. [Kill-Switch & Circuit Breaker Laws](#11-kill-switch--circuit-breaker-laws)
12. [Variation & Content Generation Laws](#12-variation--content-generation-laws)
13. [Rate Limiting & Domain Protection Laws](#13-rate-limiting--domain-protection-laws)
14. [SplitInBatches & Loop Laws](#14-splitinbatches--loop-laws)
15. [Universal Pre-Deploy Hardening Checklist](#15-universal-pre-deploy-hardening-checklist)
16. [Project Hardening Report Template](#16-project-hardening-report-template)
17. [Governance Evolution Protocol](#17-governance-evolution-protocol)
18. [Supremacy Clause](#18-supremacy-clause)

---

## 1. Purpose & Scope

### Why This Document Exists

During the hardening of COEE v1.4, **23 distinct failure classes** were discovered — all of which had passed JSON validation, n8n import, and expression checks. Every failure was silent at build time and catastrophic at runtime. None were caught by existing governance documents.

This document encodes every discovered failure pattern as a universal, project-agnostic law. Its purpose is to ensure that no NexumOps project ever encounters these failure classes again — regardless of domain, product, or workflow engine version.

### What This Document Governs

Every n8n workflow, agent, automation, pipeline, and system component across ALL NexumOps projects, including but not limited to:

- Recepcionista IA (AI Receptionist)
- COEE (Cold Outbound Email Engine)
- UPA (Universal Provisioning Agent)
- Outbound sales automation engines
- Any future project

### Relationship to Existing Governance

This document operates at the same level as UNIVERSAL_WORKFLOW_GOVERNANCE.md. Where UWG defines abstract laws (e.g., "all inputs are hostile"), this document defines **concrete, verifiable, lintable implementation rules** derived from observed production failures.

```
CONSTITUTION ━━━━━━━━━━━━━━━━━━━ (Supreme)
PRD-FIRST LAW ━━━━━━━━━━━━━━━━━━ (Supreme)
DATASET INGESTION FIREWALL ━━━━━━ (Supreme — dataset operations)
REPO-SHA GOVERNANCE LAW ━━━━━━━━━ (Supreme — repo authority)
UNIVERSAL WORKFLOW GOVERNANCE ━━━━ (Supreme — abstract anti-failure laws)
UNIVERSAL HARDENING DOCTRINE ━━━━ (Supreme — concrete anti-recurrence rules) ← THIS FILE
        │
        ▼
Project-specific governance, PRDs, execution pipelines
```

---

## 2. n8n Expression & Data Flow Laws

### Origin Failure

COEE v1.4: All notification nodes after IF branches used `$('NodeName').item.json.field`. Item pairing broke on false branches. Runtime crash: "Paired item data for item from node 'X' is unavailable." Silent at build, import, and expression validation.

### UHD-EXPR-001 — `.item` Accessor Is Forbidden

The `.item` accessor on `$('NodeName')` is **FORBIDDEN** in all production workflows.

**Why:** `.item` relies on n8n's paired-item tracking, which silently breaks after IF/Switch branches, inside SplitInBatches loops, and on SplitInBatches "done" outputs. The failure is invisible at build time.

**Required alternative:**
- For **post-branch nodes** (after IF, Switch, Router): Use `$json.field` — data flows through branches naturally
- For **Code nodes referencing upstream single-item nodes**: Use `$('NodeName').first().json.field`
- For **in-loop nodes**: Use `$json.field` — data must be propagated through loop items

**Sole permitted exception:** `.first()` may be used in Code nodes where `$json` is unavailable and the upstream node is guaranteed to produce exactly one item (e.g., Init/Config nodes before any branch or loop). This exception MUST be documented per-node.

### UHD-EXPR-002 — Post-Branch Data Access

After ANY IF, Switch, or Router node, downstream nodes MUST access data exclusively through `$json`.

**Why:** When an IF node routes to the false branch, nodes on that branch cannot access items from nodes that only executed on the true branch (and vice versa). The `$json` object carries all data that flowed through the active branch.

**Verification:** Every node after a branch point must be checked: if it contains `$('NodeName').item`, it is a pre-deployment BLOCK.

### UHD-EXPR-003 — Branch Data Contracts

Every branch of every IF/Switch/Router MUST produce a **complete data contract** — the full set of fields required by all downstream nodes on that branch.

**Why:** If Branch A produces `{chatId, reason}` but Branch B only produces `{chatId}`, any downstream node expecting `reason` will crash on Branch B.

**Required:** At the output of every branch, the fields available MUST be documented. Missing fields MUST either be added (with safe defaults) or the downstream node MUST guard against their absence.

### UHD-EXPR-004 — Expression Reference Validation

Every `$('NodeName')` reference in any expression MUST be validated against the workflow's actual nodes array. A reference to a node that does not exist is a **CRITICAL** pre-deployment block.

**Verification:** Automated scan of all expressions → extract all `$('NodeName')` patterns → verify each NodeName exists in `workflow.nodes[].name`.

### UHD-EXPR-005 — No Orphaned Expression Chains

If Node A feeds Node B feeds Node C through expressions, and Node A is removed or renamed, Nodes B and C silently receive `undefined`. 

**Required:** When any node is renamed or removed, ALL expressions referencing that node MUST be updated. This is not optional cleanup — it is a pre-deployment gate.

---

## 3. n8n Infrastructure & URL Laws

### Origin Failure

COEE v1.4: Supabase HTTP Request nodes used a Set node with `options: {}` as "Config." The expression `$('Config').item.json.supabaseUrl` resolved to `undefined`, producing URLs like `undefined/rest/v1/system_config`. 15 HTTP nodes across 2 workflows were affected.

### UHD-URL-001 — Infrastructure URLs Must Be Absolute and Verified

Every HTTP Request node MUST use a complete, absolute URL. URLs constructed from expressions MUST be verified to produce valid absolute URLs before deployment.

**Forbidden patterns:**
- Relative URLs (`/rest/v1/table`)
- Expression-only URLs where the base resolves to `undefined` or empty string
- URLs derived from Set nodes with empty `options: {}`
- URLs containing `undefined`, `null`, or `NaN` as string segments

### UHD-URL-002 — Init Node Pattern (Mandatory)

Every workflow that makes HTTP requests to external services MUST have a dedicated **Init Code node** at the beginning of the workflow (after trigger, before any branch) that:

1. Declares all base URLs as explicit string constants
2. Outputs them as named fields in `$json`
3. Does NOT rely on credentials, environment variables, or upstream data for URL construction

**Example pattern:**
```javascript
return [{
  json: {
    supabase_rest_base: 'https://xxxx.supabase.co/rest/v1',
    supabase_auth_base: 'https://xxxx.supabase.co/auth/v1',
    api_base: 'https://api.example.com/v1'
  }
}];
```

### UHD-URL-003 — URL Propagation Through Branches

When a workflow has IF/Switch branches, the Init node's URL fields MUST propagate through ALL branches. Post-branch nodes access URLs via `$json.field_name`.

**Verification:** For every HTTP Request node in the workflow, trace the URL source backward. If the trace crosses a branch point and uses `$('Init').item`, it is a BLOCK.

### UHD-URL-004 — URL Propagation Through Loops

Inside SplitInBatches loops, URLs MUST be embedded in each loop item's data (see Section 14). Post-loop nodes use `.first()` to access the last item's data, which must include URLs.

### UHD-URL-005 — Single Source of URL Truth

Each external service MUST have exactly ONE Init node that defines its URLs. If multiple workflows target the same service, each workflow has its own Init node with the same values. Cross-workflow URL references are FORBIDDEN.

**Exception:** A second URL source is permitted ONLY as a documented "belt-and-suspenders" re-injection — when a node in the middle of the pipeline (e.g., an email sender) strips input fields and downstream nodes need URLs. This exception MUST be documented per-node with the reason.

---

## 4. n8n Switch & IF Schema Laws

### Origin Failure

COEE v1.4: All 3 Switch nodes were generated missing the `conditions.options` block. n8n attempts `rule.conditions.options.caseSensitive` → `undefined.caseSensitive` → runtime crash. Passed JSON validation, import, and expression checks silently.

### UHD-SWITCH-001 — Switch Node Canonical Schema

Every Switch node (v3.x) MUST include the `conditions.options` block inside every rule's conditions:

```json
{
  "conditions": {
    "options": {
      "caseSensitive": true,
      "leftValue": "",
      "typeValidation": "strict"
    },
    "conditions": [
      {
        "leftValue": "={{ $json.field }}",
        "rightValue": "expected_value",
        "operator": {
          "type": "string",
          "operation": "equals"
        }
      }
    ],
    "combinator": "and"
  }
}
```

A Switch rule missing `conditions.options` is a **CRITICAL** pre-deployment block.

### UHD-SWITCH-002 — Switch Structure Key

Switch nodes MUST use `rules.values[]` — NOT `rules.rules[]`. The `rules.rules` key is a known schema variant that causes silent import followed by runtime failure.

**Verification:** Search all Switch nodes for the key `rules.rules`. If found, BLOCK.

### UHD-SWITCH-003 — Switch Fallback Output (Mandatory)

Every Switch node MUST have `fallbackOutput: "extra"` configured with a connected output. A Switch node without a connected fallback is a pre-deployment BLOCK.

### UHD-IF-001 — IF Node Canonical Schema

Every IF node (v2.x) MUST include `conditions.options` and `conditions.combinator`:

```json
{
  "conditions": {
    "options": {
      "caseSensitive": true,
      "leftValue": "",
      "typeValidation": "strict"
    },
    "conditions": [
      {
        "leftValue": "={{ $json.field }}",
        "rightValue": true,
        "operator": {
          "type": "boolean",
          "operation": "true"
        }
      }
    ],
    "combinator": "and"
  }
}
```

### UHD-IF-002 — IF Branch Completeness

Both TRUE and FALSE branches of every IF node MUST have connected outputs. An IF node with a disconnected branch is a pre-deployment BLOCK unless the disconnected branch is **explicitly documented** as a terminal path (intentional drop).

---

## 5. Messaging Platform Laws

### Origin Failure

COEE v1.4: All 8 Telegram nodes used `parse_mode: "Markdown"` (Legacy). Dynamic content containing underscores (`AUTH_DENIED: User 12345_67890`) caused Telegram to reject entire messages: "Can't parse entities." Silent at build, crash at runtime.

### UHD-MSG-001 — Markdown Legacy Is Forbidden

`parse_mode: "Markdown"` (Legacy) is **FORBIDDEN** on ALL Telegram nodes in ALL projects. Legacy Markdown interprets underscores, asterisks, brackets, and backticks as formatting entities, causing rejection of any message containing these characters in dynamic content.

**Required alternatives:**
- `parse_mode: "HTML"` — for messages requiring formatting
- No `parse_mode` (plain text) — for simple notifications

### UHD-MSG-002 — HTML Is the Standard Parse Mode

ALL Telegram nodes that send messages MUST use `parse_mode: "HTML"` as the standard mode. This applies to:

- Notification messages
- Report/summary messages
- Alert messages
- Error messages
- User-facing conversational messages

### UHD-MSG-003 — Dynamic Content Must Be Escaped

ALL dynamic values inserted into Telegram messages MUST be HTML-escaped before insertion. The following characters MUST be escaped:

| Character | Escape |
|-----------|--------|
| `&` | `&amp;` |
| `<` | `&lt;` |
| `>` | `&gt;` |

**Implementation patterns:**

For **simple messages** (1-3 dynamic fields): Inline expression escaping is permitted:
```
{{ $json.field.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;') }}
```

For **complex messages** (4+ dynamic fields or report-style): A Code node MUST implement an `esc()` function:
```javascript
function esc(s) {
  if (s == null) return '';
  return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
}
```

### UHD-MSG-004 — Error Messages Are the Most Dangerous Field

Error messages from n8n runtime, external APIs, and database operations can contain ANY characters — including XML/HTML fragments, stack traces, and arbitrary user input. 

**Required:** The `error_message` field (or equivalent) in error notification workflows MUST ALWAYS be escaped, regardless of whether the message uses formatting. Unescaped error messages will eventually crash the notification system, creating a meta-failure where the error handler itself fails.

### UHD-MSG-005 — Platform-Specific Message Testing

Before deploying any workflow that sends messages to users, every message template MUST be tested with adversarial content:

| Test Input | Purpose |
|------------|---------|
| `user_name_with_underscores` | Catches Markdown italic parsing |
| `value with <angle> brackets` | Catches HTML injection / parsing |
| `text with * asterisks *` | Catches Markdown bold parsing |
| `error: Cannot read property 'x' of undefined` | Catches error message injection |
| `línea con ñ, tildes: áéíóú` | Catches encoding issues |
| Empty string `""` | Catches null/empty rendering |
| Very long string (500+ chars) | Catches platform truncation |

---

## 6. Outbound Message Branding Laws

### Origin Failure

COEE v1.4: All Telegram nodes sent messages with default n8n attribution footer: "This message was sent automatically with n8n." Leaked internal tooling, unprofessional for operator-facing communications. `appendAttribution` defaults to `true` when not explicitly set.

### UHD-BRAND-001 — Attribution Must Be Explicitly Suppressed

ALL messaging nodes (Telegram, WhatsApp, Slack, Email) that have an `appendAttribution` or equivalent parameter MUST explicitly set it to `false`.

**Why:** Most workflow engines default to injecting their branding. This default is invisible in the UI but present in every outbound message.

### UHD-BRAND-002 — No Internal System Names in User-Facing Messages

User-facing messages MUST NOT contain:

- Workflow engine names (n8n, Make, Zapier)
- AI model names (Claude, GPT, Gemini) — unless explicitly required by PRD
- Internal workflow names or node names
- Internal system identifiers
- Debug information or stack traces
- Credential names or API endpoint URLs

### UHD-BRAND-003 — Forbidden String Scan

Before deployment, ALL message content (static text AND expression-generated text) MUST be scanned for forbidden strings:

| Forbidden String | Category |
|------------------|----------|
| `n8n` | Tooling leak |
| `workflow` | System internal |
| `node` (as system reference) | System internal |
| `execution` (as system reference) | System internal |
| `Claude` / `GPT` / `OpenAI` / `Anthropic` | AI attribution (unless PRD-required) |
| `error:` / `TypeError` / `undefined` | Debug leak |
| `localhost` / `127.0.0.1` | Infrastructure leak |
| `.supabase.co` | Infrastructure leak |

### UHD-BRAND-004 — New Messaging Node Verification

When ANY new messaging node is added to ANY workflow, the deployer MUST verify:

1. `appendAttribution` is explicitly set to `false`
2. Message content contains no forbidden strings
3. All dynamic fields are escaped for the target platform
4. Parse mode is explicitly set (not relying on default)

---

## 7. Conversational Interface Laws

### Origin Failure

COEE v1.4: No `/start` command handler existed — bot processed ALL messages as campaign input. Non-whitelisted users received generic "Campaign blocked" with no explanation. No language selection. No instructions. No attribution explanation.

### UHD-CONV-001 — Every Bot Must Handle /start

Every Telegram bot (or equivalent entry point for other platforms) MUST implement a `/start` command handler that:

1. Detects the `/start` command before any business logic
2. Routes to an onboarding sequence
3. Does NOT process `/start` as business input

### UHD-CONV-002 — Language Gate (Multi-Lingual Systems)

Systems serving users in multiple languages MUST implement a language selection gate as the FIRST step of onboarding. The selected language MUST be persisted and used for all subsequent communications.

### UHD-CONV-003 — Authorization Denial Must Be Explanatory

When a user is denied access (not whitelisted, not authorized, suspended), the denial message MUST include:

1. A clear explanation of WHY access is denied
2. The user's identifier (so they can reference it when requesting access)
3. Instructions for how to request access
4. A mechanism to re-check authorization (callback button or command)

**Forbidden:** Generic "blocked" or "access denied" messages without explanation.

### UHD-CONV-004 — Instructions Must Be Embedded

User instructions for how to use the system MUST be delivered within the conversational interface itself — not in external documentation only. Users cannot be expected to read external docs before interacting.

### UHD-CONV-005 — Attribution Transparency

If a system tracks user activity (campaigns, requests, actions tied to user ID), the user MUST be informed of this tracking during onboarding. This is not optional — it is a trust and compliance requirement.

---

## 8. Threshold & Constant Governance Laws

### Origin Failure

COEE v1.4: Metric thresholds (delivery_rate ≥95%, kill-switch <90%, spam <0.1%, rate limit 5/day, delay 30s, loop interval 72h) were scattered across PRD, Claude Code prompt, conversion-model.md, and workflow JSON. No single source of truth. Inconsistency between sources was inevitable.

### UHD-CONST-001 — Single Source of Constants

Every project MUST have a single `{PROJECT}_Configurable_Constants.md` file that contains ALL numeric thresholds, limits, intervals, ratios, and magic numbers used by any component of the system.

**Required fields per constant:**

| Field | Purpose |
|-------|---------|
| Constant Name | Machine-readable identifier |
| Value | The numeric value |
| Unit | What the number represents (hours, ratio, count, seconds) |
| Referenced By | Which doctrine files use this constant |
| Last Updated | Date of last modification |

### UHD-CONST-002 — No Hardcoded Constants in Prompts or PRDs

System prompts, PRDs, workflow JSON, and Code nodes MUST NOT contain hardcoded numeric thresholds. They MUST reference the Configurable Constants file.

**Why:** When a threshold appears in 4 places, changing it requires updating 4 files — and forgetting one creates inconsistent behavior that is invisible until it causes a failure.

**Permitted exception:** A workflow Code node may contain a constant as a runtime value IF the node includes a comment citing the source: `// Source: {PROJECT}_Configurable_Constants.md — delivery_rate_critical: 0.90`

### UHD-CONST-003 — Constant Modification Protocol

Modifying any constant requires:

1. Updating the `{PROJECT}_Configurable_Constants.md` file with new value + date
2. Updating every file listed in "Referenced By"
3. Version bump to the constants file
4. Verification that no other file contains the old value

---

## 9. Observational Subsystem Laws

### Origin Failure

COEE v1.4: Learning loop could "generate recommendations" as free text — ungoverned outputs that could be interpreted as behavioral directives. No promotion protocol existed for converting observations into rules.

### UHD-OBS-001 — Observational Systems Are Read-Only

Any subsystem classified as "observational" (learning loops, analytics engines, monitoring agents) is **READ-ONLY by definition**. It MAY compute, aggregate, log, and report. It MUST NOT modify any:

- System rule or constraint
- Workflow parameter or threshold
- Governance document
- Agent behavior
- User-facing message content
- Routing logic

### UHD-OBS-002 — No Free-Text Recommendations

Observational systems MUST NOT output free-text recommendations. All outputs MUST be structured:

```json
{
  "finding": "string — what was observed",
  "severity": "INFO | WARNING | CRITICAL",
  "proposed_action": "string — what could be done",
  "requires_doctrine_change": true | false
}
```

**Why:** Free-text recommendations create a vector for observation systems to influence behavior without governance. Structured output constrains the observation to a format that can be validated, logged, and safely ignored.

### UHD-OBS-003 — Observation-to-Rule Promotion Protocol

An observation MAY become a system rule ONLY if ALL conditions are met:

1. The observation is confirmed across ≥3 consecutive analysis cycles
2. A human operator reviews and approves the promotion
3. The promoted rule is written into a versioned doctrine file
4. The promoted rule passes DIF validation (if derived from a dataset)
5. No automated promotion is permitted — ever

### UHD-OBS-004 — Observational Output Logging

All observational outputs MUST be logged with:

- Timestamp
- Analysis cycle identifier
- Input data summary (NOT raw PII)
- Findings (structured per UHD-OBS-002)
- Whether any finding was promoted (and by whom)

---

## 10. Dataset Lifecycle Laws

### Origin Failure

COEE v1.4: A placeholder dataset with corrupted encoding (`Default_┬í­ƒÜ¿Evita que tus correos...`) was referenced during development. No deprecation tracking existed. The new canonical dataset was not referenced by exact filename in any artifact. No rule traced to a specific dataset file.

### UHD-DATA-001 — Dataset Deprecation Log (Mandatory)

Every project MUST maintain a `{PROJECT}_Dataset_Deprecation_Log.md` with an append-only record of:

| Field | Purpose |
|-------|---------|
| Date | When the action occurred |
| Action | REPLACEMENT, DEPRECATION, VERSION_UPDATE |
| Old Dataset | Exact filename of deprecated dataset |
| New Dataset | Exact filename of replacement (if applicable) |
| Affected Doctrines | Which doctrine files must be updated |
| Verified By | Who confirmed the update was complete |

### UHD-DATA-002 — No Phantom Datasets

A dataset that is referenced by name in any system artifact MUST physically exist at the referenced location. A reference to a non-existent or deprecated dataset is a pre-deployment BLOCK.

### UHD-DATA-003 — Rule-to-Dataset Traceability

Every rule enforced by any governance engine, validator, or constraint system MUST be traceable to a specific file and section in the repository. A rule that cannot be traced to a source is **unauthorized** and MUST be removed from the enforcement system.

**Forbidden sources:**
- "Best practice"
- "Industry standard"
- "Inferred from context"
- "Common sense"

### UHD-DATA-004 — Placeholder Detection

Before deployment, ALL artifact text MUST be scanned for:

- Encoding-corrupted filenames
- References to files in `99_Archive/` or deprecated paths
- Filenames containing non-UTF-8 characters
- References that don't match any existing file exactly (case-sensitive)

---

## 11. Kill-Switch & Circuit Breaker Laws

### Origin Failure

COEE v1.4: Kill-switch activation logic was defined in 3 places (PRD, Claude Code prompt, conversion-model.md). Kill-switch deactivation protocol did NOT exist — no doctrine for how or when to turn it off.

### UHD-KILL-001 — Kill-Switch Must Have Activation AND Deactivation

Every system with a kill-switch or circuit breaker MUST define BOTH:

1. **Activation conditions** — exact thresholds and triggers
2. **Deactivation protocol** — who can deactivate, how, and what happens after

A kill-switch without a deactivation protocol creates a system that can permanently disable itself with no recovery path.

### UHD-KILL-002 — Kill-Switch Definition Must Be Single-Source

Kill-switch logic MUST be defined in exactly ONE doctrine file. All other references (PRD, prompts, workflows) MUST cite that file. Contradictions between sources are a BLOCK.

### UHD-KILL-003 — Kill-Switch Deactivation Is Manual-Only (Default)

Unless explicitly overridden by PRD, kill-switch deactivation MUST be manual — requiring a human operator to explicitly re-enable the system. No automated recovery from kill-switch state.

**Why:** If the system could auto-recover, the kill-switch is not a kill-switch — it's a pause button. Kill-switches exist for conditions serious enough that a human must verify the system is safe before resuming.

### UHD-KILL-004 — Kill-Switch State Must Be Persisted

Kill-switch state MUST be stored in durable storage (database), NOT in workflow variables or in-memory state. A workflow restart MUST NOT clear the kill-switch.

---

## 12. Variation & Content Generation Laws

### Origin Failure

COEE v1.4: Variation dimensions were defined in template files, not doctrine. No constraints on what constitutes a "sufficiently different" variation. No maximum/minimum variation counts. "MUST NOT violate any repo rule" stated but which rules was unmapped.

### UHD-VAR-001 — Variation Dimensions Must Be Closed Lists

Any system that generates variations (email copy, message text, content alternatives) MUST define:

1. **Permitted variation dimensions** — exhaustive, closed list of what MAY be varied
2. **Forbidden variation actions** — exhaustive, closed list of what MUST NOT be varied
3. **Sufficiency criteria** — what constitutes a "sufficiently different" variation

These definitions MUST exist in a versioned doctrine file, NOT in templates or prompts.

### UHD-VAR-002 — Variation Constraints Must Trace to Rules

Every constraint on generated content ("must not add claims," "must not change CTA type") MUST trace to a specific rule in the project's governance matrix. Constraints that exist only as prompt instructions without doctrine backing are **ungoverned**.

### UHD-VAR-003 — No Unbounded Generation

Content generation systems MUST have explicit bounds:

- Maximum output length
- Maximum number of variations per input
- Minimum difference threshold between variations
- Explicit list of forbidden content patterns

---

## 13. Rate Limiting & Domain Protection Laws

### Origin Failure

COEE v1.4: No cool-down period between campaigns to the same domain. No domain-level sending constraints. Only global dedup existed — same recipient couldn't be contacted twice, but their domain could be flooded.

### UHD-RATE-001 — Multi-Level Rate Limiting

Systems that send outbound communications MUST implement rate limits at multiple levels:

| Level | Purpose | Example |
|-------|---------|---------|
| Per-user | Prevent individual abuse | 5 campaigns/day |
| Per-recipient | Prevent harassment | 1 contact per 30 days |
| Per-domain | Protect domain reputation | Max N emails to same domain per campaign |
| Per-system | Prevent global overload | Total daily send limit |

### UHD-RATE-002 — Domain-Level Constraints

When sending to multiple recipients at the same organization (domain), the system MUST enforce:

1. Maximum emails to the same domain per single campaign
2. Maximum campaigns targeting the same domain within a rolling window
3. These constraints apply regardless of intent classification or priority level

### UHD-RATE-003 — Rate Limit Definitions in Constants

All rate limit values MUST be defined in the project's Configurable Constants file, NOT hardcoded in workflow logic.

---

## 14. SplitInBatches & Loop Laws

### Origin Failure

COEE v1.4: 6 HTTP Request nodes inside SplitInBatches referenced `$('Init Supabase Config').item` — all crashed at runtime. Post-loop "done" output used 8 `.item` references — all crashed. The entire send-loop and post-loop pipeline was non-functional.

### UHD-LOOP-001 — No `.item` Inside Loops

Inside a SplitInBatches loop (between the SplitInBatches node and its loop-back connection), the `.item` accessor is **FORBIDDEN**. All data access MUST use `$json`.

**Why:** SplitInBatches breaks n8n's paired-item tracking. `$('NodeName').item` inside a loop cannot resolve which item in the batch corresponds to which upstream item.

### UHD-LOOP-002 — Data Propagation Through Loop Items

Any data needed inside the loop (URLs, configuration, context) MUST be embedded in each loop item BEFORE the SplitInBatches node processes them.

**Pattern:**
1. Code node before SplitInBatches adds required fields to each item
2. Inside loop, nodes access these fields via `$json.field_name`
3. If a node inside the loop strips fields (e.g., HTTP Send Email), the NEXT node must re-inject required fields

### UHD-LOOP-003 — No `.item` on SplitInBatches "Done" Output

The SplitInBatches "done" output emits when all batches are processed. At this point, paired-item tracking is completely broken for ALL upstream nodes.

**Required:** Post-loop nodes MUST use `.first()` to access upstream single-item nodes. All expressions in post-loop nodes MUST be rewritten from `$('Node').item` to `$('Node').first()`.

### UHD-LOOP-004 — Belt-and-Suspenders URL Re-Injection

When a node inside a loop (e.g., an email sender, an API call) strips its input fields from its output, the immediately following node MUST re-inject any required fields (especially URLs) from a known source — either from `$json` of the preceding node's input or from a hardcoded constant.

**Why:** Sending nodes often output only their own response, dropping all input fields. If the next node needs the Supabase URL to log the result, it must get it from somewhere.

### UHD-LOOP-005 — Loop Bounds

Every loop MUST have explicit bounds:

- SplitInBatches: batch size is explicitly configured
- No external re-entry points that could create unbounded recursion
- Retry loops: maximum retry count is explicitly set

---

## 15. Universal Pre-Deploy Hardening Checklist

This checklist EXTENDS (does not replace) the Pre-Deploy Hard Gate Checklist in UNIVERSAL_WORKFLOW_GOVERNANCE.md Section 8. Every item is PASS/FAIL. A single FAIL blocks deployment.

### 15.1 — Expression & Data Flow

| # | Check | Source Law |
|---|-------|------------|
| H-001 | Zero `.item` accessors remain in the workflow (search: `.item.json`, `.item.`) | UHD-EXPR-001 |
| H-002 | All post-branch nodes use `$json` for data access | UHD-EXPR-002 |
| H-003 | All `$('NodeName')` references resolve to existing nodes | UHD-EXPR-004 |
| H-004 | Pre-loop Code nodes use `.first()` only, documented per-node | UHD-EXPR-001 |

### 15.2 — Infrastructure & URLs

| # | Check | Source Law |
|---|-------|------------|
| H-005 | Init Code node exists with absolute URL constants | UHD-URL-002 |
| H-006 | No HTTP Request URL contains `undefined`, `null`, or empty base | UHD-URL-001 |
| H-007 | URL propagation verified through all branches | UHD-URL-003 |
| H-008 | URL propagation verified through all loops | UHD-URL-004 |

### 15.3 — Switch & IF Schema

| # | Check | Source Law |
|---|-------|------------|
| H-009 | All Switch nodes have `conditions.options` in every rule | UHD-SWITCH-001 |
| H-010 | All Switch nodes use `rules.values[]` — no `rules.rules[]` | UHD-SWITCH-002 |
| H-011 | All Switch nodes have `fallbackOutput: "extra"` with connected output | UHD-SWITCH-003 |
| H-012 | All IF nodes have `conditions.options` and `conditions.combinator` | UHD-IF-001 |
| H-013 | All IF nodes have both TRUE and FALSE branches connected (or documented terminal) | UHD-IF-002 |

### 15.4 — Messaging & Branding

| # | Check | Source Law |
|---|-------|------------|
| H-014 | Zero Telegram nodes use `parse_mode: "Markdown"` (Legacy) | UHD-MSG-001 |
| H-015 | All Telegram nodes use `parse_mode: "HTML"` | UHD-MSG-002 |
| H-016 | All dynamic content in messages is HTML-escaped | UHD-MSG-003 |
| H-017 | All messaging nodes have `appendAttribution: false` | UHD-BRAND-001 |
| H-018 | No forbidden branding strings in any message content | UHD-BRAND-003 |

### 15.5 — Loops & Batches

| # | Check | Source Law |
|---|-------|------------|
| H-019 | Zero `.item` inside SplitInBatches loops | UHD-LOOP-001 |
| H-020 | Required data propagated into loop items before SplitInBatches | UHD-LOOP-002 |
| H-021 | Post-loop nodes use `.first()` exclusively | UHD-LOOP-003 |
| H-022 | URL re-injection after field-stripping nodes documented | UHD-LOOP-004 |

### 15.6 — Governance & Constants

| # | Check | Source Law |
|---|-------|------------|
| H-023 | All thresholds trace to Configurable Constants file | UHD-CONST-001 |
| H-024 | Kill-switch has both activation AND deactivation protocol | UHD-KILL-001 |
| H-025 | Kill-switch defined in single source | UHD-KILL-002 |
| H-026 | No deprecated/placeholder datasets referenced | UHD-DATA-002 |
| H-027 | All governance rules trace to specific source files | UHD-DATA-003 |

### 15.7 — Conversational (if applicable)

| # | Check | Source Law |
|---|-------|------------|
| H-028 | /start command handler exists | UHD-CONV-001 |
| H-029 | Language gate implemented (multi-lingual systems) | UHD-CONV-002 |
| H-030 | Authorization denial includes explanation + re-check mechanism | UHD-CONV-003 |

### Gate Decision

```
IF all applicable checks = PASS → DEPLOY PERMITTED
IF any check = FAIL           → DEPLOY BLOCKED
                                 Report: check ID, failure description, source law, required fix
```

---

## 16. Project Hardening Report Template

Every project MUST produce a Hardening Report before production deployment, using this structure:

```markdown
# {PROJECT} v{X.Y} — GOVERNANCE HARDENING REPORT

Version: GH-{NNN}
Date: {YYYY-MM-DD}
Authority: CONSTITUTION.md + UNIVERSAL_HARDENING_DOCTRINE.md v1.0

---

## 1. GOVERNANCE STATUS BEFORE

### {Category 1}
- [List ungoverned behaviors discovered]

### {Category N}
- [List ungoverned behaviors discovered]

---

## 2. NEW / UPDATED DOCTRINE FILES

### FILE 1: `{path}`
**Purpose:** ...
**Scope:** ...
**Behaviors Governed:** ...

---

## 3. DATASET UPDATE LOG
[If applicable — deprecations, replacements, version changes]

---

## 4. GOVERNANCE STATUS AFTER
[Table: Behavior | Governed By | Status]

---

## 5. GOVERNANCE GAP REPORT — BEFORE vs AFTER
[Table: Behavior | Before | After]

---

## 6. HARDENING CHECKLIST COMPLIANCE
[Execute Universal Pre-Deploy Hardening Checklist — all 30 items]

---

## 7. REMAINING GAPS
[What cannot be closed without human action]

---

## 8. ONE-SHOT READINESS ASSESSMENT
[READY / CONDITIONALLY READY / NOT READY — with specific blockers]
```

---

## 17. Governance Evolution Protocol

### UHD-EVO-001 — Every New Failure Becomes a Law

When a production failure occurs that is NOT covered by this document:

1. Document the failure (root cause, system, impact)
2. Abstract to the general failure class
3. Write the universal law that prevents recurrence
4. Add to this document in the appropriate section
5. Add corresponding check(s) to the Pre-Deploy Hardening Checklist
6. Audit ALL existing projects against the new law

### UHD-EVO-002 — Cross-Project Propagation

When a new law is added from Project A's failure:

- ALL other projects MUST be audited against the new law
- Projects that predate the law are NOT exempt
- The audit may be deferred but MUST NOT be skipped

### UHD-EVO-003 — Failure Report Format

Every failure submitted to this governance loop MUST follow:

```
FAILURE REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
failure_id:          UHD-FAIL-{NNN}
date:                {ISO 8601}
project:             {project name}
system:              {workflow / agent / pipeline}
component:           {specific node / expression / config}
failure_class:       FIXABLE_AUTOMATICALLY | HUMAN_ACTION_REQUIRED
root_cause:          {specific technical cause}
general_pattern:     {abstract failure class}
existing_law_gap:    {which UHD section should have prevented it, or "NEW"}
proposed_law:        {exact law text}
proposed_check:      {exact checklist item}
checklist_id:        H-{NNN}
retroactive_impact:  {list of projects potentially affected}
```

### UHD-EVO-004 — Version Control

Every change to this document MUST include:

- Version number increment
- Date of change
- Failure report ID that triggered the change
- Exact diff (sections added/modified)
- System owner acknowledgment

---

## 18. Supremacy Clause

### Authority Declaration

This document holds **SUPREME authority** — equal to the Constitution, Universal Workflow Governance, and Repo-SHA Governance Law — on all matters of production hardening, failure prevention, and pre-deployment validation.

### What This Document Overrides

| Override Target | Ruling |
|---|---|
| "It worked in testing" | Testing environments are sanitized. This document governs production reality. |
| "It imports fine" | Import success does not equal runtime success. Every law here addresses import-safe failures. |
| "The schema looks correct" | Missing `conditions.options` looks correct too. Verify, don't assume. |
| Speed / deadline pressure | No check may be skipped for time. |
| "It's just one node" | One `.item` accessor crashed an entire pipeline. |
| "We'll fix it after launch" | There is no "after launch" for one-shot systems. |

### Enforcement

- A single FAIL on the Hardening Checklist blocks deployment
- No partial compliance
- No "conditional deployment pending fix"
- No verbal overrides

### Immutability

This document may only be modified through:

- A versioned commit with documented justification
- Evidence from a production failure that reveals a gap
- System owner approval
- Re-audit of all active projects against the new version

---

*End of UNIVERSAL HARDENING DOCTRINE v1.0*
