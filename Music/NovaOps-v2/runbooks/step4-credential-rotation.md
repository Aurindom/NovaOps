# Credential Rotation & Authentication Failures Runbook

## Scenario: CrashLoop After Credential Rotation or Secret Update

### Symptoms
- **Metrics:** CPU 0%, memory 0%, pod is not running (failed startup)
- **Kubernetes Events:** CrashLoopBackOff, Failed with exit code 1, Error: container exited
- **Logs:** Password authentication failed, connection refused to database/API, FATAL: cannot establish connection

### Diagnosis

#### Did credentials change recently?
1. **Check audit trail:** Who rotated secrets? When? (git log for secret rotations, Kubernetes secret metadata)
2. **Timing correlation:** CrashLoop started immediately after credential change?
3. **Service startup:** Reads old secret from Kubernetes secret store before application startup?

#### Root causes
- SRE rotated database password but forgot to update Kubernetes secret
- API key expired; new key not propagated to ConfigMap
- Certificate chain updated; application still references old cert path
- TLS secret rotated; application caching old cert in memory

### Remediation

#### Action: rollback_deployment
**When:** Recent deployment or secret rotation caused CrashLoop
1. Identify the bad commit (if code change) or the secret version (if secret change)
2. Rollback code to last known good version
3. OR rollback Kubernetes secret to previous version (kubectl rollout history secret/...)
4. Restart pod: Should pick up old secret and start successfully
5. Post-mortem: Validate new credential before applying (e.g., test DB connection)

#### Action: noop_require_human
**When:** CrashLoop persists after rollback
1. Escalate to on-call SRE/security engineer
2. Manual credential validation required
3. Potential new auth scheme or infrastructure dependency change
4. Requires careful, manual steps (no auto-recovery safe)

### References
- Kubernetes secret rotation: Use versioning (secret-v1, secret-v2) for safe rollback
- Startup probe: Application should retry connection on startup if secret is stale
- Credential validation: Always test new creds in staging first
