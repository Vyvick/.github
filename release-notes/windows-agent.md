# Lira RDP Windows Agent release notes

Verified history for Lira RDP Windows agent builds. The archive separates complete build coverage from detailed customer-facing notes so missing descriptions are not invented.

[View the release notes on vyvick.com](https://vyvick.com/en/agent-release-notes.html)

## Complete verified build history

The repository records 47 production agent builds from 2.6.75 through 2.7.34. Test-only packages are excluded.

| Version | Date | Record |
| --- | --- | --- |
| 2.7.34 | 2026-09-04 | Event-collection compatibility and fault-isolation hotfix. |
| 2.7.33 | 2026-09-03 | Detailed release notes below. |
| 2.7.32 | 2026-09-03 | Detailed release notes below. |
| 2.7.31 | 2026-08-26 | Detailed release notes below. |
| 2.7.30 | 2026-08-26 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.29 | 2026-08-26 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.28 | 2026-08-18 | Detailed release notes below. |
| 2.7.27 | 2026-08-18 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.26 | 2026-08-18 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.25 | 2026-08-17 | Detailed release notes below. |
| 2.7.24 | 2026-08-15 | Detailed release notes below. |
| 2.7.23 | 2026-08-13 | Detailed release notes below. |
| 2.7.22 | 2026-08-13 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.21 | 2026-08-13 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.20 | 2026-08-13 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.19 | 2026-08-13 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.18 | 2026-08-13 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.17 | 2026-08-13 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.16 | 2026-08-13 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.15 | 2026-08-12 | Detailed release notes below. |
| 2.7.14 | 2026-08-12 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.13 | 2026-08-12 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.12 | 2026-08-12 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.11 | 2026-08-12 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.10 | 2026-08-11 | Detailed release notes below. |
| 2.7.9 | 2026-08-11 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.8 | 2026-08-11 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.7 | 2026-08-11 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.6 | 2026-08-11 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.5 | 2026-08-10 | Detailed release notes below. |
| 2.7.4 | 2026-08-10 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.3 | 2026-08-10 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.2 | 2026-08-10 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.1 | 2026-08-10 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.7.0 | 2026-08-06 | Detailed release notes below. |
| 2.6.100 | 2026-08-01 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.6.99 | 2026-07-31 | Detailed release notes below. |
| 2.6.98 | 2026-07-31 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.6.97 | 2026-07-31 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.6.96 | 2026-07-31 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.6.95 | 2026-07-31 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.6.94 | 2026-07-30 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.6.93 | 2026-07-30 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.6.92 | 2026-07-29 | Detailed release notes below. |
| 2.6.91 | 2026-07-28 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.6.89 | 2026-07-26 | Packaged build verified in the retained artifact archive; no standalone public change summary was retained. |
| 2.6.75 | 2026-07-23 | Recorded deployed build; the performance baseline is retained, but the release artifact and standalone change summary are not. |

## Detailed release notes

The entries below are the releases for which a verified customer-facing change summary is retained. The newest release appears first.

### 2.7.34 — 2026-09-04

**Reliability hotfix — Event collection is independent of update checks**

- Idle update-manifest numeric fields are nullable-compatible in the agent.
- Forced, heartbeat-triggered, and scheduled update-check failures are contained and logged; they can no longer abort the Security event-collection iteration.
- The canary deployment path preflights the signed manifest inside the running backend container before enqueueing an update.
- Update package SHA-256: `9ba2dee981b5380908bb40c16bbdee9ad508d7fd3c52b0253d98485d72465fc8`; exact size: `2,038,451` bytes.
- All six Windows artifacts passed Authenticode validation. Agent 33 reached checkpoint=head and completed signed read-only command 2450. Full backend regression: 548 passed, one skipped.
- [Read the incident engineering record](../security/incident-2026-09-04-agent-event-collection.md).

### 2.7.33 — 2026-09-03

**Maintenance release — Reliable updater failure reporting**

- Every updater exit path now records the current target version and exact failure.
- Stale updater results from an earlier target are ignored so they cannot be reported as the outcome of a newer update.
- The signed package SHA-256 is `945b3e71ca582ba24fc94d5da494baa9ac98495a26fcbec251979ffc6eba88cd`; its exact size is `2,037,756` bytes.
- The RSA-PSS manifest and all six Authenticode release artifacts verified. Controlled Windows Server 2016 and 2022 canaries installed the build and completed a signed read-only diagnostics command.

### 2.7.32 — 2026-09-03

**Security release — Stronger update, command, identity, and MFA safeguards**

- Updates require an RSA-PSS/SHA-256 signed manifest, exact hash and size checks, HTTPS same-origin delivery, monotonic version progression, and Authenticode validation under the pinned Lira publisher.
- Remote commands are bound to the individual agent and command content, signed with an expiry, checked against a local allowlist, and protected by a durable replay ledger across channel fallback and service restart.
- HTTP and WebSocket authentication no longer accept the legacy shared agent token. Installer enrollment uses a short-lived credential and no preselected durable identity.
- Administrators can select compatibility `fail_open` or strict `fail_closed` behavior after testing independent recovery. In the protected fail-closed path, the Credential Provider Filter does not fall through to another password-only provider.
- MFA identities preserve Windows domain or UPN qualification so equally named accounts in different domains remain separate.
- [Read the complete security engineering report](../security/engineering-2026-09-03.md).

### 2.7.31 — 2026-08-26

**Retained release — Safer recovery and faster allowlist reconciliation**

- Adding an address to the allowlist now removes matching local Windows Firewall restrictions immediately, including when the policy has just changed.
- If the portal is temporarily unavailable during an MFA-protected sign-in, the agent avoids leaving the server indefinitely inaccessible while retaining the saved MFA policy for automatic recovery.
- Subscription suspension and reactivation now preserve the configured MFA policy and apply the current protection state consistently.
- Enrollment, secure credential renewal and portal connectivity recovery no longer require a service restart.

### 2.7.28 — 2026-08-18

**Retained release — Safer enrollment and event processing**

- Fresh enrollment now establishes a clean starting point for security events, reducing unintended blocks from older records.
- Event processing now handles clock differences and out-of-window records more safely.
- Compatibility with Microsoft Defender exclusions for supported database workloads was improved.

### 2.7.25 — 2026-08-17

**Maintenance release — Agent maintenance build**

- The retained agent core was refreshed for staged distribution.
- No separate administrator-facing change is documented for this final build of the date.

### 2.7.24 — 2026-08-15

**Retained release — Verified release packaging**

- Installer and update packages now receive additional code-signing validation before publication.
- Published Windows packages include timestamped Authenticode signatures.

### 2.7.23 — 2026-08-13

**Retained release — Improved connectivity and update reliability**

- The agent now uses available portal connectivity more efficiently and switches automatically to a fallback when needed.
- Lengthy Windows Update work no longer causes an active agent to appear offline during servicing.

### 2.7.15 — 2026-08-12

**Retained release — Outbound RDP sign-in compatibility**

- Lira sign-in protection remains limited to supported Windows sign-in and workstation-unlock scenarios.
- Outbound RDP prompts retain standard Windows behavior while MFA remains available for protected inbound RDP access.

### 2.7.10 — 2026-08-11

**Maintenance release — Agent maintenance build**

- The agent received a maintenance refresh for staged distribution.
- No administrator action or configuration change is required for this build.

### 2.7.5 — 2026-08-10

**Maintenance release — Agent maintenance build**

- The agent received a maintenance refresh for staged distribution.
- No administrator action or configuration change is required for this build.

### 2.7.0 — 2026-08-06

**Retained release — RDP TLS and endpoint compatibility**

- Certificate and endpoint handling was improved for environments where public and internal RDP settings differ.
- Required RDP service changes are now applied in a controlled manner only when certificate or security settings change.

### 2.6.99 — 2026-07-31

**Retained release — RDP MFA reliability and protection**

- Password and one-time-code sign-in reliability was improved for protected Windows users.
- MFA challenges and recovery codes now receive stronger protection against expiry, replay and reuse.

### 2.6.92 — 2026-07-29

**Retained release — Agent status and rollout reliability**

- Agent status reporting now distinguishes normal inactivity from delayed processing and operational errors more accurately.
- Update rollout improvements preserve monitoring continuity and event catch-up during servicing.
