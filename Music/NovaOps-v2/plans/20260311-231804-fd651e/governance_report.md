# Governance Report - 20260311-231804-fd651e

**Generated:** 2026-03-12T03:18:05.052106+00:00
**Status:** `pending_approval`

---

## Risk Assessment

| Field | Value |
|---|---|
| Risk Score | 49/100 [#####.....] |
| Decision | **REQUIRE_APPROVAL** |
| Policy Hit | `low_confidence_requires_approval` |
| Reason | Confidence below 65% — insufficient certainty for autonomous action |
| Severity | `P2` |
| Proposed Action | `restart_pods` |
| Confidence | 52% (source: `convergence_disagreement`) |
| Evaluated At | 2026-03-12T03:18:05.024211+00:00 |


---

## Append-Only Audit Trail

| Timestamp (UTC) | Event | Actor | Detail |
|---|---|---|---|
| 2026-03-12 03:18:04 | `ALERT_RECEIVED` | SYSTEM | alert=Auth Service — OAuth Thread Deadlock on auth-service |
| 2026-03-12 03:18:05 | `CONVERGENCE_CHECK` | SYSTEM | {"agree": false, "war_room_action": "restart_pods", "jury_action":... |
| 2026-03-12 03:18:05 | `TRIAGE_COMPLETE` | SYSTEM | domain=`deadlock` severity=`P2` service=`auth-service` |
| 2026-03-12 03:18:05 | `HYPOTHESIS_FORMED` | SYSTEM | confidence=82% - Thread deadlock requires pod restart to recover execution flow. |
| 2026-03-12 03:18:05 | `CRITIC_VERDICT` | SYSTEM | verdict=`PASS` confidence=82% |
| 2026-03-12 03:18:05 | `GOVERNANCE_DECISION` | SYSTEM | decision=**REQUIRE_APPROVAL** policy=`low_confidence_requires_approval` risk=49/100 |

---

## Policy Evaluation

Policies are evaluated in priority order from `governance/policies/default.yaml`.
The first matching policy determines the decision.

**Risk score formula:**
- Action weight: rollback=40, restart=20, scale=15, noop=0
- Severity weight: P1=30, P2=20, P3=10, P4=5
- Confidence penalty: `max(0, (0.75 - confidence) * 40)` for low confidence

> This audit trail is append-only. Entries are written once and never modified.
