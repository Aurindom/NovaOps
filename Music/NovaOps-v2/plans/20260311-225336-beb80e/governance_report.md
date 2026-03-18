# Governance Report - 20260311-225336-beb80e

**Generated:** 2026-03-12T02:53:36.778619+00:00
**Status:** `pending_approval`

---

## Risk Assessment

| Field | Value |
|---|---|
| Risk Score | 41/100 [####......] |
| Decision | **REQUIRE_APPROVAL** |
| Policy Hit | `low_confidence_requires_approval` |
| Reason | Confidence below 65% — insufficient certainty for autonomous action |
| Severity | `P2` |
| Proposed Action | `scale_deployment` |
| Confidence | 58% (source: `convergence_disagreement`) |
| Evaluated At | 2026-03-12T02:53:36.763046+00:00 |


---

## Append-Only Audit Trail

| Timestamp (UTC) | Event | Actor | Detail |
|---|---|---|---|
| 2026-03-12 02:53:36 | `ALERT_RECEIVED` | SYSTEM | alert=Search Indexer — Zero-Shot CPU Spike on search-indexer |
| 2026-03-12 02:53:36 | `CONVERGENCE_CHECK` | SYSTEM | {"agree": false, "war_room_action": "scale_deployment",... |
| 2026-03-12 02:53:36 | `TRIAGE_COMPLETE` | SYSTEM | domain=`traffic_surge` severity=`P2` service=`search-indexer` |
| 2026-03-12 02:53:36 | `HYPOTHESIS_FORMED` | SYSTEM | confidence=88% - Checkout is under elevated demand and should scale horizontally. |
| 2026-03-12 02:53:36 | `CRITIC_VERDICT` | SYSTEM | verdict=`PASS` confidence=88% |
| 2026-03-12 02:53:36 | `GOVERNANCE_DECISION` | SYSTEM | decision=**REQUIRE_APPROVAL** policy=`low_confidence_requires_approval` risk=41/100 |

---

## Policy Evaluation

Policies are evaluated in priority order from `governance/policies/default.yaml`.
The first matching policy determines the decision.

**Risk score formula:**
- Action weight: rollback=40, restart=20, scale=15, noop=0
- Severity weight: P1=30, P2=20, P3=10, P4=5
- Confidence penalty: `max(0, (0.75 - confidence) * 40)` for low confidence

> This audit trail is append-only. Entries are written once and never modified.
