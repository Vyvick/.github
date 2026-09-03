# Lira RDP Windows Agent release notes

Verified release history for retained Lira RDP Windows agent builds. The newest retained release appears first.

[View the release notes on vyvick.com](https://vyvick.com/en/agent-release-notes.html)

## 2.7.31 — 2026-08-26

**Retained release — Safer recovery and faster allowlist reconciliation**

- Adding an address to the allowlist now removes matching local Windows Firewall restrictions immediately, including when the policy has just changed.
- If the portal is temporarily unavailable during an MFA-protected sign-in, the agent avoids leaving the server indefinitely inaccessible while retaining the saved MFA policy for automatic recovery.
- Subscription suspension and reactivation now preserve the configured MFA policy and apply the current protection state consistently.
- Enrollment, secure credential renewal and portal connectivity recovery no longer require a service restart.

## 2.7.28 — 2026-08-18

**Retained release — Safer enrollment and event processing**

- Fresh enrollment now establishes a clean starting point for security events, reducing unintended blocks from older records.
- Event processing now handles clock differences and out-of-window records more safely.
- Compatibility with Microsoft Defender exclusions for supported database workloads was improved.

## 2.7.25 — 2026-08-17

**Maintenance release — Agent maintenance build**

- The retained agent core was refreshed for staged distribution.
- No separate administrator-facing change is documented for this final build of the date.

## 2.7.24 — 2026-08-15

**Retained release — Verified release packaging**

- Installer and update packages now receive additional code-signing validation before publication.
- Published Windows packages include timestamped Authenticode signatures.

## 2.7.23 — 2026-08-13

**Retained release — Improved connectivity and update reliability**

- The agent now uses available portal connectivity more efficiently and switches automatically to a fallback when needed.
- Lengthy Windows Update work no longer causes an active agent to appear offline during servicing.

## 2.7.15 — 2026-08-12

**Retained release — Outbound RDP sign-in compatibility**

- Lira sign-in protection remains limited to supported Windows sign-in and workstation-unlock scenarios.
- Outbound RDP prompts retain standard Windows behavior while MFA remains available for protected inbound RDP access.

## 2.7.10 — 2026-08-11

**Maintenance release — Agent maintenance build**

- The agent received a maintenance refresh for staged distribution.
- No administrator action or configuration change is required for this build.

## 2.7.5 — 2026-08-10

**Maintenance release — Agent maintenance build**

- The agent received a maintenance refresh for staged distribution.
- No administrator action or configuration change is required for this build.

## 2.7.0 — 2026-08-06

**Retained release — RDP TLS and endpoint compatibility**

- Certificate and endpoint handling was improved for environments where public and internal RDP settings differ.
- Required RDP service changes are now applied in a controlled manner only when certificate or security settings change.

## 2.6.99 — 2026-07-31

**Retained release — RDP MFA reliability and protection**

- Password and one-time-code sign-in reliability was improved for protected Windows users.
- MFA challenges and recovery codes now receive stronger protection against expiry, replay and reuse.

## 2.6.92 — 2026-07-29

**Retained release — Agent status and rollout reliability**

- Agent status reporting now distinguishes normal inactivity from delayed processing and operational errors more accurately.
- Update rollout improvements preserve monitoring continuity and event catch-up during servicing.
