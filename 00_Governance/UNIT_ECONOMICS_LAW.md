# UNIT ECONOMICS LAW — TM-AdsOS

**Version:** 1.0
**Date:** 2026-02-11
**Authority:** DS-001 Section 2, 12 | CONSTITUTION.md v1.0
**Scope:** All financial viability checks within TM-AdsOS and any TUAX Micro OS performing ad spend validation.

---

## 1. Purpose

This law governs all financial calculations, viability verdicts, and budget constraints within the system. No creative production, testing, or scaling action may occur without passing the financial gates defined here.

---

## 2. Core Formulas

### 2.1 — Margin Calculation

```
gross_margin = AOV × (1 - COGS_percent)
```

### 2.2 — Breakeven ROAS

```
breakeven_roas = AOV / gross_margin
```

### 2.3 — Operational ROAS (with safety buffer)

```
operational_roas = breakeven_roas × 1.3
```

The 1.3 multiplier provides a 30% safety buffer to account for attribution decay, return rates, and platform reporting discrepancies.

### 2.4 — CPA Ceiling

```
cpa_ceiling = gross_margin × 0.7
```

70% of gross margin is allocated to customer acquisition. 30% is retained for operational costs and profit.

### 2.5 — Estimated CPA (from CPM)

```
estimated_cpa = (CPM / 1000) / (CTR × CVR)
```

Where CTR and CVR are niche/country-specific estimates.

---

## 3. Viability Gates

### GATE 1 — Minimum Margin

```
IF gross_margin < (AOV × 0.20) THEN BLOCK
```

Products with less than 20% margin CANNOT support paid acquisition. This is absolute.

### GATE 2 — CPA Feasibility

```
IF estimated_cpa_at_median_CPM > cpa_ceiling THEN BLOCK
```

If the estimated cost to acquire a customer exceeds 70% of margin at median CPM for the niche/country, the product is not viable for paid advertising.

### GATE 3 — Budget Sufficiency

```
IF monthly_ad_budget < (daily_test_budget × 7 × 3) THEN WARN
```

If the monthly budget cannot support a minimum 7-day test of at least 3 concepts simultaneously, warn that testing will be severely constrained.

---

## 4. Blocking Rules

| Condition | Verdict | Blocking Reason |
|-----------|---------|-----------------|
| Margin < 20% | BLOCK | "Product margin ({margin}%) is below the 20% minimum required for profitable paid acquisition." |
| CPA > CPA ceiling at median CPM | BLOCK | "Estimated CPA (${cpa}) exceeds maximum allowable CPA (${ceiling}) at median CPM for {country}/{niche}." |
| ROAS at estimated CPA < breakeven ROAS | BLOCK | "Estimated ROAS ({roas}x) is below breakeven ({breakeven}x). Product cannot achieve profitability through paid ads." |
| Budget < minimum test spend | WARN (not block) | "Monthly budget (${budget}) is insufficient for a standard 7-day test cycle. Consider reducing concepts per cycle." |

---

## 5. CPM Reference Table

| Region | Low CPM | Median CPM | High CPM |
|--------|---------|------------|----------|
| US | $8 | $15 | $35 |
| UK | $6 | $12 | $28 |
| EU (West) | $5 | $10 | $25 |
| EU (South — ES, IT, PT) | $3 | $7 | $18 |
| LATAM | $2 | $5 | $12 |
| AU/NZ | $7 | $14 | $30 |

These are reference ranges. Actual CPMs vary by niche, season, and competition. The Gatekeeper uses median CPM for viability assessment.

---

## 6. Constants (Referenced from TM_AdsOS_Configurable_Constants.md)

| Constant | Value | Unit |
|----------|-------|------|
| safety_buffer_multiplier | 1.3 | ratio |
| margin_allocation_to_cpa | 0.70 | ratio |
| minimum_margin_percent | 0.20 | ratio |
| minimum_test_days | 7 | days |
| minimum_concepts_per_cycle | 3 | count |
| budget_test_cycle_max_percent | 0.50 | ratio (of monthly budget) |

---

## 7. Immutability

This law may only be modified through a versioned commit with documented justification, evidence from production data, and system owner approval.

---

*End of UNIT_ECONOMICS_LAW.md v1.0*
