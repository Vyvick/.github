# Lira RDP Security Engineering Report — 2026-09-03

This document records the security changes, verification evidence, rollout
state, and known limitations for the Lira RDP platform and Windows agent. It is
an internal, repository-backed engineering assessment, not an independent
penetration test or security certification.

## Executive summary

The 2026-09-03 hardening release addresses the principal questions raised in
the external review: enforcement around the Windows Credential Provider,
protection and replay resistance for privileged agent commands, the update
trust chain, Windows identity ambiguity, legacy shared credentials, TOTP secret
handling, and honest disclosure of the remaining trust boundaries.

Agent 2.7.32 introduced the security controls. Agent 2.7.33 adds deterministic
updater failure reporting and rejects stale updater results. Two staged
canaries, Windows Server 2016 and Windows Server 2022, installed 2.7.33 and then
executed a signed read-only diagnostic command. The fleet-wide target remains
2.7.31 while the signing root and the remaining live test matrix are rolled out
in controlled stages.

## Architecture and trust boundaries

The Windows agent is a privileged service. It establishes outbound HTTPS and
WebSocket connections to the Lira service; normal management does not require a
new inbound management port on the protected server. The service can apply
Windows Firewall policy, collect security events, coordinate MFA, manage RDP
TLS, perform supported Windows maintenance, and restart the host when an
authorized workflow requires it.

That capability creates an intentionally explicit trust path:

`Lira control plane -> per-agent command queue -> privileged Windows service -> Windows host`

Compromise of an authorized control-plane identity can therefore have a larger
impact than compromise of a read-only monitoring panel. Per-agent signing,
expiry, replay protection, local command allowlisting, tenant isolation, RBAC,
audit records, staged rollout, and independent recovery paths reduce this risk;
they do not make the privileged control plane untrusted or harmless.

Portal access and agent traffic use distinct application endpoints and can be
placed behind separate routing, rate-limit, and availability policies. Further
network-plane separation remains an architecture task rather than a claim of
the current release.

## Windows MFA enforcement

A Windows Credential Provider collects and serializes credentials; Windows LSA
and its authentication packages remain responsible for validating the Windows
credentials. Lira does not claim that a Credential Provider replaces LSA.

Lira installs both its Credential Provider and an `ICredentialProviderFilter`.
For protected sign-in scenarios, the filter controls whether alternative
providers may be used. Each agent has an explicit outage policy:

- `fail_open` is the compatibility default. It preserves an emergency password
  path if the local MFA service or portal is unavailable.
- `fail_closed` denies the protected sign-in when the portal times out, the
  local MFA pipe fails, or the Lira provider is missing. In this mode the filter
  does not fall through to another password-only provider.

The durable policy is applied before the first heartbeat after a service
restart. Fail-closed must be enabled only after an independent route such as a
hypervisor console, iLO/iDRAC, KVM, authenticated WinRM, or equivalent
break-glass access has been tested.

This design still requires live validation for every supported Windows logon
path. The remaining matrix includes RDP with NLA/CredSSP, console sign-in,
workstation unlock, reconnect, portal outage, local-service outage, break-glass,
and rollback. Unsupported or untested logon paths are not represented as
verified.

## TOTP secrets and authentication strength

RDP MFA currently uses TOTP and recovery codes. TOTP is materially stronger
than a password alone, but it is not phishing-resistant. FIDO2 or PIV should be
preferred where the surrounding Windows identity and access architecture can
support them. This release does not claim FIDO2/PIV support.

TOTP seeds are stored under organization-scoped secret paths when OpenBao is
configured. The encrypted database fallback binds ciphertext to the owning
organization. Recovery codes are stored as keyed digests rather than plaintext.
Access to the application encryption material remains a sensitive operational
boundary; backup, rotation, unseal, and administrator-access procedures must be
controlled accordingly.

TOTP verification rejects reuse within the accepted time window. MFA identity
matching now preserves a Windows domain or UPN so equally named accounts in
different domains remain distinct. Unqualified local users are canonicalized
with the host name. A narrowly scoped compatibility lookup remains for older
leaf-only enrollments and should be removed after those users are re-enrolled.

## Signed commands and replay resistance

Commands delivered through HTTP polling, heartbeat responses, RabbitMQ, or the
WebSocket channel use an HMAC-SHA256 envelope bound to:

- the individual agent;
- command identifier and command type;
- canonical command payload;
- creation time and expiry.

The command key is derived from the individual agent bearer and stored by the
service only under organization-scoped secret encryption. Credential rotation
also rotates the command key and pauses delivery until the agent confirms the
new bearer.

Agent 2.7.32 and later reject unsigned, altered, wrong-agent, wrong-key,
future-dated, non-expiring, and expired commands. Before execution, the agent
records the command identifier in a restrictive-ACL, bounded at-most-once
ledger. This prevents duplicate execution caused by WebSocket/polling races and
rejects replay after a service restart. Unknown command types are denied by a
local allowlist.

