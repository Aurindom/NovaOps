# Governance Report - 20260311-231807-b64f99

**Generated:** 2026-03-12T03:18:07.171544+00:00
**Status:** `pending_approval`

---

## Risk Assessment

| Field | Value |
|---|---|
| Risk Score | 67/100 [#######...] |
| Decision | **REQUIRE_APPROVAL** |
| Policy Hit | `rollback_always_requires_approval` |
| Reason | rollback_deployment is irreversible — always require human approval |
| Severity | `P2` |
| Proposed Action | `rollback_deployment` |
| Confidence | 55% (source: `convergence_disagreement`) |
| Evaluated At | 2026-03-12T03:18:07.148823+00:00 |


---

## Append-Only Audit Trail

| Timestamp (UTC) | Event | Actor | Detail |
|---|---|---|---|
| 2026-03-12 03:18:07 | `ALERT_RECEIVED` | SYSTEM | alert=Inventory DB — Bad Credential Rotation on inventory-db |
| 2026-03-12 03:18:07 | `CONVERGENCE_CHECK` | SYSTEM | {"agree": false, "war_room_action": "rollback_deployment",... |
| 2026-03-12 03:18:07 | `TRIAGE_COMPLETE` | SYSTEM | domain=`config_drift` severity=`P2` service=`inventory-db` |
| 2026-03-12 03:18:07 | `HYPOTHESIS_FORMED` | SYSTEM | confidence=85% - Configuration error requires deployment rollback to restore service... |
| 2026-03-12 03:18:07 | `CRITIC_VERDICT` | SYSTEM | verdict=`PASS` confidence=85% |
| 2026-03-12 03:18:07 | `GOVERNANCE_DECISION` | SYSTEM | decision=**REQUIRE_APPROVAL** policy=`rollback_always_requires_approval` risk=67/100 |

---

## Policy Evaluation

Policies are evaluated in priority order from `governance/policies/default.yaml`.
The first matching policy determines the decision.

**Risk score formula:**
- Action weight: rollback=40, restart=20, scale=15, noop=0
- Severity weight: P1=30, P2=20, P3=10, P4=5
- Confidence penalty: `max(0, (0.75 - confidence) * 40)` for low confidence

> This audit trail is append-only. Entries are written once and never modified.
