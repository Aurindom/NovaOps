# Zero-Shot Incident Mitigation Runbook

## Scenario: Unknown Failure Mode (CPU Spike, Timeout, No Runbook)

### Symptoms
- **High CPU:** 95%+ utilization, threads saturated
- **Liveness probe:** Timeouts (504 Gateway Timeout, context deadline exceeded)
- **Logs:** ElasticsearchTimeoutException, indexing queue backlog, workers saturated
- **No matching runbook:** Issue doesn't fit OOM, deadlock, or traffic surge patterns

### Diagnosis

#### Unknown failure mode indicators
1. **Anomaly:** Behavior outside normal operating parameters
2. **No clear root cause:** Logs mention external system (Elasticsearch, queue, index), not application error
3. **Resource pressure:** CPU high, but memory and disk okay
4. **New service or rare scenario:** Indexer/batch processor unusual patterns

#### Possible causes
- Elasticsearch cluster degraded (slow queries, high GC)
- Upstream job queue producer flooding with work (backlog building)
- Resource contention from neighbor pods (noisy neighbor)
- Bulk operation hitting upstream limit (30s timeout on bulk index)
- Worker pool too small for actual workload

### Remediation

#### Action: scale_deployment (best guess)
**When:** Resource pressure + unknown root cause
1. Assume workload exceeded capacity (safest assumption)
2. Scale replicas: +50% more pods OR increase CPU request by 2x
3. Monitor for 60-120s: Does CPU pressure ease?
4. If yes: Keep scaled, investigate why baseline is insufficient
5. If no: Revert scale, escalate to human investigation

#### Action: noop_require_human (escalation)
**When:** Scaling doesn't help or infrastructure is at limit
1. Unknown failure mode with insufficient context
2. Requires domain expertise: Is this Elasticsearch degradation? Job queue issue?
3. Escalate with full context: logs, metrics, timeline
4. Potential next steps: Restart problematic upstream service, adjust timeout thresholds

### References
- Unknown failure mode: Always try safe scaling first (low risk)
- Timeouts: Check if upstream service SLA or our worker efficiency
- Worker saturation: If 100% utilization persists after scale, may be application bug
