# Governance Report - 20260311-225332-6e8d69

**Generated:** 2026-03-12T02:53:32.625933+00:00
**Status:** `pending_approval`

---

## Risk Assessment

| Field | Value |
|---|---|
| Risk Score | 48/100 [#####.....] |
| Decision | **REQUIRE_APPROVAL** |
| Policy Hit | `low_confidence_requires_approval` |
| Reason | Confidence below 65% — insufficient certainty for autonomous action |
| Severity | `P2` |
| Proposed Action | `restart_pods` |
| Confidence | 53% (source: `convergence_disagreement`) |
| Evaluated At | 2026-03-12T02:53:32.610415+00:00 |


---

## Append-Only Audit Trail

| Timestamp (UTC) | Event | Actor | Detail |
|---|---|---|---|
| 2026-03-12 02:53:32 | `ALERT_RECEIVED` | SYSTEM | alert=Order Processor — Organic Cache Bloat on order-processor |
| 2026-03-12 02:53:32 | `CONVERGENCE_CHECK` | SYSTEM | {"agree": false, "war_room_action": "restart_pods", "jury_action":... |
| 2026-03-12 02:53:32 | `TRIAGE_COMPLETE` | SYSTEM | domain=`oom` severity=`P2` service=`order-processor` |
| 2026-03-12 02:53:32 | `HYPOTHESIS_FORMED` | SYSTEM | confidence=83% - Memory pressure indicates unstable workers that should be recycled. |
| 2026-03-12 02:53:32 | `CRITIC_VERDICT` | SYSTEM | verdict=`PASS` confidence=83% |
| 2026-03-12 02:53:32 | `GOVERNANCE_DECISION` | SYSTEM | decision=**REQUIRE_APPROVAL** policy=`low_confidence_requires_approval` risk=48/100 |

---

## Policy Evaluation

Policies are evaluated in priority order from `governance/policies/default.yaml`.
The first matching policy determines the decision.

**Risk score formula:**
- Action weight: rollback=40, restart=20, scale=15, noop=0
- Severity weight: P1=30, P2=20, P3=10, P4=5
- Confidence penalty: `max(0, (0.75 - confidence) * 40)` for low confidence

> This audit trail is append-only. Entries are written once and never modified.
