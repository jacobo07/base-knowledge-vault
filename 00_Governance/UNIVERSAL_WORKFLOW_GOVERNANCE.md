# UNIVERSAL WORKFLOW GOVERNANCE

## Anti-Failure Doctrine · One-Shot Systems · Human-Hostile Environments

**Version:** 1.0  
**Authority Level:** SUPREME — Equal to Constitution and PRD-FIRST LAW  
**Enforcement:** Mandatory across ALL projects, repositories, agents, and pipelines  
**Immutability:** This document may only be modified through versioned change with documented justification, evidence from production failures, and system owner approval.

---

## Table of Contents

1. [Foundational Principle](#1-foundational-principle)
2. [Determinism Laws](#2-determinism-laws)
3. [Messaging & Parser Hostility](#3-messaging--parser-hostility)
4. [State & Routing Governance](#4-state--routing-governance)
5. [Switch / IF / Router Hard Rules](#5-switch--if--router-hard-rules)
6. [Human Input Hostility](#6-human-input-hostility)
7. [One-Shot Operational Definition](#7-one-shot-operational-definition)
8. [Pre-Deploy Hard Gate Checklist](#8-pre-deploy-hard-gate-checklist)
9. [Learning Loop & Governance Evolution](#9-learning-loop--governance-evolution)
10. [Final Doctrine Statement](#10-final-doctrine-statement)

---

## 1. Foundational Principle

### Why One-Shot Systems Fail on First Execution

Systems fail on first run because their builders operated under three fatal assumptions:

1. **The assumption of clean input.** Every external input — user text, API response, webhook payload, database row — arrives dirty, malformed, partial, or actively hostile. Systems built assuming well-formed input fail the instant reality arrives.

2. **The assumption of deterministic tooling.** Workflow engines, parsers, routers, and external APIs contain undocumented behavior, version-specific quirks, implicit defaults, and silent data transformations. A system that does not account for tooling non-determinism is a system waiting to break.

3. **The assumption that "it worked in testing" means "it will work in production."** Testing environments are sanitized. Production environments are adversarial. The gap between them is where every critical failure lives.

### LAW 1.1 — Assumption Prohibition

**All assumptions are FORBIDDEN.** Every data shape, schema, default value, node behavior, routing outcome, and external response MUST be explicitly verified, guarded, or handled. If a value cannot be confirmed, it MUST be treated as absent.

### LAW 1.2 — Hostile Default Posture

**All inputs are hostile by default.** This includes:

- User-provided text (arbitrary characters, injection vectors, empty strings, extreme lengths)
- API responses (missing fields, changed schemas, rate limits, partial data)
- Webhook payloads (duplicate deliveries, out-of-order events, malformed bodies)
- Database reads (null values, stale data, type mismatches, empty result sets)
- Configuration values (missing environment variables, expired credentials, wrong types)
- Internal node outputs (unexpected array/item wrapping, empty arrays, null propagation)

No node, function, or routing decision may trust its input without explicit validation.

### LAW 1.3 — Failure Is the Default Outcome

**Systems do not succeed by default; they fail by default.** Correct execution is the exception that must be explicitly constructed, validated, and defended. Every path through the system that is not explicitly handled is a failure path.

---

## 2. Determinism Laws

### LAW 2.1 — Explicit Over Implicit

**Every behavior MUST be explicitly declared.** Implicit resolution — where a system relies on undocumented defaults, engine internals, or "how it usually works" — is FORBIDDEN.

This applies to:

- Node parameter values (never rely on default; always set explicitly)
- Data type coercion (never rely on automatic type conversion)
- Array vs. item resolution (never assume the engine will auto-wrap or auto-unwrap)
- Execution order (never assume nodes execute in visual/layout order)
- Error propagation (never assume errors will bubble up correctly without explicit handling)

### LAW 2.2 — Non-Deterministic Behavior Must Be Bounded

When non-deterministic behavior cannot be eliminated (e.g., external API response times, third-party webhooks), it MUST be:

1. **Identified** — The specific source of non-determinism is documented.
2. **Bounded** — Timeouts, retries with limits, and circuit breakers constrain the non-deterministic element.
3. **Defaulted** — A deterministic fallback path exists for every non-deterministic outcome.

Unbounded non-determinism (e.g., infinite retry loops, open-ended waits) is a SYSTEM FAILURE.

### LAW 2.3 — Mandatory Normalization at Boundaries

At every system boundary (webhook receipt, API response, database read, user input, inter-workflow communication), data MUST be normalized to a canonical internal representation BEFORE any routing, logic, or storage occurs.

Normalization includes:

- Trimming whitespace
- Lowercasing comparison values (unless case is semantically meaningful)
- Coercing types explicitly (string to number, string to boolean)
- Replacing null/undefined with explicit sentinel values or empty defaults
- Validating structure against expected schema
- Rejecting or sanitizing values that exceed expected length or complexity

### LAW 2.4 — Implicit Item Resolution Is Forbidden

Workflow engines frequently auto-resolve arrays to single items or wrap single items in arrays without explicit instruction. This creates a class of silent failures where logic branches receive unexpected data shapes.

**REQUIRED:** Every node that consumes data MUST explicitly declare whether it expects an array or a single item. If the upstream output could be either, an explicit guard MUST resolve the ambiguity before the consuming node executes.

```
IF input is array AND length > 0 → process as array
IF input is array AND length === 0 → handle empty case explicitly
IF input is single item → wrap in array OR process as item (declared explicitly)
IF input is null/undefined → handle absence explicitly
```

### LAW 2.5 — Tooling-Specific Non-Determinism Registry

The following known non-deterministic behaviors in workflow engines MUST be guarded against in every system:

| Behavior | Risk | Required Guard |
|----------|------|----------------|
| Auto-wrapping single items in arrays | Logic branches receive unexpected shape | Explicit type check before routing |
| Silent null propagation through expression chains | Downstream nodes receive null without error | Null guard at every expression that references upstream data |
| Version-dependent parameter schemas | Node imports fail or behave differently across versions | Pin node versions; validate schemas via API before build |
| Implicit string-to-number coercion in comparisons | Switch/IF evaluates incorrectly | Explicit type coercion before comparison |
| Execution order assumptions in parallel branches | Race conditions in shared state | No shared mutable state between parallel branches |
| Silent truncation of long string values | Data loss without error | Length validation at input boundaries |
| Default values that differ between UI and API creation | Workflow behaves differently when imported vs. created in UI | Always set every parameter explicitly; never rely on defaults |
| Empty string treated differently from null/undefined | Conditional logic fails silently | Normalize empty string, null, and undefined to a single sentinel before evaluation |

---

## 3. Messaging & Parser Hostility

### LAW 3.1 — Messaging Platforms Are Hostile Parsers

Every messaging platform (Telegram, WhatsApp, Slack, SMS gateways, email renderers) applies its own parsing, character filtering, encoding, truncation, and rendering rules. These rules are:

- Undocumented or inconsistently documented
- Version-dependent
- Platform-dependent (mobile vs. desktop vs. web)
- Subject to change without notice

**REQUIRED:** All message content MUST be constructed for the lowest-common-denominator parser. Features that work on one platform version or client MUST NOT be assumed to work on all.

### LAW 3.2 — Markdown in Messages Is Unsafe by Default

Markdown interpretation varies dramatically across platforms:

| Platform | Bold | Italic | Code | Links | Headers |
|----------|------|--------|------|-------|---------|
| Telegram | `*text*` or `**text**` (mode-dependent) | `_text_` | `` `code` `` | `[text](url)` (MarkdownV2) | Not supported |
| WhatsApp | `*text*` | `_text_` | `` `code` `` | Auto-detected only | Not supported |
| Slack | `*text*` | `_text_` | `` `code` `` | `<url\|text>` | Not supported |
| SMS | Not supported | Not supported | Not supported | Plain URL only | Not supported |

**REQUIRED:**
- Messages MUST use only formatting constructs confirmed for the target platform.
- If the target platform is unknown or variable, messages MUST use plain text only.
- Markdown mode (where applicable) MUST be set explicitly in the API call, never assumed from defaults.

### LAW 3.3 — Special Characters Must Be Escaped

The following characters are known to cause parsing failures across messaging platforms and MUST be escaped or avoided:

| Character | Risk | Platforms Affected |
|-----------|------|--------------------|
| `_` (underscore) | Triggers italic parsing | Telegram, WhatsApp, Slack |
| `*` (asterisk) | Triggers bold parsing | Telegram, WhatsApp, Slack |
| `` ` `` (backtick) | Triggers code block parsing | Telegram, Slack |
| `~` (tilde) | Triggers strikethrough | Telegram (MarkdownV2), Slack |
| `>` (greater than) | Triggers block quote | Telegram (MarkdownV2), Slack |
| `#` (hash) | Triggers heading or channel reference | Slack |
| `\|` (pipe) | Breaks link syntax | Slack |
| `(`, `)` | Breaks link syntax | Telegram (MarkdownV2) |
| `.` (period after number) | Triggers ordered list | Telegram (MarkdownV2) |
| `-` (hyphen at line start) | Triggers unordered list | Telegram (MarkdownV2) |
| `{`, `}` | Breaks template/expression engines | Workflow engines, Slack |
| `=` (equals) | Breaks key-value parsers | Some webhook processors |
| Emojis with variation selectors | Offset calculation errors | Telegram (entity offsets) |

**REQUIRED:** When user-provided content is embedded in messages, ALL special characters MUST be escaped for the target platform's parser before message assembly.

### LAW 3.4 — Dynamic Text Construction Rules

When constructing messages from templates and dynamic data:

1. **Variable substitution MUST occur BEFORE formatting is applied.** Inserting formatted text into a template that then applies its own formatting creates double-encoding.
2. **All dynamic values MUST be sanitized for the target parser** before insertion. A user's name containing `*` will break bold formatting in Telegram.
3. **Message length MUST be validated against platform limits** before sending. Truncation by the platform is uncontrolled and will break formatting.
4. **Line breaks MUST use the platform's expected encoding** (`\n` vs. `<br>` vs. double newline). Mixing line break styles creates rendering artifacts.

### LAW 3.5 — Entity Offset Integrity

Platforms that use character offsets for formatting (Telegram entities, Slack blocks) are extremely fragile:

- **Multi-byte characters** (emojis, CJK, diacritics) consume more bytes than their visual character count.
- **Variation selectors** and **zero-width joiners** are invisible but consume offset space.
- **UTF-16 surrogate pairs** (emojis above U+FFFF) count as 2 characters in some APIs and 1 in others.

**REQUIRED:** When using offset-based entity formatting, character counting MUST use the target API's encoding (UTF-8, UTF-16, or code points). Using the wrong encoding silently shifts all subsequent entities, producing garbled formatting.

### LAW 3.6 — Branding and Attribution Prohibition

System-generated messages MUST NOT include:

- AI model names, engine names, or "powered by" attributions (unless explicitly required by the PRD)
- Internal system identifiers, workflow names, or node names
- Debug information, stack traces, or technical error messages
- Metadata intended for logging but not for end users

Every character in a user-facing message MUST serve the user's need. System vanity is FORBIDDEN.

---

## 4. State & Routing Governance

### LAW 4.1 — State Machines Are Mandatory

Every system that processes multi-step interactions, maintains user context, or routes based on prior actions MUST implement an explicit finite state machine (FSM).

An explicit FSM requires:

1. **Enumerated states** — Every possible state is named and documented. There is no unnamed state.
2. **Defined transitions** — Every valid transition from one state to another is explicitly declared. Transitions that are not declared are FORBIDDEN.
3. **Transition triggers** — Every transition has an explicit trigger (event, input, condition). Implicit transitions are FORBIDDEN.
4. **State persistence** — The current state is stored in a durable, queryable location (database, not memory). In-memory-only state is FORBIDDEN for production systems.
5. **Default state** — A well-defined initial state exists for every new entity entering the system.

### LAW 4.2 — State-Driven Routing Is Required

All routing decisions MUST be derived from the persisted state, not from raw input content.

```
FORBIDDEN: Route based on message text content ("if message contains 'help' → help flow")
REQUIRED:  Route based on state ("if user.state === 'AWAITING_HELP_TOPIC' → help flow")
```

Raw input determines state transitions. State determines routing. These are separate operations and MUST NOT be collapsed into a single step.

### LAW 4.3 — Known State Categories

Every multi-step system MUST handle at minimum the following state dimensions:

| Dimension | Purpose | Examples |
|-----------|---------|----------|
| **Authentication state** | Is this entity identified and authorized? | `ANONYMOUS`, `IDENTIFIED`, `AUTHENTICATED`, `SUSPENDED` |
| **Onboarding state** | Has this entity completed required setup? | `NEW`, `ONBOARDING_STEP_1`, `ONBOARDING_STEP_2`, `ONBOARDED` |
| **Language/locale state** | What language does this entity operate in? | `LANG_ES`, `LANG_EN`, `LANG_UNSET` |
| **Interaction state** | Where is this entity in their current workflow? | `IDLE`, `AWAITING_INPUT`, `PROCESSING`, `COMPLETED`, `ERROR` |
| **Authorization state** | What is this entity allowed to do? | `FREE_TIER`, `PAID_TIER`, `ADMIN`, `BLOCKED` |

Not every system requires every dimension. But every system MUST explicitly declare which dimensions it tracks and MUST handle the case where a dimension is unset.

### LAW 4.4 — State Transitions Must Be Atomic

A state transition MUST be atomic: the old state is replaced by the new state in a single operation. Between the read of the current state and the write of the new state, no other operation may modify the state.

If atomicity cannot be guaranteed (e.g., distributed systems without transactions):

- Implement idempotency keys on state transitions.
- Implement optimistic locking (version check before write).
- Handle stale-state conflicts explicitly (retry or escalate).

Silent state corruption from concurrent writes is a SYSTEM FAILURE.

### LAW 4.5 — Unrecognized State Is a Hard Error

If the system reads a state value that is not in its enumerated set, this is NOT a soft error. The system MUST:

1. Log the unrecognized state with full context.
2. Route to an explicit error handler.
3. NOT attempt to "guess" the intended state.
4. NOT default to the initial state (this causes re-onboarding or data loss).
5. Notify the operator if the unrecognized state persists.

---

## 5. Switch / IF / Router Hard Rules

### LAW 5.1 — Known Failure Vectors in Routing Nodes

The following failure patterns have been observed in production workflow engines and MUST be prevented:

| Failure | Root Cause | Prevention |
|---------|-----------|------------|
| Switch evaluates wrong branch | Case sensitivity mismatch between input and condition | Normalize both sides to same case before comparison |
| Switch evaluates no branch | Input value not matching any condition, no fallback configured | MANDATORY fallback output on every switch |
| IF node passes both TRUE and FALSE | Expression evaluates to truthy non-boolean value | Coerce expression result to boolean explicitly |
| IF node passes neither branch | Expression throws error silently | Wrap expression in try-catch or use safe accessor patterns |
| Router skips items silently | Array input with some items matching no route | Validate routing coverage before router node |
| Switch schema key mismatch | `rules.values` vs `rules.rules` vs `rules.conditions` across versions | Verify schema via API/MCP before build; never assume |
| Hidden UI-set flags | Workflow engine UI sets parameters not visible in JSON export | Import workflow JSON and verify all parameters are present |

### LAW 5.2 — Every Router MUST Have a Fallback

**EVERY** Switch node, IF node, and Router node MUST have a connected fallback/default/else output.

The fallback output MUST:

1. Capture the unexpected value that triggered the fallback.
2. Log the event with: node name, expected values, actual value, execution context.
3. Route to a safe handler (error workflow, notification, or graceful degradation).
4. NOT silently drop the execution.

A router without a connected fallback is a pre-deployment BLOCK.

### LAW 5.3 — Comparison Safety Rules

| Rule | Rationale |
|------|-----------|
| Always normalize case before string comparison | `"Active"` ≠ `"active"` ≠ `"ACTIVE"` |
| Always trim whitespace before comparison | `"active "` ≠ `"active"` |
| Always coerce types explicitly before comparison | `"3"` vs `3` behaves differently in different engines |
| Never compare against raw user input directly | User input is unsanitized; compare against normalized version |
| Never use `==` when `===` is available | Loose equality introduces type coercion surprises |
| Always handle null/undefined as a distinct case | `null == undefined` is true in JavaScript but means different things |
| Never compare floating-point numbers for equality | Use epsilon-range comparison or integer representations |

### LAW 5.4 — Switch Schema Must Be Verified

The schema for Switch nodes varies across workflow engine versions. The parameter key for conditions may be `rules.values`, `rules.rules`, `rules.conditions`, or another variant depending on the node version.

**REQUIRED:** Before building any Switch node, query the workflow engine API or MCP to confirm the exact schema for the target node version. Using the wrong schema key produces a node that imports without error but evaluates no conditions at runtime — a silent failure.

### LAW 5.5 — Boolean Handling in Conditionals

Boolean values in workflow engines are a known failure vector:

- The string `"false"` is truthy in JavaScript.
- The number `0` is falsy but may represent a valid value.
- An empty string `""` is falsy but may be a valid user response ("no comment").
- `null` and `undefined` are both falsy but carry different semantics.

**REQUIRED:** Every conditional MUST compare against explicit values, never rely on truthiness/falsiness alone.

```
FORBIDDEN: if (value) → route
REQUIRED:  if (value === true) → route A
           if (value === false) → route B
           if (value === null || value === undefined) → route C (absence handler)
```

---

## 6. Human Input Hostility

### LAW 6.1 — Users Will Break Every System

This is not a warning. It is an axiom. Users will:

- Send empty messages
- Send messages with only whitespace
- Send messages with only emojis
- Send messages exceeding any reasonable length
- Send messages in unexpected languages
- Send media when text is expected
- Send text when media is expected
- Click buttons that should no longer be active
- Repeat the same input multiple times rapidly
- Navigate out-of-sequence in multi-step flows
- Provide data in wrong formats (dates, numbers, phone numbers)
- Attempt to inject code, commands, or system prompts
- Abandon flows mid-step and return hours or days later
- Interact from multiple devices or sessions simultaneously

**REQUIRED:** Every input handler MUST account for ALL of the above. "The user will follow the expected flow" is a FORBIDDEN assumption.

### LAW 6.2 — Validation Before Action, Always

**No action may be taken on user input before validation.**

Validation occurs in this order:

1. **Presence check** — Is the input non-null, non-empty, and of the expected type?
2. **Format check** — Does the input match the expected pattern (length, characters, structure)?
3. **Semantic check** — Is the input value within the expected domain (valid option, existing entity, authorized action)?
4. **State check** — Is this input expected given the user's current state?

If any check fails, the system MUST respond with a safe, user-understandable message and MUST NOT advance the state.

### LAW 6.3 — Safe Failure Responses

When user input fails validation, the system response MUST:

1. Acknowledge that the input was received (the user must know the system is alive).
2. Explain what was expected (without revealing system internals).
3. Provide a clear path to retry or correct.
4. NOT expose error codes, stack traces, node names, or internal state labels.
5. NOT leave the user in an unrecoverable state.

```
FORBIDDEN: "Error: node 'Validate_Phone' failed — TypeError: cannot read property 'match' of undefined"
REQUIRED:  "I didn't understand that phone number. Please send it in the format +34 XXX XXX XXX."
```

### LAW 6.4 — No Silent Crashes

If the system encounters an error during input processing:

- The user MUST receive a response (even if it is a generic "something went wrong").
- The error MUST be logged with full context.
- The user's state MUST NOT be corrupted.
- The user MUST be able to retry or continue from their last valid state.

A system that crashes silently — accepting input, failing internally, and producing no response — is a SYSTEM FAILURE of the highest severity. Users will assume the system is dead and will not return.

### LAW 6.5 — Idempotency Under Rapid Input

Users frequently:

- Double-tap buttons
- Resend messages when they don't see an immediate response
- Trigger webhooks multiple times through retry mechanisms

**REQUIRED:** All state-changing operations MUST be idempotent or protected by deduplication:

- Use execution locks or idempotency keys for state transitions.
- Reject duplicate inputs within a configurable time window.
- Never process the same webhook delivery ID twice.

### LAW 6.6 — Session Timeout and Stale State Handling

Users abandon flows and return. When they return:

- The system MUST check whether their in-progress state is still valid.
- If the state has expired (configurable timeout), the system MUST reset gracefully and inform the user.
- If the state is still valid, the system MUST resume from the correct step without re-requesting already-provided information.
- The system MUST NOT assume the user remembers where they left off.

---

## 7. One-Shot Operational Definition

### LAW 7.1 — Definition of One-Shot

A system qualifies as "one-shot" if and only if:

1. **It imports without errors.** The system artifact (workflow JSON, agent configuration, pipeline definition) can be loaded into the target runtime without modification, without missing dependencies, and without schema validation failures.

2. **It executes correctly on first run.** The system processes its intended inputs and produces its intended outputs on the very first execution, with zero manual intervention, zero parameter adjustment, and zero "quick fixes."

3. **It handles all anticipated failure modes.** Every failure class identified in the system's failure taxonomy is either prevented, auto-repaired, or escalated. No failure produces silent degradation.

4. **It requires zero human correction.** The system does not rely on a human operator to fix outputs, adjust routing, or restart after errors during normal operation.

5. **It is deterministic.** Given the same inputs and external conditions, the system produces the same outputs every time. Non-deterministic external factors (API latency, third-party availability) are bounded and defaulted.

### LAW 7.2 — Conditions That MUST Be Met

Before a system may be declared one-shot ready:

| Condition | Verification Method |
|-----------|-------------------|
| All node schemas verified against runtime API | Schema ground-truth query |
| All parameters explicitly set (no reliance on defaults) | Lint rule: no empty required parameters |
| All routing nodes have connected fallback outputs | Lint rule: graph traversal check |
| All inputs validated before processing | Code review: validation-before-action pattern |
| Error workflow configured and connected | Lint rule: error workflow ID present and valid |
| All credentials verified as existing and valid | Runtime credential check (read-only) |
| All expressions syntactically valid | Expression parser validation |
| No placeholder values in any field | Lint rule: search for TODO, TBD, PLACEHOLDER, xxx, changeme |
| Idempotency implemented for all state-changing operations | Design review: dedup mechanism documented |
| Fallback paths tested for all failure classes | Test matrix: one test per failure class |

### LAW 7.3 — What Disqualifies a System from Deployment

A system is NOT one-shot ready and MUST NOT be deployed if ANY of the following are true:

| Disqualifying Condition | Severity |
|------------------------|----------|
| Any node references a schema key not confirmed by runtime API | CRITICAL |
| Any routing node lacks a connected fallback output | CRITICAL |
| Any expression references a node that does not exist in the workflow | CRITICAL |
| Any credential reference points to a non-existent credential | CRITICAL |
| Any parameter contains a placeholder value | CRITICAL |
| Any input path lacks validation before action | HIGH |
| No error workflow is configured | HIGH |
| Any state transition is not atomic or idempotent | HIGH |
| Any external API call lacks timeout and retry bounds | HIGH |
| Any user-facing message contains system internals | MEDIUM |
| Any dynamic text construction does not escape special characters | MEDIUM |

**A single CRITICAL disqualification blocks deployment. No exceptions.**

---

## 8. Pre-Deploy Hard Gate Checklist

This checklist is BINARY. Every item is PASS or FAIL. A single FAIL blocks deployment.

### 8.1 — Schema & Structure

| # | Check | Pass/Fail |
|---|-------|-----------|
| 1 | Every node type verified against runtime API schema | |
| 2 | Every node version pinned and compatible with target runtime | |
| 3 | Every parameter explicitly set (no implicit defaults) | |
| 4 | Workflow JSON is complete and valid (no partial exports) | |
| 5 | All connections reference valid node names and output indices | |

### 8.2 — Routing & Logic

| # | Check | Pass/Fail |
|---|-------|-----------|
| 6 | Every Switch/IF/Router has a connected fallback output | |
| 7 | Every comparison normalizes case and type before evaluation | |
| 8 | Every conditional handles null/undefined as a distinct case | |
| 9 | No routing decision is based on raw, unsanitized user input | |
| 10 | No dead-end nodes exist (unless explicitly documented as terminal) | |

### 8.3 — Input & Validation

| # | Check | Pass/Fail |
|---|-------|-----------|
| 11 | Every external input boundary has a validation step | |
| 12 | Every validation failure produces a user-facing response | |
| 13 | No validation failure leaves the system in a silent error state | |
| 14 | All array/item expectations are explicitly handled | |
| 15 | All string inputs are trimmed and sanitized before use | |

### 8.4 — Error Handling & Resilience

| # | Check | Pass/Fail |
|---|-------|-----------|
| 16 | Error workflow is configured with valid workflow ID | |
| 17 | Error workflow captures: workflow ID, execution ID, failed node, error message, payload shape | |
| 18 | Error workflow classifies failure as FIXABLE_AUTOMATICALLY or HUMAN_ACTION_REQUIRED | |
| 19 | Every external API call has timeout and bounded retry | |
| 20 | No unbounded loops or recursive calls exist | |

### 8.5 — State & Idempotency

| # | Check | Pass/Fail |
|---|-------|-----------|
| 21 | All states are enumerated and documented | |
| 22 | All state transitions are atomic | |
| 23 | All state-changing operations are idempotent or deduplicated | |
| 24 | Stale state / session timeout is handled explicitly | |
| 25 | Unrecognized state values route to an error handler, not a default state | |

### 8.6 — Messaging & Output

| # | Check | Pass/Fail |
|---|-------|-----------|
| 26 | All user-facing messages are free of system internals | |
| 27 | All dynamic text escapes special characters for the target platform | |
| 28 | All messages respect target platform length limits | |
| 29 | No branding, attribution, or AI model names in user-facing messages (unless PRD-required) | |
| 30 | Markdown formatting uses only constructs confirmed for the target platform | |

### 8.7 — Credentials & Deployment

| # | Check | Pass/Fail |
|---|-------|-----------|
| 31 | All credential references point to existing, valid credentials | |
| 32 | No placeholder credentials exist in the workflow | |
| 33 | No hardcoded secrets, API keys, or tokens in workflow JSON | |
| 34 | Workflow can be imported to target environment without modification | |
| 35 | No TODO, TBD, PLACEHOLDER, FIXME, or CHANGEME markers remain | |

### Gate Decision

```
IF all 35 checks = PASS → DEPLOY PERMITTED
IF any check = FAIL   → DEPLOY BLOCKED
                         Report: check number, failure description, required remediation
```

**No partial compliance. No "we'll fix it after launch." No exceptions.**

---

## 9. Learning Loop & Governance Evolution

### LAW 9.1 — Every New Error Becomes a Law

When a production failure occurs that is NOT already covered by this governance document:

1. **Document the failure** — Root cause, affected system, failure class, impact.
2. **Extract the general pattern** — Abstract from the specific failure to the class of failures it represents.
3. **Write the law** — Formulate a governance rule that would have prevented this failure.
4. **Add to this document** — The new law is added in the appropriate section with a reference to the originating failure.
5. **Validate retroactively** — All existing systems are checked against the new law. Systems that violate it are flagged for remediation.

This loop is NOT optional. It is the mechanism by which this document evolves from a static ruleset into a living immune system.

### LAW 9.2 — Failure Feedback Format

Every failure submitted to this governance loop MUST follow this format:

```
FAILURE REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
failure_id:          [unique identifier]
date:                [ISO 8601]
system:              [system name + version]
node/component:      [specific failing element]
failure_class:       FIXABLE_AUTOMATICALLY | HUMAN_ACTION_REQUIRED
root_cause:          [specific technical cause]
general_pattern:     [abstract class of failure this represents]
existing_law_gap:    [which section of this document should have prevented it, or "NEW"]
proposed_law:        [exact text of the governance rule to add]
prevention_method:   [what check, guard, or lint rule would catch this pre-deploy]
retroactive_impact:  [list of existing systems that may be affected]
```

### LAW 9.3 — Cross-Project Propagation

This governance document is shared across ALL projects. When a failure is discovered in Project A and a new law is added:

- **All other projects** (B, C, D, ...) MUST be audited against the new law at the earliest opportunity.
- Projects that predate the new law are NOT exempt.
- The audit may be deferred but MUST NOT be skipped.

This prevents the pattern where one project learns from failure but all other projects remain vulnerable to the same failure class.

### LAW 9.4 — Governance Version Control

This document MUST be version-controlled. Every change MUST include:

- Version number increment
- Date of change
- Failure report ID that triggered the change (if applicable)
- Exact diff of what was added, modified, or removed
- System owner acknowledgment

No in-conversation amendment, verbal override, or implied exception is valid for modifying this document.

### LAW 9.5 — Governance Audit Cadence

At minimum every 30 days (or after every 5 production failures, whichever comes first):

1. Review all failure reports submitted since last audit.
2. Confirm all proposed laws have been added.
3. Confirm all retroactive audits have been completed or scheduled.
4. Verify that the pre-deploy checklist (Section 8) reflects all current laws.
5. Archive resolved failure reports.

Skipping a governance audit degrades the immune system. The next failure that could have been prevented is the cost.

---

## 10. Final Doctrine Statement

> **A system that has not been designed to survive hostility has been designed to fail.**

This document is not documentation. It is not guidance. It is not a collection of best practices.

This document is a **system immune layer** — a set of hard laws derived from observed production failures, encoded to prevent their recurrence across every system, project, and pipeline where it is deployed.

Every law exists because a system failed. Every guard exists because an assumption was wrong. Every check exists because "it should work" did not work.

Systems governed by this doctrine do not hope for correctness. They enforce it. They do not assume clean input. They sanitize it. They do not trust tooling. They verify it. They do not rely on humans. They protect against them.

**If a system cannot survive its first encounter with reality, it was never ready for production.**

---

*End of UNIVERSAL WORKFLOW GOVERNANCE v1.0*
