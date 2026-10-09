# Lira Agent v3 — public engineering roadmap

Updated: 2026-10-09

Lira Agent v3 is a new Windows security-agent architecture for Windows Server
and Windows 10/11. It is being built from operational experience with the
previous agent generation, with failure isolation and recoverability treated as
release requirements rather than optional improvements.

Agent v3 is currently an engineering preview. It is not generally available,
and no production-readiness claim is made by this document.

## Design goals

- Separate supervision, event collection, transport, local protection, update
  handling and MFA so that a stalled integration cannot silently stop the
  whole agent.
- Keep local security controls operating when the Lira control plane is
  unavailable.
- Bind every enrolled installation to its own device identity and reject
  replayed or out-of-order control messages.
- Limit remote administration to versioned, narrowly defined operations. The
  portal does not provide arbitrary shell, PowerShell or upload-and-execute
  capability.
- Make health, degraded states and recovery actions visible without requiring
  unrestricted remote access to the endpoint.
- Preserve a tested local recovery path for RDP and MFA changes.

## Current status

| Area | Status | Public scope |
|---|---|---|
| Component boundaries and supervision | Implemented, under test | Independent component health and bounded restart behavior |
| Enrollment and device identity | Implemented, accepted on Windows 10/11 | Enrollment-bound v3 identity and epoch; non-exportable per-user CNG key |
| Heartbeat and health reporting | Implemented, under test | Strict v3 contract, component state and monotonic sequence |
| Local event storage and transport | Implemented, under test | Durable local state and retry without losing acknowledged position |
| Local RDP attack blocking | Under test | Per-server policy, allowlist and expiring local decisions |
| Signed policy and restricted operations | Under test | Versioned allowlist; no generic remote command execution |
| Signed updater and rollback | Under test | Serialized update path, integrity checks and staged rollout |
| Windows Server installation | Under test | MSI lifecycle and recovery testing across supported Server releases |
| Windows 10/11 endpoint installation | Under test | Separate endpoint profile and MSI identity |
| Windows logon MFA | Safety gate implemented; acceptance pending | Activation remains locked until signed provisioning and the logon/recovery matrix pass |
| Lira Access enrollment | Implemented, closed preview | Browser PKCE, device invitation, administrator approval/revocation and dual-key rotation; feature remains disabled by default |
| Private access client and tunnel | In development | Authorized resource access without exposing public RDP; no general-availability claim |

“Implemented, under test” means code and automated checks exist, but the area
has not completed the full release acceptance matrix. It does not mean general
availability.

## Release stages

1. Laboratory validation on supported Windows Server and Windows client VMs.
2. Controlled pilot with explicit recovery access and no automatic fleet-wide
   activation.
3. Limited production rollout with health gates and rollback criteria.
4. General availability only after installer, upgrade, rollback, load, outage,
   RDP and MFA acceptance tests pass.

## Security boundaries

- Windows credentials are not sent through the Lira portal.
- Endpoint private keys are generated and retained on the endpoint.
- Standard-user acceptance on Windows 10 and Windows 11 verifies that the Lira
  Access ECDSA P-256 key is non-exportable and its protected ACL contains only
  SYSTEM and the owning Windows user.
- The control plane cannot silently expand the local security ceiling, replace
  trust anchors or disable MFA through an ordinary policy update.
- MFA activation is fail-safe while v3 provisioning and recovery validation are
  incomplete.
- Existing endpoints are not automatically migrated to Agent v3 merely because
  a preview build exists.

## What is not being announced

- No public Agent v3 download is available yet.
- Lira Access remains a closed technical preview. Its end-to-end client and
  tunnel path is not being announced as production-ready.
- No final release date or compatibility guarantee is being promised yet.
- Passing an individual automated test or VM scenario is not equivalent to a
  supported production release.

Security issues should be reported privately through the security contact
published by Vyvick. Do not include credentials, enrollment tokens, private
keys or customer data in public issues.
