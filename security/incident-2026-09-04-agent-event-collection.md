# Agent event collection incident — 2026-09-04

## Summary

After the staged deployment of Windows agent 2.7.33, agents continued to send
heartbeats but stopped advancing their Windows Security event checkpoints. The
first operator report concerned agent 33. Fleet telemetry then showed the same
post-update checkpoint pattern on every inspected 2.7.33 agent.

No Security log was cleared. The durable checkpoints and Windows Security logs
remained available, and collection resumed from the saved checkpoints after the
server-side compatibility fix.

## Root cause

The no-update response from `/api/agent/update/check` serialized
`package_size` and `manifest_schema` as JSON `null`. Agent 2.7.33 deserialized
both fields into non-nullable integers. Its scheduled update check therefore
threw before `ReadNewEvents` ran. The outer loop caught and retried the error,
so heartbeat traffic continued while event collection did not.

This exposed a second design weakness: an update-control-plane exception was
allowed to interrupt the event-collection iteration.

## Detection evidence

- Agent 33 service and heartbeat were current, but checkpoint `49781` remained
  behind monitored head `69886`.
- A read-only XPath query on the server returned matching Security events after
  the checkpoint.
- An isolated build of the production `EventLogWatcher` read 100 events and
  advanced an independent probe checkpoint from `49781` to `50268`.
- A controlled SYSTEM console run captured the exact exception at
  `$.package_size` on every scheduled update check.
- Fleet review showed the same approximately 20,000-record post-update rewind
  gap on all inspected 2.7.33 agents.

## Remediation

1. The backend response contract now emits numeric zero values for
   `package_size` and `manifest_schema` whenever no update is available.
2. Agent 2.7.34 accepts nullable idle-manifest numeric fields defensively.
3. Agent 2.7.34 isolates forced, heartbeat-triggered, and scheduled update-check
   failures from Security event collection and records a Windows Event Log
   warning instead of aborting the collection iteration.
4. The canary deployment script now copies release artifacts into the running
   backend container and validates the signed manifest there before enqueueing
   an update command.

## Verification

- Focused backend regression: `24 passed`; full backend regression:
  `548 passed, 1 skipped`.
- Agent 2.7.34 managed and native builds completed successfully.
- All six Windows release artifacts passed Authenticode validation.
- Update package SHA-256:
  `9ba2dee981b5380908bb40c16bbdee9ad508d7fd3c52b0253d98485d72465fc8`.
- Update package size: `2,038,451` bytes.
- Agent 33 reported 2.7.34, remained `Running`, caught up to checkpoint=head,
  advanced its portal event timestamp, and
  completed signed read-only command 2450 successfully.
- Previously stalled 2.7.33 agents resumed automatically after the backend
  compatibility deployment; sampled agents reached checkpoint=head.

## Rollout state

Agent 33 is the sole 2.7.34 canary. The global target remains 2.7.31. Further
fleet rollout remains staged until the canary finishes catch-up and the full
regression suite completes.
