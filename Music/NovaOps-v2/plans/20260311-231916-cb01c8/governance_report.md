# Governance Report - 20260311-231916-cb01c8

**Generated:** 2026-03-12T03:19:16.444053+00:00
**Status:** `pending_approval`

---

## Risk Assessment

| Field | Value |
|---|---|
| Risk Score | 44/100 [####......] |
| Decision | **REQUIRE_APPROVAL** |
| Policy Hit | `noop_requires_approval` |
| Reason | No-op actions indicate insufficient confidence and should stay human-gated |
| Severity | `P2` |
| Proposed Action | `noop_require_human` |
| Confidence | 15% (source: `convergence_disagreement`) |
| Evaluated At | 2026-03-12T03:19:16.417187+00:00 |


---

## Append-Only Audit Trail

| Timestamp (UTC) | Event | Actor | Detail |
|---|---|---|---|
| 2026-03-12 03:19:16 | `ALERT_RECEIVED` | SYSTEM | alert=Notification Service — Twilio SMS Outage on notification-service |
| 2026-03-12 03:19:16 | `CONVERGENCE_CHECK` | SYSTEM | {"agree": false, "war_room_action": "noop_require_human",... |
| 2026-03-12 03:19:16 | `TRIAGE_COMPLETE` | SYSTEM | domain=`unknown` severity=`P2` service=`notification-service` |
| 2026-03-12 03:19:16 | `HYPOTHESIS_FORMED` | SYSTEM | confidence=45% - Signal does not match a known domain with enough confidence. |
| 2026-03-12 03:19:16 | `CRITIC_VERDICT` | SYSTEM | verdict=`FAIL` confidence=45% |
| 2026-03-12 03:19:16 | `GOVERNANCE_DECISION` | SYSTEM | decision=**REQUIRE_APPROVAL** policy=`noop_requires_approval` risk=44/100 |

---

## Policy Evaluation

Policies are evaluated in priority order from `governance/policies/default.yaml`.
The first matching policy determines the decision.

**Risk score formula:**
- Action weight: rollback=40, restart=20, scale=15, noop=0
- Severity weight: P1=30, P2=20, P3=10, P4=5
- Confidence penalty: `max(0, (0.75 - confidence) * 40)` for low confidence

> This audit trail is append-only. Entries are written once and never modified.