This is message authentication using per-agent symmetric keys, not a claim of
public-key operator signatures. It prevents cross-agent substitution and
replay, but a fully authorized control plane remains capable of creating valid
commands. mTLS, workload identity, HSM/KMS-backed signing, and more granular
operator authorization remain defense-in-depth design options, not features
claimed by this release.

## Agent authentication and tenant isolation

The legacy shared agent token is no longer accepted by HTTP or WebSocket
authentication. Production startup validation rejects attempts to enable it.
Personalized installer generation uses a short-lived enrollment token and does
not embed a fleet-wide bearer or a preselected durable agent identity.

Tenant separation is enforced in application authorization and PostgreSQL Row
Level Security. The direct-login RLS integration test verifies that tenant
isolation fails closed when executed against PostgreSQL with the intended
application-role model.

## Update trust chain

The update service requires a detached manifest matching the version, channel,
package SHA-256, and package size. Release tooling signs the canonical manifest
with RSA-PSS/SHA-256 and refuses a signing certificate whose public key does not
match the key pinned in the agent.

Before installation the agent verifies:

1. the RSA-PSS manifest signature and exact signed fields;
2. HTTPS and same-origin package location;
3. exact package size and SHA-256;
4. monotonic version progression;
5. WinTrust/Authenticode validity and the pinned Lira publisher.

The updater repeats Authenticode/publisher verification for the staged agent
executable, managed assembly, and Credential Provider. The signed package is not
allowed to install its own trust anchor. The Lira Code Signing Root must be
distributed independently through GPO, authenticated WinRM, console, or another
administrator-controlled path.

Agent 2.7.33 release identifiers:

| Field | Value |
| --- | --- |
| Manifest algorithm | RSA-PSS with SHA-256 |
| Manifest key ID | `lira-release-rsa-2026-01` |
| Update package SHA-256 | `945b3e71ca582ba24fc94d5da494baa9ac98495a26fcbec251979ffc6eba88cd` |
| Update package size | `2,037,756` bytes |
| Signing-root thumbprint | `33F8A2F17436A5E3CCD176A228B3FF9A808193ED` |
| Signing-root CER SHA-256 | `50257868BBD7C4484D8B233AAD3F36CE5266D6A1BDB600AFDA1D55B89F880A18` |

A SHA-256 value alone is not treated as an independent trust guarantee. Its
value here is protected by the signed manifest and complemented by publisher
pinning and independent trust-root provisioning.

## Verification evidence

Verification completed on 2026-09-03:

- Full backend suite: **545 passed, 1 skipped**. The skipped test is the opt-in
  PostgreSQL integration and was executed separately.
- Direct PostgreSQL 16 RLS integration: **1 passed** against a disposable
  instance; production data was not used.
- Release builds passed for the managed agent, updater, and native Credential
  Provider.
- Authenticode verification: **6 of 6** release artifacts valid under the pinned
  Lira publisher.
- The RSA-PSS manifest, exact package hash, and exact package size verified.
- Six frontend locale JSON files parsed successfully.
- The production frontend build completed with TypeScript checks; the recorded
  npm audit result contained zero known findings.
- Two controlled canaries—Windows Server 2016 and Windows Server 2022—installed
  2.7.33, resumed heartbeat, and completed a signed read-only diagnostics
  command.

The first canary attempt correctly failed with WinTrust `0x800B010A` because the
private signing root was not yet present. After independent authenticated
provisioning to the required machine certificate stores, both canaries updated
successfully. This failure is retained as evidence that an untrusted signed
binary was not silently accepted.

## Rollout state

Agent 2.7.33 is retained and available for controlled rollout, but it is not yet
the fleet-wide target. The global target remains 2.7.31. Remaining hosts require
independent signing-root provisioning and staged validation before promotion.

The live matrix still to be completed includes RDP/NLA, console, reconnect,
MFA-service and portal outage behavior, break-glass, rollback, credential
rotation, and WebSocket/polling replay scenarios. A successful pair of canaries
is evidence for the tested paths, not proof for every host or authentication
path.

## Assurance limits and follow-up work

- This is an internal, repository-backed verification. No independent
  penetration test, SOC 2 certification, or ISO 27001 certification is claimed.
- TOTP is not phishing-resistant; FIDO2/PIV remains future architecture work.
- The privileged control plane remains a high-impact trust boundary.
- mTLS, SID-bound identity, HSM/KMS-backed key custody, and formal certificate
  revocation/rollback design remain defense-in-depth follow-ups.
- Older leaf-only MFA identities should be re-enrolled before the compatibility
  lookup is removed.
- Fleet promotion remains gated on the live Windows test matrix and independent
  trust-root distribution.

Security issues should be reported through Vyvick's private security contact,
not through a public GitHub issue.
