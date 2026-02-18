# TM-AdsOS CONFIGURABLE CONSTANTS

**Version:** 1.0
**Date:** 2026-02-11
**Authority:** UHD-CONST-001

---

## Unit Economics Constants

| Constant Name | Value | Unit | Referenced By | Last Updated |
|---------------|-------|------|---------------|-------------|
| safety_buffer_multiplier | 1.3 | ratio | UNIT_ECONOMICS_LAW.md §2.3 | 2026-02-11 |
| margin_allocation_to_cpa | 0.70 | ratio | UNIT_ECONOMICS_LAW.md §2.4 | 2026-02-11 |
| minimum_margin_percent | 0.20 | ratio | UNIT_ECONOMICS_LAW.md §3 GATE 1 | 2026-02-11 |
| budget_test_cycle_max_percent | 0.50 | ratio | UNIT_ECONOMICS_LAW.md §3 GATE 3 | 2026-02-11 |

## Testing Constants

| Constant Name | Value | Unit | Referenced By | Last Updated |
|---------------|-------|------|---------------|-------------|
| minimum_test_days | 7 | days | DS-001 §7, AGENT_REGISTRY AGENT-003 | 2026-02-11 |
| standard_test_days | 14 | days | DS-001 §7 | 2026-02-11 |
| minimum_concepts_per_cycle | 3 | count | UNIT_ECONOMICS_LAW.md §3 GATE 3 | 2026-02-11 |
| creative_briefs_per_session | 3 | count | AGENT_REGISTRY AGENT-002 | 2026-02-11 |
| iteration_ratio_existing | 0.80 | ratio | DS-001 Principle 5 | 2026-02-11 |
| iteration_ratio_new | 0.20 | ratio | DS-001 Principle 5 | 2026-02-11 |

## Scaling & Velocity Constants

| Constant Name | Value | Unit | Referenced By | Last Updated |
|---------------|-------|------|---------------|-------------|
| minimum_hit_rate_for_scaling | 0.10 | ratio | DS-001 Principle 1, VELOCITY_LAW.md §2 | 2026-02-11 |
| target_hit_rate | 0.15 | ratio | DS-001 §4 | 2026-02-11 |
| critical_hit_rate | 0.05 | ratio | DS-001 §4 | 2026-02-11 |
| max_weekly_budget_percent | 0.25 | ratio | VELOCITY_LAW.md §4.1 | 2026-02-11 |
| max_budget_increase_per_week | 0.30 | ratio | VELOCITY_LAW.md §4.2 | 2026-02-11 |
| consecutive_profitable_weeks_for_scale | 2 | weeks | VELOCITY_LAW.md §4.2 | 2026-02-11 |

## Dependency Concentration Constants

| Constant Name | Value | Unit | Referenced By | Last Updated |
|---------------|-------|------|---------------|-------------|
| single_ad_warning_threshold | 0.30 | ratio | DS-001 §10, VELOCITY_LAW.md §3 | 2026-02-11 |
| single_ad_critical_threshold | 0.40 | ratio | DS-001 §10, VELOCITY_LAW.md §3 | 2026-02-11 |
| single_ad_escalation_threshold | 0.60 | ratio | DS-001 §10, VELOCITY_LAW.md §3 | 2026-02-11 |
| single_angle_critical_threshold | 0.70 | ratio | DS-001 §10, VELOCITY_LAW.md §3 | 2026-02-11 |
| single_format_critical_threshold | 0.80 | ratio | DS-001 §10, VELOCITY_LAW.md §3 | 2026-02-11 |
| iteration_sprint_variants | 5 | count | DS-001 §8 | 2026-02-11 |
| iteration_sprint_deadline_days | 7 | days | DS-001 §8 | 2026-02-11 |

## System Constants

| Constant Name | Value | Unit | Referenced By | Last Updated |
|---------------|-------|------|---------------|-------------|
| max_telegram_message_length | 4096 | characters | Telegram Bot API | 2026-02-11 |
| session_timeout_hours | 24 | hours | ARCHITECTURE_SPEC.md | 2026-02-11 |
| max_response_time_seconds | 120 | seconds | PRD.md §12 | 2026-02-11 |
| min_iterations_per_winner | 3 | count | DS-001 §4 | 2026-02-11 |
| max_iterations_before_exhaustion | 20 | count | DS-001 §4 | 2026-02-11 |

---

*End of TM_AdsOS_Configurable_Constants.md v1.0*
