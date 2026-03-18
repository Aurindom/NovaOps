# Governance Report - 20260311-230959-a645b5

**Generated:** 2026-03-12T03:09:59.579973+00:00
**Status:** `pending_approval`

---

## Risk Assessment

| Field | Value |
|---|---|
| Risk Score | 70/100 [#######...] |
| Decision | **REQUIRE_APPROVAL** |
| Policy Hit | `rollback_always_requires_approval` |
| Reason | rollback_deployment is irreversible — always require human approval |
| Severity | `P2` |
| Proposed Action | `rollback_deployment` |
| Confidence | 50% (source: `convergence_disagreement`) |
| Evaluated At | 2026-03-12T03:09:59.564771+00:00 |


---

## Append-Only Audit Trail

| Timestamp (UTC) | Event | Actor | Detail |
|---|---|---|---|
| 2026-03-12 03:09:59 | `ALERT_RECEIVED` | SYSTEM | alert=Redis Cache OOM — Bad Deployment on redis-cache |
| 2026-03-12 03:09:59 | `CONVERGENCE_CHECK` | SYSTEM | {"agree": false, "war_room_action": "rollback_deployment",... |
| 2026-03-12 03:09:59 | `TRIAGE_COMPLETE` | SYSTEM | domain=`oom` severity=`P2` service=`redis-cache` |
| 2026-03-12 03:09:59 | `HYPOTHESIS_FORMED` | SYSTEM | confidence=80% - Memory pressure indicates unstable workers that should be recycled. |
| 2026-03-12 03:09:59 | `CRITIC_VERDICT` | SYSTEM | verdict=`PASS` confidence=80% |
| 2026-03-12 03:09:59 | `GOVERNANCE_DECISION` | SYSTEM | decision=**REQUIRE_APPROVAL** policy=`rollback_always_requires_approval` risk=70/100 |

---

## Policy Evaluation

Policies are evaluated in priority order from `governance/policies/default.yaml`.
The first matching policy determines the decision.

**Risk score formula:**
- Action weight: rollback=40, restart=20, scale=15, noop=0
- Severity weight: P1=30, P2=20, P3=10, P4=5
- Confidence penalty: `max(0, (0.75 - confidence) * 40)` for low confidence

> This audit trail is append-only. Entries are written once and never modified.
