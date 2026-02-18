# AGENT REGISTRY — TM-AdsOS

**Version:** 1.0
**Date:** 2026-02-11
**PRD Version:** PRD.md v1.0
**Pipeline Composition Version:** PIPELINE_COMPOSITION.md v1.0
**Authoritative Dataset:** DS-001 ($100k/day Creative System v1.0)

---

## 1. Registry Purpose

This registry enumerates every agent authorized to exist within TM-AdsOS v1.0. An agent not listed here is ILLEGAL (per Repo-SHA Governance Law Section 5). No agent may self-authorize expanded scope.

---

## 2. Registered Agents

### AGENT-001: Gatekeeper Financiero Brutal

| Field | Value |
|-------|-------|
| Agent ID | AGENT-001 |
| Name | Gatekeeper Financiero Brutal |
| PRD Feature | F1 (Financial Gatekeeper) |
| Dataset Authority | DS-001 Section 2, 12; UNIT_ECONOMICS_LAW.md v1.0 |
| Input Contract | product_intake record (AOV, COGS%, country, niche, monthly_budget) |
| Output Contract | gatekeeper_results record (viable, target_roas, cpa_ceiling, margin, blocking_reason) |
| Execution Mode | Synchronous, single-invocation |
| State Transition | INTAKE_COMPLETE → GATEKEEPER_PASS or GATEKEEPER_FAIL |
| MUST NOT | Produce creative briefs. Recommend products. Override margin thresholds. Suggest workarounds for blocked products. |
| Dependencies | Supabase (read intake, write results), UNIT_ECONOMICS_LAW.md (formulas) |
| Integration Risk | False positives (blocking viable products) or false negatives (passing non-viable products). Mitigated by conservative thresholds with 30% safety buffer. |

### AGENT-002: Creative Hit-Rate Factory

| Field | Value |
|-------|-------|
| Agent ID | AGENT-002 |
| Name | Creative Hit-Rate Factory |
| PRD Feature | F2 (Creative Brief Generator) |
| Dataset Authority | DS-001 Sections 6, 7, Principles 2, 6; FOREPLAY_CREATIVE_INTELLIGENCE_LAYER.md v1.0 |
| Input Contract | product_intake + gatekeeper_results (all fields) |
| Output Contract | 3× creative_briefs records (angle, concept, awareness_stage, avatar, hypothesis, hook, triggers, format, citation) |
| Execution Mode | Synchronous, single-invocation via GPT-4o |
| State Transition | GATEKEEPER_PASS → CREATIVE_DONE |
| MUST NOT | Generate briefs without hypotheses. Skip research citations. Produce more than 3 briefs per session. Generate actual ad copy or scripts (V1 produces briefs, not final creative). Use formats not in the approved list. |
| Dependencies | OpenAI GPT-4o (generation), Supabase (read context, write briefs), DS-001 doctrine (hypothesis format, buying triggers) |
| Integration Risk | Hypothesis quality depends on GPT-4o's niche knowledge. V1 limitation: no live creative research data. Mitigated by explicit V1 disclaimer in output. |

### AGENT-003: Testing Discipline Engine

| Field | Value |
|-------|-------|
| Agent ID | AGENT-003 |
| Name | Testing Discipline Engine |
| PRD Feature | F3 (Testing Protocol Engine) |
| Dataset Authority | DS-001 Sections 7, 8, 9, Principles 4, 5 |
| Input Contract | creative_briefs (3 briefs) + gatekeeper_results (budget constraints) |
| Output Contract | testing_plans record (duration, concepts_count, iteration_ratio, winner_criteria, loser_criteria, learning_template) |
| Execution Mode | Synchronous, single-invocation via GPT-4o |
| State Transition | CREATIVE_DONE → TESTING_DONE |
| MUST NOT | Set test duration below 7 days. Skip learning extraction template. Allow concepts without hypotheses. Override the 80/20 iteration ratio without explicit justification. |
| Dependencies | OpenAI GPT-4o (plan generation), Supabase (read briefs, write plan), DS-001 doctrine (testing rules) |
| Integration Risk | Testing plan may not account for platform-specific constraints (Meta vs TikTok). V1 outputs platform-agnostic plans. |

### AGENT-004: Unit Economics Guard

