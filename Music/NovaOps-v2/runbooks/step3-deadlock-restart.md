# Deadlock & Thread Contention Runbook

## Scenario: Thread Deadlock or Race Condition

### Symptoms
- **Metrics:** CPU near 0% (< 5%), memory stable, but service is unresponsive
- **Kubernetes Events:** Readiness probe failed (context deadline exceeded), liveness probe timeout
- **Logs:** Deadlock detected, thread waiting to acquire lock, race condition warning, all requests blocked

### Diagnosis

#### Is it a deadlock?
1. **Deadlock indicators:**
   - Multiple threads waiting on same lock (ThreadA waits for ThreadB, ThreadB waits for ThreadA)
   - CPU is near 0% (threads are blocked, not spinning)
   - Readiness probe times out (blocked on /healthz endpoint)
   - Reoccurrence: Not random; reproducible under load

2. **Root causes:**
   - Recent code change introducing improper lock ordering
   - Third-party library deadlock (e.g., OAuth token refresh)
   - Stale transaction lock in database client

### Remediation

#### Action: restart_pods
**When:** Confirmed deadlock with no recent deployment
1. Immediate action: Rolling restart (kill + reschedule pods)
2. Expect: ~10-30 second service unavailability during restart
3. Post-restart: Monitor thread activity logs for recurrence
4. If recurs within 5 mins: Escalate to **noop_require_human** for code review

#### Action: rollback_deployment
**When:** Recent code change introduced deadlock
1. Check git log: Changes to locking, transaction handling, or concurrency
2. Deadlock likely deterministic (happens on every restart if code-based)
3. Rollback to last known good version
4. If rollback fixes: Code review the bad commit

### References
- Deadlock vs Livelock: Deadlock = completely blocked; Livelock = spinning/retrying endlessly
- Safe restart window: Identify max safe downtime for service SLA
- Thread dump: jstack or similar to inspect lock graph on next occurrence
