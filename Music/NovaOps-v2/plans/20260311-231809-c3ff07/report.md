# Investigation Report

**Incident ID:** 20260311-231809-c3ff07
**Domain:** oom
**Alert:** Order Processor — Organic Cache Bloat on order-processor
**Completed:** 2026-03-11T23:18:09.205521

## Result
### Triage
- Domain: `oom`
- Severity: `P2`
- Service: `order-processor`
- Summary: Memory pressure indicates unstable workers that should be recycled.

### Root Cause
- Top hypothesis: Memory pressure indicates unstable workers that should be recycled.
- Confidence: 83%
- Recommended action: `restart_pods`
- Reasoning: Memory pressure indicates unstable workers that should be recycled.

### Critic Verdict
- Verdict: `PASS`
- Confidence: 83%
- Feedback: OOM evidence is clear and restarting pods is a safe first action.

### Proposed Remediation
- Action: `restart_pods`
- Justification: Memory pressure indicates unstable workers that should be recycled.
- Parameters: service_name=order-processor, namespace=default
- Verify: Verify latency, error rate, and pod health after remediation.

## Schema Validation
- Score: 100%
- Valid nodes: 8/8
- Invalid nodes: none


## Artifacts
- findings/triage.json
- findings/log_analyst.json
- findings/metrics_analyst.json
- findings/k8s_inspector.json
- findings/github_analyst.json
- hypotheses.md
- findings/critic.json
- findings/remediation.json
- structured.json
- validation.json