| Field | Value |
|-------|-------|
| Agent ID | AGENT-004 |
| Name | Unit Economics Guard |
| PRD Feature | F4 (Budget & Threshold Calculator) — validation component |
| Dataset Authority | DS-001 Section 12; UNIT_ECONOMICS_LAW.md v1.0 |
| Input Contract | testing_plans + gatekeeper_results + product_intake.monthly_ad_budget |
| Output Contract | Validated budget parameters or adjusted recommendations |
| Execution Mode | Synchronous, single-invocation (Code node or GPT-4o) |
| State Transition | TESTING_DONE → TESTING_DONE (re-validated, no separate state) |
| MUST NOT | Approve budgets exceeding 50% of monthly budget per test cycle. Override gatekeeper viability verdict. Suggest increasing budget beyond user's stated capacity. |
| Dependencies | Supabase (read all prior results), UNIT_ECONOMICS_LAW.md (budget ceiling formulas) |
| Integration Risk | Budget adjustments may reduce testing power. System warns but does not block on budget constraints (only on viability). |

### AGENT-005: Velocity & Execution Engine

| Field | Value |
|-------|-------|
| Agent ID | AGENT-005 |
| Name | Velocity & Execution Engine |
| PRD Feature | F4 (Budget & Threshold Calculator) — threshold generation component |
| Dataset Authority | DS-001 Sections 10, 12, 13; VELOCITY_LAW.md v1.0 |
| Input Contract | All prior agent outputs + product_intake |
| Output Contract | budget_plans record (daily/weekly budgets, kill/scale thresholds, dependency alerts) |
| Execution Mode | Synchronous, single-invocation via GPT-4o |
| State Transition | TESTING_DONE → BUDGET_DONE |
| MUST NOT | Set scale threshold below 10% hit rate. Omit dependency concentration alerts. Authorize scaling without meeting all 8 readiness conditions. Produce budgets exceeding validated economics. |
| Dependencies | OpenAI GPT-4o (threshold computation), Supabase (read all context, write plan), VELOCITY_LAW.md (all formulas and thresholds) |
| Integration Risk | Thresholds are computed from estimated (not actual) data in V1. System clearly labels all thresholds as "starting points pending real performance data." |

### AGENT-006: Telegram Orchestrator

| Field | Value |
|-------|-------|
| Agent ID | AGENT-006 |
| Name | Telegram Orchestrator |
| PRD Feature | All features (interface layer) |
| Dataset Authority | ARCHITECTURE_SPEC.md (state machine), UNIVERSAL_WORKFLOW_GOVERNANCE.md (input hostility), UNIVERSAL_HARDENING_DOCTRINE.md (messaging rules) |
| Input Contract | Telegram webhook payloads (messages, callback queries) |
| Output Contract | Telegram messages (HTML parse mode, escaped content) |
| Execution Mode | Event-driven, continuous |
| State Transition | Manages ALL state transitions defined in ARCHITECTURE_SPEC.md Section 2.1 |
| MUST NOT | Process messages without state validation. Route based on message content instead of state. Send messages with Markdown parse mode. Include system internals in user messages. Send unescaped dynamic content. Process non-whitelisted users beyond denial message. |
| Dependencies | Telegram Bot API, Supabase (state persistence), Agent Pipeline workflow (sub-workflow call) |
| Integration Risk | Telegram API rate limits, webhook delivery failures, concurrent message processing. Mitigated by idempotency keys and state-based routing. |

---

## 3. Agent Interaction Matrix

| Source Agent | Target Agent | Data Passed | Mandatory |
|-------------|-------------|-------------|-----------|
| Orchestrator | AGENT-001 | product_intake | YES |
| AGENT-001 | AGENT-002 | gatekeeper_results + product_intake | YES (only if viable) |
| AGENT-002 | AGENT-003 | creative_briefs + gatekeeper_results | YES |
| AGENT-003 | AGENT-004 | testing_plans + gatekeeper_results + product_intake | YES |
| AGENT-004 | AGENT-005 | validated_budget_params + all prior | YES |
| AGENT-005 | Assembler | budget_plans + all prior | YES |
| Assembler | Orchestrator | strategy_outputs | YES |

No agent may communicate with another agent outside this matrix. No lateral communication. No skip connections.

---

## 4. Legality Declaration

All agents listed in this registry meet the Agent Legality Conditions defined in Repo-SHA Governance Law Section 5.1:

1. Governing repository declared: YES
2. Pinned commit SHA declared: (to be set at freeze)
3. All governing documents enumerated: YES (per each agent's Dataset Authority field)
4. All dataset dependencies enumerated: YES (DS-001)
5. All governing documents exist at pinned SHA: (to be verified at freeze)
6. Agent behavior fully derivable from governing documents: YES
7. Agent constraints explicitly declared: YES (MUST NOT fields)

---

*End of AGENT_REGISTRY.md v1.0*
