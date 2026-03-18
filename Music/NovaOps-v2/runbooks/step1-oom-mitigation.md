# OOM Mitigation Runbook

## Scenario: Out of Memory (OOM) Errors

### Symptoms
- **Metrics:** Memory usage exceeds container limit (memory_limit_mb)
- **Kubernetes Events:** OOMKilling warnings, pod eviction
- **Logs:** Out of memory allocating X bytes, GC overhead limit exceeded, memory-related crashes

### Diagnosis

#### Recent Deployment OOM (URGENT)
- Check git log: Recent deployment with memory-related changes?
- If developer bumped memory limits, cache size, or session store config → **ROLLBACK**
- If no memory-related changes but timing coincides → **INVESTIGATE** buffer bloat in aggregators

#### Organic Cache Bloat (GRADUAL)
- No recent deployments affecting memory
- In-memory cache (Redis, application object store) accumulated over time
- Size metrics: OrderCache.size() > safe_threshold, heap fragmentation
- Solution: **RESTART PODS** to flush heap and reset cache

### Remediation

#### Action: rollback_deployment
**When:** Recent deployment + OOM
1. Identify the bad commit (git log within last 30 mins)
2. Previous stable commit hash available
3. Roll back to last known good version
4. Monitor memory: should drop within 5 mins of rollback completion

#### Action: restart_pods
**When:** Organic cache bloat (no deployment) + high OOM frequency
1. Identify service and namespace (from metrics/k8s_events)
2. Rolling restart (podDisruptionBudget awareness)
3. Expect brief loss of in-flight requests; cache flushes on restart
4. Monitor memory post-restart: should stabilize below 70% limit

### References
- OOMKilling: Kubernetes memory overcommit detection
- Cache bloat: Months-old code without maintenance window
- Safe threshold: 80% of container memory limit
