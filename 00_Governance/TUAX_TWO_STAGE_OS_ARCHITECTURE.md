# TUAX TWO-STAGE OPERATING SYSTEM — FULL ARCHITECTURAL SPECIFICATION

**Version:** 1.0  
**Date:** 2026-02-11  
**Authority:** CONSTITUTION.md v1.0 · PIPELINE_COMPOSITION.md v1.0  
**Pinned SHA:** `116b494583422d2125b92b01ea6fda5a9977db17`  
**Mode:** ARCHITECTURE / SPEC — No execution artifacts  
**Status:** ACTIVE  

---

## TABLE OF CONTENTS

1. [SECTION 1 — MASTER PRD](#section-1--master-prd)
2. [SECTION 2 — MICRO OS ARCHITECTURE (35€/day)](#section-2--micro-os-architecture-35day)
3. [SECTION 3 — MACRO OS ARCHITECTURE (UNLOCKED PHASE)](#section-3--macro-os-architecture-unlocked-phase)
4. [SECTION 4 — DATASET VALUE EXTRACTION PROTOCOL (DVEP)](#section-4--dataset-value-extraction-protocol-dvep)
5. [SECTION 5 — GOVERNANCE STRUCTURE](#section-5--governance-structure)
6. [SECTION 6 — CLAUDE CODE PROMPT](#section-6--claude-code-prompt)
7. [SECTION 7 — CURSOR PS1 TEMPLATE UPGRADE](#section-7--cursor-ps1-template-upgrade)

---

# SECTION 1 — MASTER PRD

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 1. WHAT

- **Product Name:** TUAX Revenue Discipline OS
- **Domain:** Direct-to-consumer e-commerce paid advertising (Shopify-based)
- **One sentence:** A two-stage operating system that enforces economic discipline on ad-funded e-commerce from €35/day micro-validation through structural scaling beyond $6M/day, governed by SHA-pinned dataset doctrines and failure-intolerant automation.

## 2. WHO

- **Target user:** Solo e-commerce operator or lean team (1–3 people) running Shopify stores with paid advertising as the primary acquisition channel.
- **Their main problem:** They waste ad budget on untested creatives, scale before signal is validated, ignore unit economics, and have no systematic learning loop — resulting in unprofitable spend, romantic optimism about "the next winner," and capital destruction.

## 3. WHY

- **Why will they pay for this:** Because without discipline enforcement, 90%+ of ad spend is wasted on creatives that never had a hypothesis, products that never had margin validation, and scaling decisions made on hope. This system replaces hope with math.
- **What are they doing today instead:** Manual spreadsheet tracking (if anything), gut-feeling creative decisions, scaling based on 24-hour ROAS spikes, no systematic learning extraction, no kill discipline, no margin floor enforcement.

## 4. FEATURES (MAX 5)

1. **Gatekeeper Financiero Brutal** — Pre-launch economic validator that blocks any product/creative from receiving ad spend unless margin floor, CPA ceiling, and refund ceiling are mathematically viable.
2. **Creative Hit-Rate Factory** — Hypothesis-driven creative production engine enforcing the DS-001 ($100k/day Creative System) doctrine: intent before production, 80/20 iteration law, winner understanding mandate.
3. **Testing Discipline Engine** — Structured testing protocol with hard kill rules (≤72h for zero-signal losers), minimum test duration (7 days for valid classification), and mandatory learning extraction per test.
4. **Unit Economics Guard** — Real-time margin monitoring that blocks scaling when net margin < 25%, refund rate > 12%, or CPA > break-even for 48+ hours.
5. **Failure Memory & Velocity Engine** — Append-only learning log that encodes anti-patterns, tracks learning velocity (learnings/concept/week), and blocks repeated hypothesis patterns that have already failed.

## 5. DATA

### Table: `products`
| Field | Type | Purpose |
|-------|------|---------|
| product_id | UUID | Primary key |
| shopify_product_id | string | Shopify reference |
| product_name | string | Human-readable name |
| cogs | decimal | Cost of goods sold per unit |
| selling_price | decimal | Current selling price |
| shipping_cost | decimal | Average shipping cost per unit |
| margin_gross | decimal | Calculated: (selling_price - cogs - shipping_cost) / selling_price |
| break_even_cpa | decimal | Calculated: selling_price - cogs - shipping_cost |
| refund_rate_trailing_30d | decimal | Rolling 30-day refund rate |
| status | enum | CANDIDATE, TESTING, ACTIVE, PAUSED, KILLED |
| created_at | timestamp | Record creation |
| updated_at | timestamp | Last modification |

### Table: `creatives`
| Field | Type | Purpose |
|-------|------|---------|
| creative_id | UUID | Primary key |
| product_id | UUID | FK → products |
| hypothesis | text | IF-THEN-BECAUSE statement (mandatory, non-empty) |
| research_citation | text | Customer research or creative research finding cited |
| marketing_angle | string | Strategic angle being tested |
| format | enum | UGC, VSL, DEMO, COMPARISON, TESTIMONIAL, CAROUSEL, STATIC |
| awareness_stage | enum | COLD, WARM, HOT |
| avatar | string | Target customer segment |
| status | enum | DRAFT, HYPOTHESIS_APPROVED, LAUNCHED, WINNER, LOSER, KILLED, ITERATED |
| parent_creative_id | UUID | FK → creatives (null if net-new, populated if iteration) |
| is_iteration | boolean | True if derived from a winner |
| launch_date | date | Date launched for testing |
| kill_date | date | Date killed or classified |
| created_at | timestamp | |
| updated_at | timestamp | |

### Table: `test_results`
| Field | Type | Purpose |
|-------|------|---------|
| result_id | UUID | Primary key |
| creative_id | UUID | FK → creatives |
| test_start_date | date | When test began |
| test_end_date | date | When test concluded (min 7 days) |
| spend_total | decimal | Total ad spend during test |
| revenue_total | decimal | Total attributed revenue |
| roas | decimal | Calculated: revenue / spend |
| hook_rate | decimal | % past first 3 seconds |
| hold_rate | decimal | % through midpoint |
| ctr | decimal | Click-through rate |
| cpa_actual | decimal | Actual cost per acquisition |
| classification | enum | WINNER, LOSER, INCONCLUSIVE |
| learning_extracted | boolean | Has a learning been documented |
| learning_text | text | The documented learning |
| anti_pattern | text | Specific element to avoid (if loser) |
| created_at | timestamp | |

### Table: `economic_snapshots`
| Field | Type | Purpose |
|-------|------|---------|
| snapshot_id | UUID | Primary key |
| product_id | UUID | FK → products |
| snapshot_date | date | Date of snapshot |
| daily_spend | decimal | Ad spend that day |
| daily_revenue | decimal | Revenue that day |
| daily_roas | decimal | ROAS that day |
| daily_cpa | decimal | CPA that day |
| net_margin | decimal | Net margin after all costs |
| refund_count | integer | Refunds processed that day |
| refund_rate | decimal | Refund rate that day |
| cumulative_profit_loss | decimal | Running P/L since product activation |
| kill_switch_triggered | boolean | Whether kill conditions were met |

### Table: `learning_log`
| Field | Type | Purpose |
|-------|------|---------|
| learning_id | UUID | Primary key |
| creative_id | UUID | FK → creatives |
| product_id | UUID | FK → products |
| learning_type | enum | OBSERVATION, CONFIRMED_RULE, ANTI_PATTERN |
| dimension | enum | HOOK, ANGLE, FORMAT, AUDIENCE, TRIGGER, PROOF, CREDIBILITY |
| finding | text | What was observed |
| confirmation_count | integer | Number of tests confirming this finding |
| promoted_to_rule | boolean | Whether this has been promoted to a binding rule |
| promoted_date | date | When promoted |
| created_at | timestamp | |

### Table: `os_state`
| Field | Type | Purpose |
|-------|------|---------|
| state_id | UUID | Primary key |
| current_stage | enum | MICRO, MACRO |
| unlock_date | date | When MACRO was unlocked (null if still MICRO) |
| hit_rate_trailing_4w | decimal | Trailing 4-week hit rate |
| active_products | integer | Count of ACTIVE products |
| total_daily_budget | decimal | Current total daily ad budget |
| kill_switch_global | boolean | Global kill switch state |
| last_audit_date | date | Last governance audit |

## 6. PAGES

This is a backend/automation system. No user-facing pages in V1. All interfaces are:

| Route | Name | Protected |
|-------|------|-----------|
| N/A — Telegram Bot | Operator Dashboard (conversational) | YES — whitelist |
| N/A — Google Sheets | Economic Tracker | YES — shared access |
| N/A — n8n Dashboard | Workflow Monitor | YES — admin only |

## 7. USER FLOWS

### Flow 1: Product Candidacy Validation
1. Operator submits product candidate (Shopify URL or product details)
2. System extracts: COGS, selling price, shipping cost
3. System calculates: gross margin, break-even CPA
4. IF gross margin < 25% → BLOCK with exact margin shortfall
5. IF break-even CPA < €5 → WARN (thin margin, high risk)
6. IF valid → Product status → CANDIDATE
7. Operator confirms → Product status → TESTING

### Flow 2: Creative Launch
1. Operator submits creative brief
2. System validates: hypothesis present (IF-THEN-BECAUSE), research citation present, angle declared, format declared, avatar declared
3. IF any field missing → BLOCK with exact missing field
4. IF iteration → System validates parent_creative_id exists and parent is WINNER
5. System checks iteration ratio: current iterations / total concepts ≥ 80%
6. IF ratio violated → WARN (too many net-new concepts)
7. Creative status → LAUNCHED
8. Test timer begins (minimum 7 days)

### Flow 3: Test Classification
1. After 7 days minimum, system pulls performance data
2. System calculates: ROAS, hook rate, CTR, hold rate, CPA, spend level
3. System classifies: WINNER (all winner criteria met) or LOSER (any loser criteria met) or INCONCLUSIVE (extend to 14 days)
4. System REQUIRES learning extraction: operator must document finding
5. IF WINNER → mandatory scene-by-scene breakdown trigger
6. IF LOSER → mandatory anti-pattern recording
7. IF learning not provided within 48 hours → BLOCK next creative launch

### Flow 4: Kill Discipline
1. System monitors daily: spend, ROAS, CPA, refund rate
2. IF creative has zero spend after 72 hours of budget availability → AUTO-KILL
3. IF CPA > break-even for 48 consecutive hours → AUTO-KILL
4. IF refund rate > 12% on product trailing 7 days → PAUSE product, notify operator
5. IF net margin < 25% for 72 consecutive hours → BLOCK scaling, notify operator
6. IF hit rate < 10% trailing 4 weeks → BLOCK all new concept production, enforce research sprint

### Flow 5: Scale Authorization
1. System checks all 8 scaling readiness conditions (DS-001 Section 12)
2. IF all conditions PASS → Scale authorized, budget increase permitted
3. IF any condition FAIL → BLOCK with specific failed condition
4. Scaling is additive: budget increases in 20% increments, never doubles
5. Each increment requires 72 hours of stable metrics before next increment

## 8. INTEGRATIONS

- [x] Shopify (product data, order data, refund data)
- [x] Meta Ads API (creative performance data, spend data)
- [x] Google Sheets (economic tracker, learning log, testing tracker)
- [x] Telegram Bot (operator notifications, commands, dashboard)
- [x] n8n (workflow orchestration engine)
- [x] Supabase (persistent state storage, product/creative/test data)
- [ ] Clerk — NOT USED
- [ ] Stripe — NOT USED (Shopify handles payments)
- [ ] Resend — NOT USED

## 9. DESIGN

- **Style:** Functional, zero-decoration, operator-grade
- **Primary color:** N/A — this is a backend system; Telegram messages use plain text with HTML formatting only
- **Reference sites:** N/A — no frontend

## 10. PRICING

- **Model:** Internal tool — no external pricing in V1
- **Tiers:** N/A

| Tier | Price | Includes |
|------|-------|----------|
| Internal Use | €0 | Full system for operator's own stores |

## 11. NOT BUILDING (V1) — HARD EXCLUSIONS

- No multi-channel scaling (Meta only in V1)
- No advanced personalization or dynamic creative optimization
- No automated creative generation (AI-generated ad copy/video)
- No complex funnel routing or multi-step retargeting
- No cross-brand or multi-brand portfolio management
- No automated bid management or audience optimization
- No landing page optimization or CRO
- No customer lifetime value prediction models
- No TikTok, Google Ads, or other platform integrations
- No mobile app
- No user-facing web dashboard
- No team management or multi-user permissions
- No Macro OS modules — they are SPEC only until unlock criteria met
- No CPM (Causal Performance Memory) — Macro OS only
- No cross-dataset blending — Macro OS only

## 12. SUCCESS

| Metric | Target | Measurement |
|--------|--------|-------------|
| Hit rate (trailing 4 weeks) | ≥ 15% | Winners / Total launched concepts |
| Zero-hypothesis launches | 0 | Count of concepts launched without IF-THEN-BECAUSE |
| Learning extraction rate | 100% | % of completed tests with documented learning |
| Kill discipline compliance | 100% | % of kill triggers acted on within SLA |
| Net margin floor compliance | 100% | % of days where scaling was blocked when margin < 25% |
| Refund rate compliance | 100% | % of products paused when refund > 12% |
| Budget waste rate | < 15% | Spend on concepts killed before 7 days / total spend |
| Time to kill (zero-signal losers) | ≤ 72 hours | Average time from launch to kill for zero-spend creatives |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## PRD VALIDATION CROSS-REFERENCE

| Feature | User Flow(s) | Page(s) | Data Entity(ies) |
|---------|-------------|---------|------------------|
| Gatekeeper Financiero Brutal | Flow 1, Flow 4 | Telegram Bot, Google Sheets | products, economic_snapshots, os_state |
| Creative Hit-Rate Factory | Flow 2 | Telegram Bot | creatives, learning_log |
| Testing Discipline Engine | Flow 3, Flow 4 | Google Sheets | test_results, creatives |
| Unit Economics Guard | Flow 4, Flow 5 | Google Sheets, Telegram Bot | economic_snapshots, products, os_state |
| Failure Memory & Velocity Engine | Flow 3 | Google Sheets | learning_log, test_results |

**Validation result:** Every feature maps to at least one user flow, one interface, and one data entity. No orphan features. No scope leakage beyond NOT BUILDING. SUCCESS metrics are numeric and binary-evaluable.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## CAPITAL GATING LOGIC

### Micro OS Budget Constraints

| Parameter | Value | Unit |
|-----------|-------|------|
| Total daily ad budget (initial) | 35 | EUR |
| Maximum creatives under test simultaneously | 3 | count |
| Budget per creative (split) | 10–12 | EUR |
| Minimum test duration | 7 | days |
| Maximum test duration (before forced classification) | 14 | days |
| Maximum daily budget before Macro unlock | 200 | EUR |

### Unlock Criteria: Micro → Macro

ALL of the following must be TRUE simultaneously for a minimum of 14 consecutive days:

| # | Condition | Threshold | Measurement |
|---|-----------|-----------|-------------|
| 1 | Hit rate | ≥ 15% | Trailing 4-week average |
| 2 | Refund rate | ≤ 10% | Trailing 30-day average across all active products |
| 3 | Net margin | ≥ 30% | Trailing 14-day average after all costs (COGS, shipping, ad spend, refunds, platform fees) |
| 4 | Creative iteration cycle | < 72 hours | Average time from winner classification to first iteration launch |
| 5 | CPA | Below break-even | 3 consecutive days minimum |
| 6 | Cashflow | Positive | Cumulative P/L positive for trailing 14 days |
| 7 | Learning loop active | YES | Creative meetings documented every week for past 4 weeks |
| 8 | Winner understanding | 100% | Every active winner has documented "why it works" |
| 9 | Dependency concentration | Top 1 ad ≤ 30% of spend | Current week |
| 10 | Iteration coverage | Every winner has ≥ 3 active iterations | Current state |

If ANY condition drops below threshold during the 14-day window, the window resets.

### Kill Thresholds

| Trigger | Threshold | SLA |
|---------|-----------|-----|
| Zero-spend creative | 0 impressions after 72 hours with budget | Auto-kill within 4 hours |
| CPA above break-even | CPA > BE CPA for 48 continuous hours | Auto-pause, operator notification |
| Negative daily P/L | 3 consecutive days | Auto-pause scaling, operator alert |
| Refund spike | > 12% trailing 7 days on any product | Product auto-pause, operator escalation |
| Hit rate collapse | < 5% trailing 4 weeks | BLOCK all new concept production |
| Global margin collapse | Net margin < 15% trailing 7 days | GLOBAL KILL — all ad spend paused |

### Break-Even CPA Logic

```
break_even_cpa = selling_price - cogs - shipping_cost - (selling_price × platform_fee_rate)
```

Where `platform_fee_rate` includes Shopify transaction fees + payment processor fees (typically 2.9% + €0.30 per transaction for Shopify Payments).

For a product selling at €49.99 with €12 COGS, €5 shipping, and 3.5% platform fees:

```
break_even_cpa = 49.99 - 12.00 - 5.00 - (49.99 × 0.035) = 49.99 - 12.00 - 5.00 - 1.75 = €31.24
```

CPA MUST remain below this value. The 25% margin floor means the TARGET CPA is:

```
target_cpa = break_even_cpa × 0.75 = €31.24 × 0.75 = €23.43
```

### LTV vs CAC Constraint

In Micro OS, LTV = first-purchase AOV (no repeat purchase modeling allowed in V1 — NOT BUILDING).

```
CAC = total_ad_spend / total_conversions
LTV:CAC ratio must be ≥ 1.33 (equivalent to 25% margin floor)
```

If LTV:CAC drops below 1.33 for 72 consecutive hours → BLOCK scaling.

### Learning Velocity Requirement

```
learning_velocity = documented_actionable_learnings / concepts_launched (per week)
```

Minimum acceptable: ≥ 1.0 (at least one actionable learning per concept launched).

If learning_velocity < 1.0 for 2 consecutive weeks → BLOCK new concept production until learning backlog is cleared.

---

# SECTION 2 — MICRO OS ARCHITECTURE (35€/day)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## Micro OS Objective

Operate within a €35/day total ad budget to detect winning products and creatives through hypothesis-driven testing, disciplined kill logic, and mandatory learning extraction. The Micro OS is a signal-detection machine — its sole purpose is to find validated winners and prove the engine works before any capital is added.

## Micro OS Duration

Maximum 90 days. If unlock criteria are not met within 90 days, a full system review is mandatory: all products, all creatives, all learnings must be audited. The operator must decide: pivot products, pivot angles, or exit.

## Active Modules (ONLY these 6)

### MODULE 1: Gatekeeper Financiero Brutal

**Purpose:** Prevent any product from entering the testing pipeline unless it is mathematically capable of being profitable.

**Input:** Product candidate data (COGS, selling price, shipping cost, platform fees).

**Logic:**
1. Calculate gross margin: `(selling_price - cogs - shipping - platform_fees) / selling_price`
2. IF gross_margin < 0.25 → BLOCK: "Product rejected. Gross margin is {X}%. Minimum required: 25%. Increase price by €{Y} or reduce COGS by €{Z} to qualify."
3. Calculate break_even_cpa: `selling_price - cogs - shipping - platform_fees`
4. IF break_even_cpa < 5.00 → WARN: "Break-even CPA is only €{X}. Extremely thin margin. Any CPA volatility will cause losses. Proceed only with explicit acknowledgment."
5. Calculate target_cpa: `break_even_cpa × 0.75`
6. IF target_cpa < (daily_budget_per_creative × 0.5) → WARN: "Target CPA (€{X}) is less than half the daily creative budget (€{Y}). Statistical significance will be slow. Consider higher-AOV products."
7. IF all checks pass → Product status → CANDIDATE, operator notification sent.

**Output:** Product status updated in `products` table. Operator notification via Telegram.

**Kill logic within this module:**
- Product with 0 sales after 14 days of ad spend → Auto-kill, status → KILLED
- Product with refund rate > 12% at any point → Auto-pause, operator escalation

**What this module does NOT do:**
- Does not evaluate creative quality (that's Module 2)
- Does not manage ad campaigns (operator does this manually in Micro OS)
- Does not predict LTV beyond first purchase

### MODULE 2: Creative Hit-Rate Factory

**Purpose:** Enforce DS-001 doctrine on every creative concept. No creative enters the pipeline without hypothesis, research citation, and strategic justification.

**Input:** Creative brief submission (via Telegram or structured form).

**Mandatory fields (ALL required, no exceptions):**
1. `hypothesis` — IF-THEN-BECAUSE format. Example: "IF we test a UGC testimonial format with a hook showing the product solving [specific pain point] THEN we expect hook rate > 5% and CTR > 1.5% BECAUSE customer research shows [pain point] is the #1 mentioned issue in Amazon reviews."
2. `research_citation` — Specific finding from customer research or creative research. "Amazon reviews mention X" is valid. "I think this will work" is BLOCKED.
3. `marketing_angle` — Named strategic angle (e.g., "pain point: back pain relief", "social proof: influencer endorsement", "comparison: vs competitor X")
4. `format` — One of: UGC, VSL, DEMO, COMPARISON, TESTIMONIAL, CAROUSEL, STATIC
5. `awareness_stage` — COLD, WARM, or HOT
6. `avatar` — Target customer segment description

**Validation rules:**
- IF `hypothesis` is empty or does not contain "IF", "THEN", and "BECAUSE" → BLOCK
- IF `research_citation` is empty → BLOCK
- IF `is_iteration` = true AND `parent_creative_id` is null → BLOCK
- IF `is_iteration` = true AND parent creative status ≠ WINNER → BLOCK

**Iteration ratio enforcement (DS-001 Principle 5 — 80/20 Law):**
- Count of iterations in current cycle / total concepts in current cycle
- IF ratio < 0.80 AND this is a net-new concept → WARN: "Iteration ratio is {X}%. Minimum 80% iterations required. Submit an iteration of [winner name] instead, or provide explicit justification for net-new concept."
- Justification must cite a research finding not covered by any existing winner's angle.

**Output:** Creative status updated. Operator notification with validation result.

### MODULE 3: Testing Discipline Engine

**Purpose:** Enforce structured testing protocol with hard time boundaries, mandatory classification, and mandatory learning extraction.

**Test lifecycle:**

```
Day 0: Creative launched → status = LAUNCHED, test_start_date set
Day 1-3: Early signal monitoring
  - IF zero impressions after 72 hours → AUTO-KILL, no learning required (platform delivery failure, not creative failure)
Day 3-7: Testing window (NO action permitted)
  - No kills allowed (unless zero spend)
  - No scaling allowed
  - No modifications allowed
Day 7: First classification checkpoint
  - Pull metrics: ROAS, hook_rate, CTR, hold_rate, CPA, total spend
  - IF winner criteria met → status = WINNER
  - IF loser criteria clear → status = LOSER
  - IF inconclusive → extend to Day 14
Day 14: Final classification (mandatory)
  - IF still inconclusive → status = LOSER (inconclusive at 14 days = insufficient signal = kill)
```

**Winner criteria (ALL must be met):**
- Achieved top spend among active creatives (relative within the account)
- ROAS ≥ account profitability threshold (net margin ≥ 25% after all costs)
- Results consistent over ≥ 5 of the 7 test days (no single-day spikes)
- CPA ≤ target_cpa for ≥ 5 of the 7 test days

**Loser criteria (ANY triggers):**
- Zero or negligible spend after 7 days (platform chose not to serve it)
- ROAS below profitability threshold consistently (≥ 5 of 7 days)
- Hook rate below 3% (audience is not stopping)
- CPA > break_even_cpa for ≥ 5 of 7 days

**Learning extraction enforcement:**
- After classification (WINNER or LOSER), operator has 48 hours to submit learning
- Learning must include: finding, dimension (hook/angle/format/audience/trigger/proof/credibility), and either iteration hypothesis (for winners) or anti-pattern (for losers)
- IF learning not submitted within 48 hours → BLOCK next creative launch
- Block message: "Learning extraction overdue for creative [{name}]. Submit learning before launching new concepts."

### MODULE 4: Unit Economics Guard

**Purpose:** Continuous real-time monitoring of economic health. Blocks scaling and triggers kills when economic constraints are violated.

**Daily snapshot process:**
1. Pull from Shopify: orders, revenue, refunds for each active product
2. Pull from Meta: spend per creative, per product
3. Calculate per product: daily ROAS, daily CPA, daily net margin, daily refund rate
4. Store in `economic_snapshots` table
5. Evaluate kill thresholds (see Capital Gating Logic above)

**Monitoring intervals:**
- Economic snapshot: every 24 hours at 23:59 UTC
- Kill threshold evaluation: every 24 hours immediately after snapshot
- Refund monitoring: every 24 hours (refund data may lag 24-48 hours from Shopify)

**Actions on threshold breach:**

| Threshold | Action | Notification |
|-----------|--------|--------------|
| Net margin < 25% for 72h | BLOCK scaling, reduce budget to prior level | "⚠️ MARGIN ALERT: Net margin {X}% for {product}. Scaling blocked. Budget reverted to €{Y}/day." |
| Refund rate > 12% trailing 7d | PAUSE product ad spend | "🔴 REFUND ALERT: Refund rate {X}% for {product}. Ad spend paused. Investigate product quality." |
| CPA > break-even for 48h | PAUSE creative(s) exceeding threshold | "⚠️ CPA ALERT: CPA €{X} exceeds break-even €{Y} for creative [{name}]. Creative paused." |
| Net margin < 15% trailing 7d | GLOBAL KILL — all ad spend paused | "🔴 GLOBAL KILL: System-wide margin {X}%. ALL ad spend paused. Manual review required." |
| Daily P/L negative 3 consecutive days | BLOCK scaling, operator alert | "⚠️ P/L NEGATIVE: 3 consecutive loss days. Scaling blocked until 2 consecutive profit days." |

### MODULE 5: Failure Memory Lite

**Purpose:** Prevent repeated mistakes by maintaining an append-only log of learnings and anti-patterns.

**Storage:** `learning_log` table in Supabase + mirrored in Google Sheets for operator visibility.

**Learning types:**
- `OBSERVATION` — Single test finding, not yet confirmed
- `CONFIRMED_RULE` — Same finding confirmed across ≥ 3 tests, promoted to binding rule
- `ANTI_PATTERN` — Specific element proven to fail, must be avoided

**Duplicate hypothesis detection:**
- Before approving a new creative brief, the system checks the learning log for:
  - Same marketing angle + same format combination that previously resulted in LOSER
  - Same hook approach that has an ANTI_PATTERN entry
- IF duplicate detected → WARN: "Similar hypothesis tested on [{date}] with creative [{name}]. Result: LOSER. Anti-pattern: [{anti_pattern}]. If proceeding, document what is different this time."

**Promotion protocol (from DS-001 Section 9):**
- When the same learning appears as OBSERVATION in ≥ 3 tests → operator is notified
- Operator reviews and approves promotion to CONFIRMED_RULE
- Promoted rules become binding: future concepts violating the rule require explicit documented justification
- NO automated promotion — human approval mandatory (per UHD-OBS-003)

**Learning velocity tracking:**
```
weekly_velocity = count(learnings WHERE created_at > 7_days_ago AND learning_type IN ('OBSERVATION', 'CONFIRMED_RULE', 'ANTI_PATTERN')) / count(creatives WHERE status IN ('WINNER', 'LOSER') AND kill_date > 7_days_ago)
```

IF weekly_velocity < 1.0 for 2 consecutive weeks → BLOCK new concept production.

### MODULE 6: Velocity & Execution Engine

**Purpose:** Track operational cadence compliance and enforce speed requirements.

**Tracked metrics:**

| Metric | Target | Enforcement |
|--------|--------|-------------|
| Time from winner classification to first iteration launch | < 72 hours | Alert at 48h, block if > 72h without iteration brief submitted |
| Creative meeting cadence | Weekly, non-negotiable | Operator must confirm meeting held via Telegram command. If missed 2 consecutive weeks → BLOCK new concept production |
| Concepts in pipeline | 3–5 at all times (at Micro scale) | Warn if < 3 concepts active. Warn if > 5 concepts active at €35/day budget. |
| Test completion rate | 100% of tests reach classification | Alert for any test past 14 days without classification |

**Operational alerts (Telegram, daily at 09:00 operator local time):**

```
📊 DAILY BRIEFING — {date}

ACTIVE PRODUCTS: {count}
ACTIVE CREATIVES: {count} ({iterations}% iterations)
HIT RATE (4w): {X}%
DAILY SPEND: €{X} / €{budget}
DAILY REVENUE: €{X}
NET MARGIN: {X}%
CPA (avg): €{X} (BE: €{Y})
REFUND RATE (30d): {X}%

⏰ ACTIONS DUE:
- {creative_name}: Learning extraction due in {X}h
- {creative_name}: Test classification due in {X} days
- Creative meeting: {status}

🚫 BLOCKS ACTIVE:
- {block_reason if any}
```

## Micro OS Exclusions (Hard Prohibitions)

These are NOT permitted in Micro OS:

1. **No complex funnel routing** — Single-step: ad → product page → purchase. No upsell flows, no email sequences, no cart abandonment automation.
2. **No multi-stage automation** — Manual campaign management in Meta Ads Manager. System monitors and alerts; operator acts.
3. **No heavy retargeting** — Broad audience testing only. No custom audience building, no lookalike audiences, no retargeting campaigns.
4. **No multi-channel scaling** — Meta only. No TikTok, no Google, no Pinterest.
5. **No advanced personalization** — Same ad to same broad audience. No dynamic product ads, no personalized copy.
6. **No automated bid management** — Manual CBO or ABO. System does not touch Meta's campaign structure.
7. **No cross-product intelligence** — Each product is tested independently. No pattern sharing between products yet.

## Micro OS Signal Definition

A "signal" is defined as:

**Positive signal:** A creative that meets ALL winner criteria for ≥ 7 consecutive days AND produces a net-positive P/L for the product it promotes.

**No signal:** A creative that generates impressions and spend but does not meet winner criteria after 14 days.

**Negative signal:** A creative with zero spend after 72 hours, or a creative with CPA > 2× break-even for ≥ 7 days.

**Product-level signal:** A product has positive signal when it has ≥ 1 winner creative AND net margin ≥ 25% trailing 14 days AND refund rate ≤ 12%.

## Product Rotation Triggers

| Trigger | Action |
|---------|--------|
| Product has 0 winners after 9 concepts tested (3 batches of 3) | KILL product. Document anti-patterns. Select next candidate. |
| Product refund rate > 15% at any point | KILL product immediately. Do not iterate. Product quality issue. |
| Product margin < 20% after all optimizations | KILL product. Margin too thin for ad-funded acquisition. |
| All product candidates exhausted (3 products killed consecutively) | FULL STOP. Return to product research phase. No ad spend until new candidates with ≥ 30% gross margin are identified. |

---

# SECTION 3 — MACRO OS ARCHITECTURE (UNLOCKED PHASE)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## Macro OS Objective

Scale validated winners beyond $6M/day in ad spend while maintaining economic discipline, compounding learning velocity, and building structural competitive moats through cross-dataset intelligence, multi-SKU portfolio management, and systematic creative iteration at scale.

## Unlock Gate

The Macro OS activates ONLY when ALL 10 unlock criteria (Section 1, Capital Gating Logic) have been met simultaneously for 14 consecutive days. The unlock is not automatic — the operator must explicitly acknowledge the transition via a Telegram command that triggers a full audit:

1. System runs complete Scaling Readiness Check (all 8 conditions from DS-001 Section 12)
2. System runs complete unlock criteria check (all 10 conditions)
3. IF all pass → System presents audit report to operator
4. Operator confirms: `/unlock_macro {confirmation_code}`
5. `os_state.current_stage` → MACRO, `unlock_date` set

## Macro OS Modules (Activated Upon Unlock)

### MODULE M1: Multi-SKU Portfolio Manager

**Purpose:** Manage multiple products simultaneously with independent economic tracking, shared learning inheritance, and portfolio-level capital allocation.

**Capabilities:**
- Independent unit economics tracking per SKU (each product has its own margin floor, CPA ceiling, kill thresholds)
- Portfolio-level daily budget allocation based on product performance ranking
- Automatic budget reallocation: top-performing products receive budget increases; underperformers receive budget decreases
- Portfolio diversification enforcement: no single product may exceed 50% of total portfolio ad spend
- New product onboarding inherits all Failure Memory rules from existing products (angle anti-patterns, format preferences, hook patterns)

**Budget allocation algorithm:**
```
For each product in ACTIVE status:
  performance_score = (trailing_7d_roas × 0.4) + ((1 - refund_rate) × 0.2) + (hit_rate × 0.2) + (margin × 0.2)
  
Rank products by performance_score.
Allocate daily budget proportionally:
  top_quartile: 40% of total budget
  second_quartile: 30% of total budget
  third_quartile: 20% of total budget
  bottom_quartile: 10% of total budget (minimum viable testing budget)
```

Products in bottom quartile for 14 consecutive days → escalation review.

### MODULE M2: Cross-Product Intelligence Inheritance

**Purpose:** When a learning is confirmed as a CONFIRMED_RULE on Product A, evaluate whether it applies to Products B, C, D and propagate as a TEST_HYPOTHESIS (not a rule) to those products.

**Mechanism:**
1. When a learning is promoted to CONFIRMED_RULE on Product A:
   - System checks: is this learning dimension-specific (hook, format, angle) or domain-specific (audience, product-category)?
   - IF dimension-specific (e.g., "UGC format outperforms VSL for this price range") → generate test hypothesis for all products in the same price range
   - IF domain-specific (e.g., "back pain angle resonates for health products") → generate test hypothesis only for products in the same category
2. Generated hypotheses are queued in the Creative Hit-Rate Factory with `research_citation` = "Cross-product inheritance from Product A, Learning ID: {learning_id}"
3. These inherited hypotheses still require operator approval before launch
4. If the inherited hypothesis fails on Product B, it is recorded as a boundary condition: "Rule {X} applies to Product A but NOT Product B. Differentiating factor: {documented difference}"

**Anti-dilution safeguard:** Inherited hypotheses are tagged as `INHERITED` and tracked separately. An inherited hypothesis that fails does NOT invalidate the original rule on Product A. Rules are product-scoped until confirmed cross-product.

### MODULE M3: Cross-Dataset Strategy Blending Engine

**Purpose:** Combine strategic insights from multiple ingested datasets (currently DS-001; future datasets as they are ingested through DIF) while maintaining dataset doctrine integrity.

**Architecture:**
- Each dataset maintains its own immutable Dataset_Doctrine.md
- The blending engine operates at the PIPELINE_COMPOSITION.md layer ONLY
- No dataset's rules are modified — blending occurs through the Authority Matrix (Section 4 of PIPELINE_COMPOSITION.md)
- When datasets have overlapping domains, the Conflict Resolution Law (Section 5 of PIPELINE_COMPOSITION.md) determines precedence

**Current state:** Pipeline v1.0 contains a single dataset (DS-001). No blending is possible yet. This module activates structurally when a second dataset is ingested through DIF and added to the pipeline composition.

**Future datasets expected:**
- $73k/day Shopify scaling dataset → domain: unit economics, product selection, scaling velocity
- Static Ads Intelligence dataset → domain: static ad creative patterns, image-first ad strategy
- Additional Shopify scaling datasets → domain: multi-market expansion, logistics optimization

**Blending rules:**
- If DS-001 says "hit rate ≥ 10% before scaling" and a new dataset says "hit rate ≥ 8% is sufficient" → DS-001 prevails (per PIPELINE_COMPOSITION Section 5, earlier dataset with explicit domain authority wins)
- Operator may override by creating a new Pipeline Composition version with an explicit resolution rule
- No silent overrides — every resolution is documented and versioned

### MODULE M4: Refund Risk Predictor

**Purpose:** Use historical refund data to predict refund probability for new products before significant ad spend is committed.

**Input features:**
- Product category
- Price point
- COGS ratio
- Shipping method (standard vs express)
- Product weight
- Historical refund rates for similar products in the portfolio

**Logic (rule-based, not ML in V1 — no ML in NOT BUILDING):**
```
refund_risk_score = 
  (0.3 × category_avg_refund_rate) +
  (0.2 × price_point_risk) +    // higher price = higher refund risk
  (0.2 × shipping_time_risk) +  // longer shipping = higher refund risk
  (0.2 × margin_pressure) +     // thin margin = less buffer for refunds
  (0.1 × novelty_risk)          // new category with no data = higher risk
```

**Output:**
- `LOW` (refund_risk_score < 0.08): Standard monitoring
- `MEDIUM` (0.08 ≤ score < 0.12): Enhanced monitoring, refund alerts at 10% instead of 12%
- `HIGH` (score ≥ 0.12): Pre-launch warning to operator, tighter kill threshold (refund > 10% = auto-pause)

### MODULE M5: Cross-SKU Pattern ID Mapper

**Purpose:** Identify recurring creative patterns that work across multiple products and codify them as portfolio-level rules.

**Mechanism:**
1. After every test classification, the system checks: does this winning creative share attributes (hook type, format, angle, trigger) with winners on other products?
2. If ≥ 3 products have winners sharing the same attribute → flag as a CROSS_SKU_PATTERN
3. Cross-SKU patterns are logged in a dedicated `cross_sku_patterns` table
4. These patterns generate iteration hypotheses across all products that haven't tested them yet
5. Patterns that fail on ≥ 2 products after inheritance → demoted to PRODUCT_SPECIFIC

### MODULE M6: Portfolio Capital Allocator

**Purpose:** Dynamic daily budget allocation across the entire product portfolio based on economic performance, hit rate, and growth potential.

**Allocation tiers:**

| Tier | Criteria | Budget Share | Max Budget Growth/Day |
|------|----------|-------------|----------------------|
| STAR | ROAS > 3×, margin > 40%, hit rate > 20% | 40% of total | +20%/day |
| PERFORMER | ROAS > 2×, margin > 30%, hit rate > 15% | 30% of total | +10%/day |
| TESTING | Active testing, insufficient data | 20% of total | Flat |
| DECLINING | Declining metrics, needs iteration | 10% of total | -10%/day until review |

**Capital increase velocity:**
- Budget increases are capped at 20% per day for any single product
- Total portfolio budget increases are capped at 15% per day
- After every increase, 72 hours of stable metrics required before next increase
- This creates an exponential scaling curve: €200 → €240 → €288 → €346 → ... → $6M+/day over months, not days

### MODULE M7: Cross-Market Replication Engine (Macro Phase 2)

**Purpose:** Replicate validated products and creatives across geographic markets.

**Prerequisites:**
- Product validated as STAR tier in home market for ≥ 30 days
- Shipping infrastructure confirmed for target market
- Legal/compliance requirements verified (product allowed in target market)
- Localization completed (ads translated, landing page localized)

**Replication protocol:**
1. Clone product entry with market-specific pricing and shipping costs
2. Recalculate break-even CPA and margin floor for target market
3. Clone top 3 winning creative hypotheses, adapted for target market language/culture
4. Launch in target market at Micro OS budget level (35€/day equivalent in local currency)
5. Target market product goes through full Micro OS testing cycle independently
6. Learnings from target market are cross-referenced with home market learnings

### MODULE M8: Multi-Channel Sensor Integration (Macro Phase 2)

**Purpose:** Extend beyond Meta to TikTok, Google Ads, and other paid channels.

**Channel onboarding protocol:**
1. Each new channel starts at Micro OS budget level independently
2. Creative hypotheses are inherited from Meta winners but reformatted for the channel
3. Channel-specific metrics are added to the Testing Discipline Engine
4. Channel-specific kill thresholds are calibrated during first 30 days
5. Portfolio Capital Allocator treats each channel as an independent allocation tier

### MODULE M9: Creative Fatigue Predictor

**Purpose:** Detect winner fatigue before it causes revenue collapse (the $700K → $100K failure pattern from DS-001 Section 14).

**Signals monitored:**
- Hook rate decline: > 20% drop from peak over 7 days → FATIGUE_WARNING
- CTR decline: > 15% drop from peak over 7 days → FATIGUE_WARNING
- CPA increase: > 25% increase from average over 7 days → FATIGUE_WARNING
- Spend decline: platform reducing delivery > 30% week-over-week → FATIGUE_ALERT

**Response protocol:**
1. FATIGUE_WARNING (any single signal): Queue 3 iteration briefs for this winner within 48 hours
2. FATIGUE_ALERT (2+ signals simultaneously): Immediately deploy next iteration from queue, increase iteration velocity
3. FATIGUE_CRITICAL (3+ signals + spend declined > 50%): Trigger emergency iteration sprint — 5 variations within 72 hours

### MODULE M10: Cross-Brand Structural Moat Builder (Macro Phase 3)

**Purpose:** Build long-term competitive advantages that compound over time.

**Moat components:**
1. **Learning database depth:** The more tests run, the more confirmed rules exist, the higher the hit rate for new products. This is a compounding advantage — competitors starting from zero cannot replicate the learning database.
2. **Cross-product pattern library:** Patterns validated across ≥ 5 products become near-certain winners for new products in similar categories.
3. **Creative velocity infrastructure:** Faster iteration cycles mean winners are exploited longer and fatigue is managed proactively.
4. **Economic governance automation:** Automated kill switches, margin enforcement, and refund monitoring prevent the capital destruction that manually-operated competitors experience.
5. **Dataset doctrine accumulation:** Each new dataset ingested through DIF adds strategic depth that no competitor can access.

## How Compounding Beyond $6M/Day Occurs

The path from €35/day to $6M+/day is not linear growth — it is exponential compounding through five reinforcing loops:

### Loop 1: Learning Velocity Compounding

Each test produces a learning. Each learning improves the next test's hypothesis quality. Higher-quality hypotheses produce higher hit rates. Higher hit rates mean more winners per testing cycle. More winners mean more iteration parents. More iteration parents mean more high-probability iterations. The system gets measurably better at finding winners over time.

**Quantified:** At 15% hit rate with 10 concepts/week, the system produces 1.5 winners/week. Each winner generates 5+ iterations. After 3 months: ~18 winners, ~90 iterations, ~270 documented learnings, ~30 confirmed rules. A competitor starting from zero needs 3+ months to build equivalent intelligence.

### Loop 2: Capital Efficiency Compounding

Higher hit rate = lower effective CPA across the portfolio. Lower CPA = higher margin. Higher margin = more capital available for reinvestment. More capital = more tests. More tests = more learnings (Loop 1). The system becomes more capital-efficient as it scales.

**Quantified:** At 15% hit rate, ~85% of creative spend goes to losers. At 25% hit rate (achievable after 6 months of learning accumulation), only ~75% goes to losers. On a $1M/day budget, that's $100K/day more efficient spend — the equivalent of $36.5M/year in recovered waste.

### Loop 3: Portfolio Diversification Compounding

More validated products = more stable revenue. More stable revenue = more confidence in budget increases. More budget = more tests across more products (Loop 1). Diversification also reduces dependency risk — no single product failing can collapse the business.

**Quantified:** With 10 validated products, the top product represents at most 30% of revenue (per dependency concentration rules). A single product failure costs at most 30% of revenue, not 100%.

### Loop 4: Cross-Product Intelligence Compounding

A confirmed rule on Product A generates a high-probability hypothesis for Products B–J. Cross-product inheritance means every new product starts at a higher baseline hit rate than the previous product. By Product #10, the starting hit rate may be 25%+ instead of the original 10%.

**Quantified:** If cross-product inheritance provides 5 pre-validated hypotheses per new product, and each has a 30% probability of winning (vs 15% for cold hypotheses), the new product's effective starting hit rate is ~30% for inherited concepts and ~15% for net-new concepts. Blended: ~25%.

### Loop 5: Structural Governance Compounding

The governance system itself compounds. Every production failure adds a new law to the Universal Hardening Doctrine. Every law prevents a class of failures across ALL projects. Over time, the failure rate approaches zero for known failure classes. The system becomes harder to break the longer it operates.

**Quantified:** After 50 production failures documented and codified, the system has 50 failure classes it is immune to. A competitor without governance documentation is vulnerable to all 50.

### Scaling Mathematics

Starting at €35/day with 20% daily budget increase capacity (when conditions met):

```
Week 1-4: €35/day → validate signal, prove engine
Week 5-8: €35 → €50/day (first cautious increase after validation)
Week 9-12: €50 → €100/day (compound if metrics hold)
Month 4-6: €100 → €500/day (add second product, cross-product learning begins)
Month 6-9: €500 → €2,000/day (portfolio diversification, 3-5 products)
Month 9-12: €2,000 → €10,000/day (multi-product portfolio, cross-product patterns active)
Year 2: €10,000 → €50,000/day (multi-market expansion begins)
Year 2-3: €50,000 → €500,000/day (multi-channel, multi-market, portfolio at scale)
Year 3+: €500,000 → $6M+/day (full Macro OS, all modules active, structural moat established)
```

This is not a projection — it is the mathematical maximum given the compounding loops and budget increase caps. Actual velocity depends on hit rate, product quality, and market size. The system enforces that scaling ONLY occurs when metrics justify it.

---

# SECTION 4 — DATASET VALUE EXTRACTION PROTOCOL (DVEP)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## Protocol Purpose

The DVEP is the operational implementation of the Dataset Ingestion Firewall (DIF) for the TUAX Revenue Discipline OS. It defines the exact steps, artifacts, and validation gates for extracting, documenting, and integrating every dataset in the knowledge vault repository.

## Protocol Authority

This protocol operates under the DIF (DATASET_INGESTION_FIREWALL.md) and inherits all its rules, prohibitions, and immutability clauses. No step in this protocol may be skipped, shortened, or combined regardless of dataset size, perceived simplicity, or time pressure.

## Per-Dataset Extraction Process

For EACH dataset in the repository at the pinned SHA (`116b494583422d2125b92b01ea6fda5a9977db17`):

### STEP 1 — RECEIVE & CLASSIFY

| Action | Detail |
|--------|--------|
| Accept the dataset | Download/access from repository at pinned SHA |
| Classify per DIF Section 2 | Determine input type (PDF, MD, ZIP, etc.) |
| Assign unique identifier | Format: DS-{NNN} (sequential) |
| Record metadata | Source, format, date received, version, file count, total size |

### STEP 2 — ISOLATE

| Action | Detail |
|--------|--------|
| Create isolated processing context | Separate conversation, separate project, or separate session |
| Verify isolation | No other dataset's artifacts, rules, or content present |
| IF isolation cannot be guaranteed | BLOCK and report |

### STEP 3 — EXTRACT (Full Pass)

| Action | Detail |
|--------|--------|
| Read ENTIRE dataset | No sampling, no skipping, no summarization |
| For ZIP archives | Extract ALL files, verify 100% extraction complete |
| For multi-file datasets | Process EVERY file |
| Record | Total content volume, structure observed, extraction errors |

### STEP 4 — ANSWER MANDATORY QUESTIONS

For each dataset, answer ALL five questions in writing:

1. **What domain does this dataset govern?** — Specific, bounded answer required. "General" is forbidden.
2. **What economic value does this dataset encode?** — At least one of: revenue generation, cost reduction, risk mitigation, competitive advantage, compliance.
3. **What are the internal rules or heuristics this dataset establishes?** — Explicit enumeration required. "See dataset" is forbidden.
4. **What conflicts could arise if this dataset's rules are applied alongside other datasets?** — At least one potential conflict identified, or explicit statement of no overlap.
5. **What would be lost if this dataset were summarized to 20% of its content?** — Specific losses named. "Nothing" is forbidden unless provably redundant.

IF any question cannot be answered → return to STEP 3 for re-extraction.
IF still unanswerable after re-extraction → classify as UNCERTAIN, flag for human review, BLOCK.

### STEP 5 — PRODUCE ARTIFACTS

#### Artifact 1: Dataset_Doctrine.md

Required sections:

| Section | Content |
|---------|---------|
| Identity | Name, source, format, date, version |
| Domain | Specific bounded domain this dataset governs |
| Core Rules | EVERY rule/heuristic/principle — enumerated, not summarized |
| Key Concepts | Every concept introduced with dataset's own definition |
| Decision Heuristics | IF-THEN logic extracted verbatim or faithful paraphrase |
| Data Entities | Schemas, taxonomies, classifications defined |
| Contradictions | Internal contradictions documented without resolution |
| Boundary Statements | What dataset does NOT cover |
| Extraction Confidence | Per-section: VERBATIM, INFERRED, or UNCERTAIN |

**Forbidden:** Summarization that reduces rule count, paraphrasing that softens hard rules, omission of "obvious" rules, injection of external rules, placeholder sections.

#### Artifact 2: Agent_Registry.md

Required sections:

| Section | Content |
|---------|---------|
| Candidate Agents | List of agents this dataset could power, one-sentence justification each |
| Required Inputs | What each agent needs as input |
| Required Outputs | What each agent produces |
| Constraints | What each agent MUST NOT do (from boundary statements) |
| Dependencies | External systems/APIs/data required |
| Integration Risk | Per-agent: what could go wrong on integration |

**Forbidden:** Agents requiring knowledge from un-ingested datasets, vaguely described agents, agents not fully derivable from this doctrine.

#### Artifact 3: Isolated Economic Valuation

Required sections:

| Section | Content |
|---------|---------|
| Value Type | Revenue generation / cost reduction / risk mitigation / competitive advantage / compliance |
| Value Mechanism | HOW value is generated (causal chain) |
| Standalone Viability | Can this power a standalone product? YES/NO with justification |
| Dependency Risk | What value lost if this dataset removed after integration? |
| Perishability | Does value degrade over time? Rate and refresh triggers |

### STEP 6 — GENERATE DERIVED MAPS

For each dataset, generate:

#### Decision Map
Enumeration of every decision point in the dataset with:
- Decision ID
- Input conditions
- Output action
- Threshold values
- Source section reference

#### Risk Map
Enumeration of every risk identified in the dataset with:
- Risk ID
- Risk description
- Probability assessment (from dataset's own framing)
- Impact if materialized
- Mitigation defined in dataset
- Residual risk

#### Mutation Map
Enumeration of every element that changes over time:
- Element name
- What triggers change
- Direction of change
- Monitoring method
- Response protocol

#### Pattern ID Map
Enumeration of every repeatable pattern:
- Pattern name
- Conditions for pattern activation
- Expected outcome
- Confidence level (how many data points support it)
- Cross-product applicability assessment

### STEP 7 — VALIDATE

Cross-check all artifacts:
- Does the doctrine contain every rule found during extraction?
- Does the Agent Registry reference only rules present in the doctrine?
- Does the Economic Valuation cite mechanisms present in the doctrine?
- Do all derived maps trace to specific doctrine sections?

IF any cross-check fails → return to STEP 5 and correct.

### STEP 8 — REGISTER

- Add dataset and artifacts to system dataset registry
- Record: dataset ID, artifact locations, ingestion date, validation status
- Dataset is now available for integration consideration

### STEP 9 — HOLD

Dataset is ingested but NOT integrated. Integration requires:
- Specific PRD feature requesting this dataset
- Bounded integration scope
- Conflict analysis against already-integrated datasets
- Addition to PIPELINE_COMPOSITION.md as a new version

## Integration into Pipeline Systems

### CPM (Causal Performance Memory) — Macro OS Only

The CPM is the system's long-term memory of causal relationships between creative decisions and performance outcomes. It is built from the structured output of the Testing Discipline Engine and Learning Log.

**CPM structure:**
```
For each confirmed causal pattern:
  - Cause: {specific creative decision: hook type, angle, format, proof element}
  - Effect: {specific performance outcome: ROAS change, hook rate change, CPA change}
  - Confidence: {number of tests confirming this pattern}
  - Scope: {which products/categories this applies to}
  - Counter-examples: {tests where this pattern did NOT hold}
  - Boundary conditions: {when this pattern breaks}
```

The CPM is READ-ONLY for all automated systems (per UHD-OBS-001). Only human-approved rule promotions may add entries to the CPM.

### Governance Layer Integration

Every dataset's doctrine is referenced in the PIPELINE_COMPOSITION.md Authority Matrix. The Authority Matrix determines which dataset governs which domain. No dataset may be applied outside its declared authority.

### Cross-Dataset Blending Engine Integration

When multiple datasets are registered, the blending engine:
1. Identifies overlapping domains in the Authority Matrix
2. Creates Shared Authority Declarations where overlap exists
3. Defines deterministic resolution rules for each overlap
4. Documents resolutions in PIPELINE_COMPOSITION.md Section 5
5. Never modifies any dataset's doctrine — resolution occurs at the composition layer only

## Anti-Dilution Safeguards

| # | Safeguard | Enforcement |
|---|-----------|-------------|
| 1 | No dataset may be summarized during extraction | DIF Section 5, Forbidden Shortcut #2 |
| 2 | No two datasets may be merged into a single doctrine | DIF Section 5, Forbidden Shortcut #1 |
| 3 | No rule may be softened from MUST to SHOULD | PIPELINE_COMPOSITION Section 6, Prohibition #2 |
| 4 | No rule may be added that is not traceable to a source dataset | PIPELINE_COMPOSITION Section 6, Prohibition #3 |
| 5 | No dataset content may be applied outside its validated domain | PIPELINE_COMPOSITION Section 6, Scope Leakage Prevention |
| 6 | No "meta-dataset" or "unified doctrine" may be created before individual doctrines exist | DIF Section 7, Explicit Prohibition |
| 7 | Dataset doctrines are immutable post-validation | PIPELINE_COMPOSITION Section 8 |
| 8 | Prior conversation about a dataset does not constitute ingestion | DIF Section 10, Supremacy Clause |

---

# SECTION 5 — GOVERNANCE STRUCTURE

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## Required File Structure

```
/
├── CONSTITUTION.md                          # Supreme governance — execution rules
├── PIPELINE_COMPOSITION.md                  # Dataset composition layer — v1.0
├── EXECUTION_PIPELINE.md                    # Step-by-step execution gates
├── FAILURE_TAXONOMY.md                      # Failure classification rules
├── ENFORCEMENT_LINTER.md                    # Pre-delivery lint gate
├── N8N_API_MCP_BRIDGE.md                    # n8n schema truth layer
├── PROJECT_RULES.md                         # Project-level enforcement
├── DATASET_INGESTION_FIREWALL.md            # DIF — dataset operations
├── Repo-SHA_Governance_Law.md               # Repository authority
├── UNIVERSAL_WORKFLOW_GOVERNANCE.md          # Abstract anti-failure laws
├── UNIVERSAL_HARDENING_DOCTRINE.md          # Concrete anti-recurrence rules
│
├── /datasets/
│   ├── /DS-001_100k_day_Creative_System/
│   │   ├── Dataset_Doctrine.md              # Immutable doctrine (existing)
│   │   ├── Agent_Registry.md                # Agent candidates from DS-001
│   │   ├── Economic_Valuation.md            # Isolated economic valuation
│   │   ├── Decision_Map.md                  # All decision points enumerated
│   │   ├── Risk_Map.md                      # All risks enumerated
│   │   ├── Mutation_Map.md                  # All time-varying elements
│   │   └── Pattern_ID_Map.md                # All repeatable patterns
│   │
│   ├── /DS-002_73k_day_Shopify/             # (Future — after DIF ingestion)
│   │   ├── Dataset_Doctrine.md
│   │   ├── Agent_Registry.md
│   │   ├── Economic_Valuation.md
│   │   ├── Decision_Map.md
│   │   ├── Risk_Map.md
│   │   ├── Mutation_Map.md
│   │   └── Pattern_ID_Map.md
│   │
│   └── /DS-003_Static_Ads_Intelligence/     # (Future — after DIF ingestion)
│       └── ... (same artifact structure)
│
├── /os/
│   ├── Cross_Dataset_Rules.md               # Rules derived from dataset interactions
│   ├── Conflict_Resolution.md               # Deterministic resolution for overlaps
│   ├── Doctrine_Linter.md                   # Automated doctrine validation rules
│   ├── Global_Agent_Index.md                # Master index of all agents across all datasets
│   ├── TUAX_Configurable_Constants.md       # ALL numeric thresholds (single source)
│   └── TUAX_Dataset_Deprecation_Log.md      # Append-only deprecation tracking
│
├── /repo/
│   ├── Repo_SHA_Lock.md                     # Current pinned SHA declaration
│   └── Dataset_Ingestion_Firewall.md        # (Symlink or copy of root DIF)
│
├── /workflows/
│   ├── Revenue_Discipline_OS.json           # Master workflow (n8n)
│   ├── Creative_Testing_Governed.json       # Creative testing pipeline
│   ├── Product_Scanner.json                 # Product candidacy validation
│   ├── Unit_Economics_Guard.json             # Economic monitoring
│   └── Error_Workflow.json                  # Auto-repair + escalation
│
├── /prompts/
│   ├── Claude_Code_System_Prompt.md         # Compiler prompt for Claude Code
│   ├── Creative_Brief_Validator.md          # Prompt for validating creative briefs
│   └── Learning_Extraction_Prompt.md        # Prompt for structured learning extraction
│
└── /audit/
    ├── Execution_Log.md                     # Append-only execution history
    ├── Governance_Hardening_Report.md        # Per-project hardening report
    └── Failure_Reports/                     # Individual failure report files
        └── UHD-FAIL-001.md                  # (Example)
```

## What Each File Enforces

| File | Enforcement Scope |
|------|-------------------|
| CONSTITUTION.md | Supreme execution rules: one-shot principle, mode separation, failure intolerance, governance safety |
| PIPELINE_COMPOSITION.md | Which datasets exist, which domains they govern, how conflicts are resolved |
| EXECUTION_PIPELINE.md | Step order: PRD → Architecture → Compilation → Execution. No step may be skipped or reordered |
| FAILURE_TAXONOMY.md | Classification of all failure types. Determines auto-repair eligibility vs human escalation |
| ENFORCEMENT_LINTER.md | Pre-delivery validation gate. All artifacts must pass lint before delivery |
| N8N_API_MCP_BRIDGE.md | n8n schema truth. No node parameters may be invented. Schema must be confirmed via API/MCP |
| PROJECT_RULES.md | Project-level guardrails that enforce Constitution within ChatGPT/Claude projects |
| DATASET_INGESTION_FIREWALL.md | Dataset operations: ingestion, isolation, extraction, validation, registration, integration |
| Repo-SHA_Governance_Law.md | Repository authority. All governance must be SHA-pinned. No floating references |
| UNIVERSAL_WORKFLOW_GOVERNANCE.md | Abstract anti-failure laws: hostile input posture, determinism, state machines, routing safety |
| UNIVERSAL_HARDENING_DOCTRINE.md | Concrete rules: .item prohibition, URL safety, Switch schema, messaging platform laws |
| Cross_Dataset_Rules.md | Rules that emerge from interactions between multiple datasets |
| Conflict_Resolution.md | Deterministic resolution for domain overlaps between datasets |
| Doctrine_Linter.md | Automated validation that doctrines are complete, well-formed, and internally consistent |
| Global_Agent_Index.md | Master index of every agent candidate across all datasets, with status and dependencies |
| TUAX_Configurable_Constants.md | Single source of truth for ALL numeric thresholds, limits, and intervals |
| TUAX_Dataset_Deprecation_Log.md | Append-only record of dataset replacements and deprecations |
| Repo_SHA_Lock.md | Declaration of current pinned SHA with pin date and authority |

## Illegal States

| # | Illegal State | Detection Method | Response |
|---|--------------|------------------|----------|
| 1 | Workflow executing without valid PRD | PRD validation gate in EXECUTION_PIPELINE Step 0 | BLOCK |
| 2 | Dataset referenced but not DIF-ingested | Cross-reference workflow references against /datasets/ directory | BLOCK |
| 3 | Agent operating without governing SHA | SHA declaration check in agent initialization | BLOCK |
| 4 | Two datasets governing same domain without Shared Authority Declaration | Authority Matrix audit in PIPELINE_COMPOSITION | BLOCK |
| 5 | Constant defined in workflow but not in Configurable Constants file | Grep for hardcoded numbers in workflow JSON vs constants file | BLOCK |
| 6 | Kill-switch without deactivation protocol | Kill-switch definition audit per UHD-KILL-001 | BLOCK |
| 7 | Learning promoted to rule without human approval | Promotion log audit per UHD-OBS-003 | ROLLBACK + BLOCK |
| 8 | Dataset doctrine modified after validation | SHA comparison of doctrine files | PIPELINE INVALID |
| 9 | Workflow with .item accessor in any expression | Regex scan per UHD-EXPR-001 | BLOCK |
| 10 | Telegram message with parse_mode "Markdown" (Legacy) | Node parameter scan per UHD-MSG-001 | BLOCK |
| 11 | Switch node missing conditions.options | Schema validation per UHD-SWITCH-001 | BLOCK |
| 12 | Any node without explicit parameter values (relying on defaults) | Parameter completeness scan per LAW 2.1 | BLOCK |
| 13 | Pipeline output not traceable to a source dataset | Traceability audit per PIPELINE_COMPOSITION Section 9 | BLOCK |
| 14 | Creative launched without IF-THEN-BECAUSE hypothesis | Hypothesis field validation in Creative Hit-Rate Factory | BLOCK |
| 15 | Scaling attempted with hit rate < 10% | os_state.hit_rate_trailing_4w check | BLOCK |

## Block Conditions Summary

| Block Type | Source Document | Trigger | Required Resolution |
|------------|----------------|---------|---------------------|
| PRD_VIOLATION | CONSTITUTION.md | Missing/incomplete/ambiguous PRD | Provide complete PRD |
| SCHEMA_VIOLATION | N8N_API_MCP_BRIDGE.md | Node parameter not confirmed by API | Confirm schema via n8n API/MCP |
| DIF_VIOLATION | DATASET_INGESTION_FIREWALL.md | Dataset used before full ingestion | Complete DIF Order of Operations |
| GOVERNANCE_VIOLATION | Repo-SHA_Governance_Law.md | Asset not present at pinned SHA | Commit asset to repo, advance SHA |
| LINT_FAILURE | ENFORCEMENT_LINTER.md | Artifact fails lint after one repair pass | Fix per lint rule, or escalate |
| PIPELINE_VIOLATION | PIPELINE_COMPOSITION.md | Dataset scope overlap without resolution | Add resolution to composition file |
| HARDENING_FAILURE | UNIVERSAL_HARDENING_DOCTRINE.md | Any H-check fails | Fix per source law |
| ECONOMIC_VIOLATION | Unit Economics Guard (this PRD) | Margin/refund/CPA threshold breached | Restore metrics to compliant range |
| DISCIPLINE_VIOLATION | Testing Discipline Engine (this PRD) | Learning not extracted, meeting missed, etc. | Complete required action |

---

# SECTION 6 — CLAUDE CODE PROMPT

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

```
# CLAUDE CODE EXECUTION PROMPT — TUAX REVENUE DISCIPLINE OS v1.0

## GOVERNING AUTHORITY

governing_repo: https://github.com/{owner}/TUAX-Knowledge-Vault
pinned_sha: 116b494583422d2125b92b01ea6fda5a9977db17
constitution_version: CONSTITUTION.md v1.0
pipeline_composition_version: PIPELINE_COMPOSITION.md v1.0
uhd_version: UNIVERSAL_HARDENING_DOCTRINE.md v1.0
uwg_version: UNIVERSAL_WORKFLOW_GOVERNANCE.md v1.0
repo_sha_law_version: Repo-SHA_Governance_Law.md v1.0

## ACKNOWLEDGEMENT (BINDING)

I acknowledge that:
1. The governing repository is: {governing_repo}
2. The pinned commit SHA is: 116b494583422d2125b92b01ea6fda5a9977db17
3. Only assets physically present at this SHA are authoritative
4. No governance rule may be reinterpreted, summarized, or extrapolated
5. Absence of explicit permission constitutes prohibition
6. All decisions must be reproducible from repo contents and logged inputs
7. This acknowledgement is binding for the duration of this operation

## MODE: EXECUTION

This prompt operates in EXECUTION mode only.
Zero commentary. Zero explanation. Artifacts only.

## PRD REFERENCE

PRD: TUAX Revenue Discipline OS v1.0 (MASTER PRD — Section 1)
PRD validation status: VALID (all 12 sections present, no placeholders, NOT BUILDING explicit, success metrics defined)

## TASK SEQUENCE

Execute the following tasks in order. Each task produces exactly one artifact.
If any task fails validation, BLOCK and report per FAILURE_TAXONOMY.md.

### TASK 1: Update Governance Files

1.1. Read the current state of all governance files at the pinned SHA.
1.2. Verify all governance files exist and pass integrity checks (UTF-8, no placeholders, complete).
1.3. Create/update Repo_SHA_Lock.md:

```markdown
# REPO SHA LOCK

governing_repo: {repo_uri}
pinned_sha: 116b494583422d2125b92b01ea6fda5a9977db17
pin_date: 2026-02-11
pin_authority: system_owner
status: ACTIVE

All agents, workflows, and systems governed by this repository
MUST reference this SHA. Floating references are ILLEGAL.
```

1.4. Create/update TUAX_Configurable_Constants.md with all numeric thresholds from the PRD:

| Constant Name | Value | Unit | Referenced By |
|---------------|-------|------|---------------|
| margin_floor_micro | 0.25 | ratio | PRD Section 1, Unit Economics Guard |
| margin_floor_macro_unlock | 0.30 | ratio | PRD Section 1, Unlock Criteria |
| refund_ceiling_micro | 0.12 | ratio | PRD Section 1, Kill Thresholds |
| refund_ceiling_macro_unlock | 0.10 | ratio | PRD Section 1, Unlock Criteria |
| hit_rate_minimum_scaling | 0.10 | ratio | PRD Section 1, DS-001 Principle 1 |
| hit_rate_target | 0.15 | ratio | PRD Section 1, DS-001 |
| hit_rate_unlock | 0.15 | ratio | PRD Section 1, Unlock Criteria |
| hit_rate_critical_failure | 0.05 | ratio | PRD Section 1, Kill Thresholds |
| daily_budget_micro_initial | 35 | EUR | PRD Section 2 |
| daily_budget_micro_max | 200 | EUR | PRD Section 1, Capital Gating |
| max_creatives_micro | 3 | count | PRD Section 2 |
| test_duration_min | 7 | days | PRD Section 2, DS-001 Section 7 |
| test_duration_max | 14 | days | PRD Section 2, DS-001 Section 7 |
| kill_zero_spend_hours | 72 | hours | PRD Section 2 |
| kill_cpa_above_be_hours | 48 | hours | PRD Section 2 |
| kill_negative_pl_days | 3 | days | PRD Section 2 |
| kill_global_margin_floor | 0.15 | ratio | PRD Section 2 |
| kill_global_margin_window | 7 | days | PRD Section 2 |
| learning_extraction_sla | 48 | hours | PRD Section 2 |
| iteration_ratio_minimum | 0.80 | ratio | DS-001 Principle 5 |
| iteration_launch_sla | 72 | hours | PRD Section 2 |
| dependency_top1_max | 0.30 | ratio | DS-001 Section 10 |
| dependency_top1_critical | 0.40 | ratio | DS-001 Section 10 |
| dependency_top1_emergency | 0.60 | ratio | DS-001 Section 10 |
| unlock_consecutive_days | 14 | days | PRD Section 1, Unlock Criteria |
| budget_increase_max_per_day | 0.20 | ratio | PRD Section 3 |
| portfolio_increase_max_per_day | 0.15 | ratio | PRD Section 3 |
| product_max_concepts_before_kill | 9 | count | PRD Section 2 |
| learning_velocity_minimum | 1.0 | ratio | PRD Section 1, DS-001 Section 4 |
| platform_fee_rate_default | 0.035 | ratio | PRD Section 1 |

1.5. Create TUAX_Dataset_Deprecation_Log.md (empty, append-only):

```markdown
# TUAX Dataset Deprecation Log

| Date | Action | Old Dataset | New Dataset | Affected Doctrines | Verified By |
|------|--------|-------------|-------------|-------------------|-------------|
| (no entries) | | | | | |
```

### TASK 2: Validate DS-001 Doctrine

2.1. Read datasets/DS-001_100k_day_Creative_System/Dataset_Doctrine.md at pinned SHA.
2.2. Validate all required sections exist per DIF Section 4.
2.3. Verify no placeholder content.
2.4. Verify all 8 principles are present and enumerated.
2.5. Verify all decision variables have explicit thresholds.
2.6. Verify all forbidden optimizations are enumerated.
2.7. IF validation passes → confirm in execution log.
2.8. IF validation fails → BLOCK with exact missing section.

### TASK 3: Generate/Update Agent_Registry.md for DS-001

3.1. Based on the validated DS-001 doctrine, produce Agent_Registry.md:

Candidate Agents (derived from DS-001):

| Agent Name | Justification | Required Inputs | Required Outputs | Constraints | Dependencies |
|------------|--------------|-----------------|------------------|-------------|--------------|
| Creative Hypothesis Validator | Enforces DS-001 Principle 2 (Intent Before Production) | Creative brief fields | APPROVED / BLOCKED + reason | Must not invent hypotheses; must not approve without research citation | Whole Creative Document |
| Test Classification Engine | Enforces DS-001 Section 7 (Scientific Testing Protocol) | 7-14 day performance metrics | WINNER / LOSER / INCONCLUSIVE + learning extraction trigger | Must not classify before Day 7; must force classification by Day 14 | Meta Ads API, economic_snapshots |
| Winner Breakdown Generator | Enforces DS-001 Principle 8 (Winners Must Be Understood) | Winner creative + performance data | Scene-by-scene breakdown, hypothesized success drivers | Must not accept "it just worked"; must enumerate specific elements | Creative asset access |
| Iteration Queue Manager | Enforces DS-001 Principle 5 (80/20 Law) + Section 8 (Iteration Doctrine) | Active winners list, current iteration ratio | Iteration brief templates, ratio compliance status | Must not allow net-new concepts to exceed 20% without justification | creatives table, learning_log |
| Learning Velocity Tracker | Enforces DS-001 Section 9 (Learning Loop) | test_results, learning_log | Weekly velocity metric, block triggers, promotion candidates | Must not promote learnings without human approval; must not auto-generate rules | Supabase, Google Sheets |
| Dependency Concentration Monitor | Enforces DS-001 Section 10 (Fragility Control) | Spend data per creative | Concentration percentages, iteration sprint triggers | Must not turn off winners without documented justification | Meta Ads API |
| Scaling Readiness Assessor | Enforces DS-001 Section 12 (Scaling Readiness) | All 8 readiness conditions | PASS / FAIL per condition, overall READY / NOT READY | Must not permit scaling if any condition fails; no overrides | All data tables |
| Creative Meeting Enforcer | Enforces DS-001 Section 11 (Operational Cadence) | Meeting confirmation events | Compliance status, block triggers if missed 2+ weeks | Must block concept production after 2 missed meetings | Telegram bot events |

### TASK 4: Generate Economic_Valuation.md for DS-001

4.1. Based on the validated DS-001 doctrine, produce Economic_Valuation.md:

| Section | Content |
|---------|---------|
| Value Type | Revenue generation (direct: higher hit rate = more winning ads = more profitable spend) + Cost reduction (less waste on losing creatives) + Competitive advantage (compounding learning system) |
| Value Mechanism | The doctrine enforces hypothesis-driven creative production which increases hit rate from typical 1-3% to target 15-20%. At scale, this means ~5-7x more revenue per creative dollar spent. The learning loop compounds: each cycle's learnings improve next cycle's hit rate. |
| Standalone Viability | YES — this doctrine alone can power a complete creative operations system for any DTC e-commerce brand |
| Dependency Risk | CRITICAL — if removed after integration, the system loses all creative governance: hypothesis requirements, testing protocol, iteration doctrine, learning loops, scaling readiness criteria. The system would revert to unstructured creative production. |
| Perishability | LOW for principles (hit rate before volume, intent before production are evergreen). MEDIUM for platform-specific tactics (Meta algorithm changes may alter optimal test duration). Refresh trigger: when hit rate drops below 10% for 4+ weeks despite doctrine compliance. |

### TASK 5: Generate Workflow Artifacts

For each workflow, validate against n8n API/MCP schema before producing JSON.
All workflows must pass ENFORCEMENT_LINTER.md and UNIVERSAL_HARDENING_DOCTRINE.md pre-deploy checklist.

5.1. Product_Scanner.json
- Trigger: Telegram command `/scan_product {shopify_url}`
- Nodes: Shopify product fetch → Margin calculator (Code node) → Gatekeeper validation → Supabase write → Telegram response
- All expressions use $json (no .item)
- All Telegram nodes: parse_mode HTML, appendAttribution false
- Error workflow configured

5.2. Creative_Testing_Governed.json
- Trigger: Telegram command `/launch_creative`
- Nodes: Brief intake → Hypothesis validator (Code node) → Research citation check → Iteration ratio check → Supabase write → Telegram confirmation
- Learning extraction timer: 48h after classification → Telegram reminder
- Block logic: IF learning not submitted → block next launch

5.3. Unit_Economics_Guard.json
- Trigger: Cron (daily 23:59 UTC)
- Nodes: Shopify orders fetch → Meta spend fetch → Margin calculator → Threshold evaluator (Switch node with rules.values, fallbackOutput extra, conditions.options present) → Kill action dispatcher → Supabase snapshot write → Telegram alert
- All kill thresholds from TUAX_Configurable_Constants.md (referenced in Code node comments)

5.4. Velocity_Tracker.json
- Trigger: Cron (daily 09:00 operator local time)
- Nodes: Supabase read (active creatives, test results, learning log, economic snapshots) → Daily briefing compiler (Code node) → Telegram daily briefing message
- Includes: action items due, blocks active, hit rate, margin status

5.5. Error_Workflow.json
- Trigger: n8n error trigger
- Nodes: Error context capture → Failure classifier (Code node: FIXABLE_AUTOMATICALLY vs HUMAN_ACTION_REQUIRED) → Conditional branch → Auto-repair (for fixable) OR Operator notification (for human-required)
- Per CONSTITUTION.md Auto-Repair rules: never auto-repair credentials, permissions, billing, PRD violations

### TASK 6: Enforce Doctrine Compliance

6.1. Before any workflow execution, validate:
- TUAX_Configurable_Constants.md exists and is current
- All constants referenced in workflows match constants file values
- DS-001 Dataset_Doctrine.md exists and has not been modified (SHA check)
- PIPELINE_COMPOSITION.md lists DS-001 as the only included dataset
- All governance files pass integrity checks

6.2. If any validation fails → BLOCK execution, report missing/invalid file.

### TASK 7: Decision Logging (CPM Foundation)

7.1. Every workflow execution logs to Supabase `execution_log` table:
- execution_id, timestamp, workflow_name, trigger_type
- governing_sha: 116b494583422d2125b92b01ea6fda5a9977db17
- rules_applied: list of doctrine sections applied
- decision: the action taken
- outcome: SUCCESS, BLOCKED, ESCALATED
- block_reason: if BLOCKED

7.2. This log is APPEND-ONLY. No entries may be modified or deleted.

### TASK 8: Block Scale Without Margin Compliance

8.1. Before any budget increase operation:
- Read os_state.hit_rate_trailing_4w
- Read trailing 14-day net margin from economic_snapshots
- Read trailing 30-day refund rate from economic_snapshots
- Evaluate ALL scaling readiness conditions

8.2. IF any condition fails → BLOCK with:
```
SCALING BLOCKED
Reason: {specific condition} = {actual value}, required = {threshold}
Required action: {specific remediation}
```

8.3. No override mechanism exists. No verbal authorization bypasses this gate.

### TASK 9: Block Execution if Governance Missing

9.1. At workflow initialization (every trigger), execute governance check:
- Verify Repo_SHA_Lock.md exists and SHA matches
- Verify CONSTITUTION.md exists
- Verify PIPELINE_COMPOSITION.md exists
- Verify DS-001 doctrine exists
- Verify TUAX_Configurable_Constants.md exists

9.2. IF any file missing → BLOCK ALL workflow execution:
```
GOVERNANCE BLOCK
Missing: {file_path}
All workflows suspended until governance is restored.
```

9.3. Operator notification via Telegram with exact missing file(s).
```

---

# SECTION 7 — CURSOR PS1 TEMPLATE UPGRADE

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

The following is a prompt for Claude Opus to generate an updated TUAX bootstrap .ps1 script:

```
# PROMPT FOR CLAUDE OPUS — TUAX BOOTSTRAP .PS1 UPGRADE

## CONTEXT

You are updating the TUAX bootstrap PowerShell script (.ps1) that initializes
new Micro OS projects. The current script creates a basic project scaffold.
The upgraded script must enforce governance-first project initialization.

## GOVERNING AUTHORITY

governing_repo: https://github.com/{owner}/TUAX-Knowledge-Vault
pinned_sha: 116b494583422d2125b92b01ea6fda5a9977db17

## REQUIREMENTS

### 1. Governance Layer Auto-Inclusion

Every new Micro OS project initialized by this script MUST:

a) Clone or pull the Knowledge Vault repository
b) Verify the repository is at the pinned SHA
c) Copy ALL governance files to the project's governance directory:
   - CONSTITUTION.md
   - PIPELINE_COMPOSITION.md
   - EXECUTION_PIPELINE.md
   - FAILURE_TAXONOMY.md
   - ENFORCEMENT_LINTER.md
   - N8N_API_MCP_BRIDGE.md
   - PROJECT_RULES.md
   - DATASET_INGESTION_FIREWALL.md
   - Repo-SHA_Governance_Law.md
   - UNIVERSAL_WORKFLOW_GOVERNANCE.md
   - UNIVERSAL_HARDENING_DOCTRINE.md
d) Create the project-specific governance files:
   - Repo_SHA_Lock.md (with pinned SHA)
   - TUAX_Configurable_Constants.md (with all thresholds)
   - TUAX_Dataset_Deprecation_Log.md (empty)
e) Verify all files were copied successfully (SHA256 hash check)

### 2. Repo SHA Pinning on Init

The script MUST:

a) Pin the SHA at initialization time
b) Write the SHA to Repo_SHA_Lock.md
c) Write the SHA to a .tuax_sha file in the project root (machine-readable)
d) Verify the SHA resolves to a valid commit (git cat-file -t {sha})
e) IF SHA verification fails → BLOCK initialization with error message

### 3. Dataset Ingestion Firewall Installation

The script MUST:

a) Copy DATASET_INGESTION_FIREWALL.md to /repo/ directory
b) Create the /datasets/ directory structure
c) Copy DS-001 doctrine artifacts (if they exist at the pinned SHA):
   - datasets/DS-001_100k_day_Creative_System/Dataset_Doctrine.md
   - datasets/DS-001_100k_day_Creative_System/Agent_Registry.md
   - datasets/DS-001_100k_day_Creative_System/Economic_Valuation.md
d) Verify all copied doctrine artifacts pass integrity checks
e) IF any doctrine artifact is missing at the SHA → WARN (not block — the project
   may need to run DIF ingestion as its first task)

### 4. Workflow Execution Gating

The script MUST:

a) Create a /workflows/ directory
b) Create a governance_check.js file that:
   - Reads Repo_SHA_Lock.md
   - Verifies all governance files exist
   - Verifies TUAX_Configurable_Constants.md exists and is non-empty
   - Returns PASS or FAIL with specific missing files
c) This governance check is designed to be called by n8n Code nodes
   before any workflow logic executes
d) No workflow may execute without this check passing

### 5. RAG Minimal Fragment Injection

The script MUST:

a) NOT include any RAG (Retrieval Augmented Generation) infrastructure
b) NOT create vector databases or embedding pipelines
c) IF RAG is needed in future, it MUST:
   - Use minimal fragment injection (smallest possible context window usage)
   - Never inject entire documents — only specific sections referenced by section ID
   - Every injected fragment MUST be traceable to a specific file and section at the pinned SHA
d) Create a .tuax_rag_policy file declaring: "RAG_MODE: MINIMAL_FRAGMENT_ONLY"

### 6. ZIP Ingestion Prohibition

The script MUST:

a) NOT include any ZIP extraction or ingestion automation
b) Create a .tuax_ingestion_policy file declaring:
   "ZIP files MUST be processed through the Dataset Ingestion Firewall.
    No automated ZIP extraction is permitted during project initialization.
    Each ZIP must be treated as an independent dataset per DIF Section 2."
c) IF a ZIP file is detected in the project directory during init →
   WARN: "ZIP file detected: {filename}. This file must be processed through
   DIF before any content can be used. See DATASET_INGESTION_FIREWALL.md."

### 7. Repo Intelligence Inheritance

The script MUST:

a) Create a /prompts/ directory with:
   - Claude_Code_System_Prompt.md (from Section 6 of this architecture)
   - A README.md explaining that all prompts derive from governance files
b) Create a /os/ directory with:
   - Cross_Dataset_Rules.md (initially empty)
   - Conflict_Resolution.md (initially: "No conflicts. Single dataset pipeline.")
   - Doctrine_Linter.md (validation rules for doctrine format)
   - Global_Agent_Index.md (initially: agents from DS-001 only)
c) Create an /audit/ directory with:
   - Execution_Log.md (empty, append-only)
   - Governance_Hardening_Report.md (template)

### SCRIPT STRUCTURE

```powershell
# TUAX Micro OS Bootstrap v2.0
# Governance-First Project Initialization
#
# Usage: .\tuax_init.ps1 -ProjectName "MyProject" -RepoPath "C:\path\to\knowledge-vault"
#
# This script creates a governance-compliant Micro OS project.
# No shortcuts. No skips. No overrides.

param(
    [Parameter(Mandatory=$true)]
    [string]$ProjectName,
    
    [Parameter(Mandatory=$true)]
    [string]$RepoPath,
    
    [string]$PinnedSHA = "116b494583422d2125b92b01ea6fda5a9977db17"
)

# Set encoding for Windows compatibility
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
$OutputEncoding = [System.Text.Encoding]::UTF8

# ... (implement all 7 requirements above)
# Each section must:
#   1. Perform the action
#   2. Verify the action succeeded
#   3. IF verification fails → Write-Error and exit with non-zero code
#   4. Log the action to a setup.log file

# Final verification: run governance_check.js equivalent in PowerShell
# and confirm all files present, all SHAs match, all directories created.
```

### ENCODING REQUIREMENTS

- All files created MUST use UTF-8 with BOM (for Windows PowerShell 5.1 compatibility)
- All file paths MUST use backslash on Windows
- All string comparisons MUST be case-insensitive (Windows filesystem)
- Spanish characters (ñ, á, é, í, ó, ú) MUST render correctly in all output files

### VALIDATION GATE

After the script completes, it MUST output:

```
✓ Project: {ProjectName}
✓ SHA: {PinnedSHA}
✓ Governance files: {count}/11 present
✓ Dataset doctrines: {count} present
✓ Constants file: PRESENT
✓ Deprecation log: PRESENT
✓ Directory structure: VALID
✓ ZIP policy: ENFORCED
✓ RAG policy: MINIMAL_FRAGMENT_ONLY

TUAX Micro OS initialized. Governance-first. Ready for execution.
```

IF any check fails, the script MUST output the specific failure and exit with code 1.
A partially initialized project is worse than no project — do not leave partial state.
```

---

# APPENDIX A — CONFIGURABLE CONSTANTS REFERENCE

All numeric thresholds used anywhere in the system. Single source of truth per UHD-CONST-001.

| Constant | Value | Unit | Category |
|----------|-------|------|----------|
| margin_floor_micro | 0.25 | ratio | Economic |
| margin_floor_macro_unlock | 0.30 | ratio | Economic |
| margin_floor_global_kill | 0.15 | ratio | Kill Switch |
| refund_ceiling_micro | 0.12 | ratio | Economic |
| refund_ceiling_macro_unlock | 0.10 | ratio | Economic |
| refund_ceiling_product_kill | 0.15 | ratio | Kill Switch |
| hit_rate_minimum_scaling | 0.10 | ratio | Scaling |
| hit_rate_target | 0.15 | ratio | Scaling |
| hit_rate_unlock | 0.15 | ratio | Unlock |
| hit_rate_critical_failure | 0.05 | ratio | Kill Switch |
| daily_budget_micro_initial | 35 | EUR | Budget |
| daily_budget_micro_max | 200 | EUR | Budget |
| max_creatives_micro | 3 | count | Testing |
| test_duration_min | 7 | days | Testing |
| test_duration_max | 14 | days | Testing |
| kill_zero_spend_hours | 72 | hours | Kill Switch |
| kill_cpa_above_be_hours | 48 | hours | Kill Switch |
| kill_negative_pl_days | 3 | days | Kill Switch |
| kill_global_margin_window | 7 | days | Kill Switch |
| learning_extraction_sla | 48 | hours | Discipline |
| iteration_ratio_minimum | 0.80 | ratio | Creative |
| iteration_launch_sla | 72 | hours | Velocity |
| dependency_top1_max | 0.30 | ratio | Risk |
| dependency_top1_critical | 0.40 | ratio | Risk |
| dependency_top1_emergency | 0.60 | ratio | Risk |
| unlock_consecutive_days | 14 | days | Unlock |
| budget_increase_max_per_day | 0.20 | ratio | Scaling |
| portfolio_increase_max_per_day | 0.15 | ratio | Scaling |
| product_max_concepts_before_kill | 9 | count | Product |
| learning_velocity_minimum | 1.0 | ratio | Discipline |
| platform_fee_rate_default | 0.035 | ratio | Economic |
| micro_os_max_duration | 90 | days | Stage |
| meeting_missed_block_threshold | 2 | weeks | Discipline |
| winner_iteration_minimum | 3 | count | DS-001 S.12 |
| winner_iteration_target | 5 | count | DS-001 S.8 |
| fatigue_hook_rate_decline | 0.20 | ratio | Macro |
| fatigue_ctr_decline | 0.15 | ratio | Macro |
| fatigue_cpa_increase | 0.25 | ratio | Macro |
| fatigue_spend_decline | 0.30 | ratio | Macro |
| portfolio_single_product_max_spend | 0.50 | ratio | Macro |
| cross_product_confirmation_threshold | 3 | count | Macro |

---

# APPENDIX B — TRACEABILITY MATRIX

Every element in this architecture traced to its source.

| Architecture Element | Source Document | Source Section |
|---------------------|----------------|----------------|
| Hit rate before volume | DS-001 Dataset Doctrine | Principle 1 |
| Intent before production | DS-001 Dataset Doctrine | Principle 2 |
| One winner can change trajectory | DS-001 Dataset Doctrine | Principle 3 |
| Learnings mandatory | DS-001 Dataset Doctrine | Principle 4 |
| 80/20 iteration law | DS-001 Dataset Doctrine | Principle 5 |
| Research feeds system | DS-001 Dataset Doctrine | Principle 6 |
| Meetings are infrastructure | DS-001 Dataset Doctrine | Principle 7 |
| Winners must be understood | DS-001 Dataset Doctrine | Principle 8 |
| Hypothesis format (IF-THEN-BECAUSE) | DS-001 Dataset Doctrine | Section 7 |
| Test duration rules (7-14 days) | DS-001 Dataset Doctrine | Section 7 |
| Winner/loser classification criteria | DS-001 Dataset Doctrine | Section 7 |
| Learning extraction rules | DS-001 Dataset Doctrine | Section 7, Section 9 |
| Iteration doctrine | DS-001 Dataset Doctrine | Section 8 |
| Dependency concentration limits | DS-001 Dataset Doctrine | Section 10 |
| Scaling readiness criteria (8 conditions) | DS-001 Dataset Doctrine | Section 12 |
| Ferrari with Fiat engine failure pattern | DS-001 Dataset Doctrine | Section 14, Failure Mode 1 |
| Ununderstood winner failure pattern | DS-001 Dataset Doctrine | Section 14, Failure Mode 2 |
| PRD requirement | CONSTITUTION.md | PRD REQUIREMENT |
| Mode separation (SPEC/COMPILATION/EXECUTION) | CONSTITUTION.md | EXECUTION MODES |
| One-shot principle | CONSTITUTION.md | ONE-SHOT PRINCIPLE |
| Block mechanism | CONSTITUTION.md | STOP/BLOCK MECHANISM |
| Auto-repair rules | CONSTITUTION.md | AUTO-REPAIR & FAILURE GOVERNANCE |
| Schema ground truth | N8N_API_MCP_BRIDGE.md | RULES |
| Dataset ingestion isolation | DATASET_INGESTION_FIREWALL.md | Section 6, Step 2 |
| No dataset summarization | DATASET_INGESTION_FIREWALL.md | Section 5, Shortcut #2 |
| Pipeline composition authority matrix | PIPELINE_COMPOSITION.md | Section 4 |
| Conflict resolution law | PIPELINE_COMPOSITION.md | Section 5 |
| .item accessor prohibition | UNIVERSAL_HARDENING_DOCTRINE.md | UHD-EXPR-001 |
| Switch conditions.options requirement | UNIVERSAL_HARDENING_DOCTRINE.md | UHD-SWITCH-001 |
| Telegram HTML parse mode | UNIVERSAL_HARDENING_DOCTRINE.md | UHD-MSG-002 |
| appendAttribution false | UNIVERSAL_HARDENING_DOCTRINE.md | UHD-BRAND-001 |
| Kill-switch activation + deactivation | UNIVERSAL_HARDENING_DOCTRINE.md | UHD-KILL-001 |
| Configurable constants single source | UNIVERSAL_HARDENING_DOCTRINE.md | UHD-CONST-001 |
| Observational systems read-only | UNIVERSAL_HARDENING_DOCTRINE.md | UHD-OBS-001 |
| Repo SHA pinning | Repo-SHA_Governance_Law.md | Section 2.2 |
| Floating references illegal | Repo-SHA_Governance_Law.md | Section 2.3 |
| Agent legality conditions | Repo-SHA_Governance_Law.md | Section 5.1 |
| Default-deny posture | Repo-SHA_Governance_Law.md | Section 6.3 |
| Hostile input posture | UNIVERSAL_WORKFLOW_GOVERNANCE.md | LAW 1.2 |
| State machines mandatory | UNIVERSAL_WORKFLOW_GOVERNANCE.md | LAW 4.1 |
| Every router must have fallback | UNIVERSAL_WORKFLOW_GOVERNANCE.md | LAW 5.2 |
| Users will break every system | UNIVERSAL_WORKFLOW_GOVERNANCE.md | LAW 6.1 |
| One-shot operational definition | UNIVERSAL_WORKFLOW_GOVERNANCE.md | LAW 7.1 |

---

*End of TUAX Two-Stage Operating System Architecture v1.0*
*Mode: ARCHITECTURE / SPEC — No execution artifacts produced*
*All elements traceable to source documents at pinned SHA*
