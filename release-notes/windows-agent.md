# Lira RDP Windows Agent release notes

Verified history for Lira RDP Windows agent builds. The archive separates complete build coverage from detailed customer-facing notes so missing descriptions are not invented.

[View the release notes on vyvick.com](https://vyvick.com/en/agent-release-notes.html)

## Complete verified build history

The repository records 44 production agent builds from 2.6.75 through 2.7.31. Test-only packages are excluded.

| Version | Date | Record |
| --- | --- | --- |
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
