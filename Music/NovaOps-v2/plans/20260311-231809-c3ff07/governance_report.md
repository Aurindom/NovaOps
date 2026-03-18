# Governance Report - 20260311-231809-c3ff07

**Generated:** 2026-03-12T03:18:09.299931+00:00
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
| Evaluated At | 2026-03-12T03:18:09.273340+00:00 |


---

## Append-Only Audit Trail

| Timestamp (UTC) | Event | Actor | Detail |
|---|---|---|---|
| 2026-03-12 03:18:09 | `ALERT_RECEIVED` | SYSTEM | alert=Order Processor — Organic Cache Bloat on order-processor |
| 2026-03-12 03:18:09 | `CONVERGENCE_CHECK` | SYSTEM | {"agree": false, "war_room_action": "restart_pods", "jury_action":... |
| 2026-03-12 03:18:09 | `TRIAGE_COMPLETE` | SYSTEM | domain=`oom` severity=`P2` service=`order-processor` |
| 2026-03-12 03:18:09 | `HYPOTHESIS_FORMED` | SYSTEM | confidence=83% - Memory pressure indicates unstable workers that should be recycled. |
| 2026-03-12 03:18:09 | `CRITIC_VERDICT` | SYSTEM | verdict=`PASS` confidence=83% |
| 2026-03-12 03:18:09 | `GOVERNANCE_DECISION` | SYSTEM | decision=**REQUIRE_APPROVAL** policy=`low_confidence_requires_approval` risk=48/100 |

---

## Policy Evaluation

Policies are evaluated in priority order from `governance/policies/default.yaml`.
The first matching policy determines the decision.

**Risk score formula:**
- Action weight: rollback=40, restart=20, scale=15, noop=0
- Severity weight: P1=30, P2=20, P3=10, P4=5
- Confidence penalty: `max(0, (0.75 - confidence) * 40)` for low confidence

> This audit trail is append-only. Entries are written once and never modified.
