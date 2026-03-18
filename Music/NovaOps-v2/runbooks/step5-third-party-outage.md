# Third-Party Dependency Outage Runbook

## Scenario: External Service Unavailability

### Symptoms
- **Metrics:** CPU low, memory low, service is running fine
- **Kubernetes Events:** None (infrastructure healthy)
- **Logs:** HTTP 503 from external API, timeout connecting to third-party service, retry exhausted

### Diagnosis

#### Is it a third-party outage?
1. **Check status page:** Twilio, Stripe, AWS, GitHub (check their public status dashboard)
2. **Network diagnostic:** Can we reach the external API? (ping, traceroute if allowed)
3. **Error pattern:** 503 Service Unavailable, 504 Gateway Timeout, connection refused → upstream issue
4. **Our service:** Healthy logs except for outbound calls; not a local problem

#### Root causes
- Third-party SaaS platform maintenance window
- Third-party DDoS attack or incident
- Network connectivity issue between our data center and theirs
- Rate limiting: We exceeded quota (but less likely for 503 errors)

### Remediation

#### Action: noop_require_human
**When:** Confirmed third-party outage
1. **No remediation action available** (cannot fix external service)
2. **Options for human decision:**
   - Degrade functionality: Fall back to cached data, skip optional features
   - Queue work: Hold requests until service recovers (if SLA allows)
   - Notify users: "Feature temporarily unavailable due to upstream outage"
   - Escalate: Contact third-party support if SLA breach
3. **Monitoring:** Set up alert when third-party service recovers

#### Action: scale_deployment (rare)
**When:** High queue depth and third-party outage ongoing
1. If we're queuing requests in memory, may need to scale to handle queue
2. Unlikely to improve situation but prevents our service from crashing
3. Better to degrade gracefully than crash

### References
- Third-party SaaS: Always check status page first
- Circuit breaker: Application should fast-fail on repeated 5xx errors
- Queue strategy: Decide whether to buffer or reject based on SLA
