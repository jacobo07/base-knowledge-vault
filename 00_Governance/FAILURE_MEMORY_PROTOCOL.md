# FAILURE MEMORY PROTOCOL — TM-AdsOS

**Version:** 1.0
**Date:** 2026-02-11
**Authority:** DS-001 Section 9 (Learning Loop) | CONSTITUTION.md v1.0 | UNIVERSAL_HARDENING_DOCTRINE.md v1.0
**Scope:** All failure capture, classification, and learning extraction within TM-AdsOS.

---

## 1. Purpose

This protocol governs how the system captures, classifies, stores, and learns from failures — both system-level failures (workflow errors, API failures) and domain-level failures (creative strategy failures, testing failures). It operationalizes DS-001's mandatory learning loop at the system infrastructure level.

---

## 2. Failure Classes

### 2.1 — System Failures

| Class | Examples | Response |
|-------|----------|----------|
| FIXABLE_AUTOMATICALLY | Invalid JSON, expression error, data shape mismatch, missing fallback | Auto-repair (once), re-validate, notify operator |
| HUMAN_ACTION_REQUIRED | Credential expired, API down, quota exceeded, permission denied | BLOCK, notify operator with exact remediation steps |
| PRD_VIOLATION | Feature outside scope, missing PRD section, scope creep | BLOCK, require PRD revision |

### 2.2 — Domain Failures (Creative Strategy)

These are NOT system errors. They are captured from user-reported outcomes and fed back into the learning system.

| Class | Trigger | Required Action |
|-------|---------|----------------|
| GATEKEEPER_FALSE_POSITIVE | Product passed gatekeeper but failed in reality | Log: actual vs predicted economics. Adjust CPM/CTR/CVR estimates. |
| GATEKEEPER_FALSE_NEGATIVE | Product blocked by gatekeeper but would have been viable | Log: actual economics. Review margin/CPA thresholds. |
| HYPOTHESIS_INVALIDATED | Creative brief hypothesis was wrong | Log: hypothesis, actual result, diagnostic. Add to anti-pattern registry. |
| WINNER_UNEXPLAINED | A winning ad cannot be explained by the team | ESCALATE: DS-001 Principle 8 violation. Mandatory winner breakdown within 48 hours. |

---

## 3. Failure Capture Format

Every failure event MUST produce:

```
failure_id:          TM-FAIL-{NNN}
timestamp:           {ISO 8601}
session_id:          {UUID}
failure_class:       FIXABLE_AUTOMATICALLY | HUMAN_ACTION_REQUIRED | PRD_VIOLATION | DOMAIN_FAILURE
failure_type:        {specific type from Section 2}
component:           {agent name / workflow node / user flow step}
root_cause:          {specific technical or strategic cause}
repair_plan:         {deterministic steps — only if FIXABLE_AUTOMATICALLY}
human_steps:         {exact actions required — only if HUMAN_ACTION_REQUIRED}
learning_extracted:  {actionable learning — only if DOMAIN_FAILURE}
changes_applied:     true | false
validation_after:    pass | fail
notification_sent:   true | false
audit_log_appended:  true | false
```

---

## 4. Learning Extraction Rules (DS-001 Section 9 — Applied)

### 4.1 — System Learning

When the same system failure occurs 3+ times:
1. Extract the general pattern
2. Write a prevention rule
3. Add to UNIVERSAL_HARDENING_DOCTRINE.md (per UHD-EVO-001)
4. Add corresponding check to Pre-Deploy Hardening Checklist

### 4.2 — Domain Learning

When a hypothesis is invalidated:
1. Record the full failure diagnostic (per DS-001 Section 9)
2. Identify which element failed (hook/angle/format/proof/credibility)
3. Form anti-pattern rule
4. Store in session-level learning output
5. When the same anti-pattern appears 3+ times across sessions: promote to niche-level rule

### 4.3 — Promotion Protocol (per UHD-OBS-003)

An observation MAY become a system rule ONLY if:
1. Confirmed across ≥ 3 analysis cycles
2. Human operator reviews and approves
3. Written into versioned doctrine file
4. NO automated promotion — ever

---

## 5. Failure Log Storage

All failure events are stored in Supabase `failure_log` table:

| Field | Type | Purpose |
|-------|------|---------|
| failure_id | TEXT | Unique identifier (TM-FAIL-NNN) |
| session_id | UUID | Associated session |
| timestamp | TIMESTAMP | When failure occurred |
| failure_class | TEXT | Classification |
| failure_type | TEXT | Specific type |
| component | TEXT | Failing component |
| root_cause | TEXT | Technical/strategic cause |
| resolution | TEXT | What was done |
| learning | TEXT | Extracted learning (if applicable) |
| promoted | BOOLEAN DEFAULT FALSE | Whether learning was promoted to rule |

Logs are APPEND-ONLY. No modification or deletion permitted.

---

*End of FAILURE_MEMORY_PROTOCOL.md v1.0*
