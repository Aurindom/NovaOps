# Traffic Surge & Scaling Runbook

## Scenario: Unexpected Traffic Spike (Black Friday, Flash Sale, Organic Surge)

### Symptoms
- **Metrics:** CPU at 99%+, memory stable, high requests_per_second (rps), connection pool exhausted
- **Kubernetes Events:** FailedProbes (HTTP 503/504), Unhealthy readiness checks, connection refused
- **Logs:** Connection pool exhausted, 429 Too Many Requests, request queue depth > safe limit, shedding load

### Diagnosis

#### Is it a traffic spike?
1. **Organic spike:** Flash sale, marketing campaign, viral content
   - Check API gateway metrics: RPS > 2x baseline?
   - Check CDN cache hit ratio: If cache miss rate high, traffic is real
   - Action: **SCALE DEPLOYMENT** (add replicas, increase CPU requests)

2. **Bad code deployment:** Infinite loop, cache stampede, connection leak
   - Check git log: Recent backend changes within last hour?
   - If yes: **ROLLBACK DEPLOYMENT**
   - If no: Assume organic spike → **SCALE DEPLOYMENT**

### Remediation

#### Action: scale_deployment
**When:** Confirmed organic traffic spike (no bad deployment)
1. Get current replica count and resource requests (kubectl describe deployment)
2. Double replicas OR increase CPU request (e.g., 100m → 200m per pod)
3. Monitor HPA metrics: Should auto-scale if HPA is configured
4. Check after 60s: CPU should drop to 50-70%, probe success rate > 99%
5. Plan follow-up: Add more resources to baseline if trend continues

#### Action: rollback_deployment
**When:** Recent deployment correlates with spike onset
1. Check git log for deployment changes within 1 hour
2. Previous stable commit hash
3. Rollback and monitor: CPU should normalize within 5 mins
4. Post-mortem: Code review the bad commit for inefficiency

### References
- RPS baseline: Established by load tests or historical median
- Safe queue depth: < 1000 requests pending
- Healthy probe response time: < 500ms
- Auto-scaling: HPA should trigger before manual intervention
