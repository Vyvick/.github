# Lira Agent 3 and Lira Access — public security model

Updated: 2026-10-09

This document describes intended and implemented security boundaries in the
closed technical preview. It is not a claim of absolute security, a formal
certification or a statement of general availability.

## Trust boundaries

- Windows credentials are validated by Windows on the protected host. They are
  not sent to or stored by the Lira portal.
- Each Agent 3 installation and Lira Access device has its own scoped identity.
  A device identity is bound to its organization, user, installation, device and
  identity epoch.
- Lira Access generates an ECDSA P-256 private key locally in Windows CNG. The
  private key is non-exportable. Real standard-user tests on Windows 10 and
  Windows 11 verified a protected ACL containing only SYSTEM and the owning user.
- The portal stores public identity material and fingerprints, not endpoint
  private keys.

## Control-plane limits

- Agent 3 accepts versioned, narrowly typed operations. There is no generic
  shell, PowerShell, arbitrary executable or upload-and-execute operation in the
  v3 control contract.
- Signed messages are bound to their intended organization, server or device,
  installation and epoch, and use expiry and replay protection.
- Device identity rotation requires proof from both the current and replacement
  private keys. A successful rotation advances the identity epoch once.
- Ordinary portal policy cannot replace local trust anchors or silently expand
  the agent's local capability ceiling.

## Local protection and failure behavior

- Local RDP attack protection is designed to continue during a portal or network
  outage. Temporary local decisions have bounded lifetimes and durable recovery
  records.
- Components for supervision, event collection, transport, local protection,
  updates and MFA are separated so a blocked integration does not silently stop
  every function.
- Privileged effects use bounded workflows, durable replay fences and explicit
  verification. Unknown outcomes are not automatically repeated.
- Security-sensitive activation remains gated behind supported-OS acceptance and
  documented independent recovery paths.

## Current evidence

- The current managed Agent 3 development suite passes 660 contract and security
  checks with zero compiler warnings or errors.
- The same signed self-contained test payload passed interactive standard-user
  CNG acceptance on Windows 10 and Windows 11.
- Tests verified public-key export, signing, private-export rejection, exact ACL,
  rejection of a substituted key fingerprint and exact cleanup of the test key.
- Tenant-scoped invitations, device approval/revocation and dual-key rotation are
  implemented behind a feature flag that remains disabled by default.

## Remaining release gates

- End-to-end Lira Access client and tunnel validation, including traffic capture,
  revocation and cross-tenant substitution tests.
- Public/commercial code signing for release artifacts.
- Stability testing of the exact signed release binary.
- Independent penetration testing and security review.
- Operational key-revocation, incident-response and recovery exercises.

## Scope limitations

No software can guarantee absolute security. An attacker who already controls the
Windows kernel, SYSTEM account, hypervisor or release-signing authority is outside
the protection boundary described here. A compromised portal must still be unable
to export endpoint private keys or turn the restricted v3 contract into arbitrary
remote code execution; this property remains a continuing release requirement.

Report suspected vulnerabilities privately through the security contact at
[vyvick.com](https://vyvick.com/en/security.html). Do not place credentials,
enrollment tokens, private keys or customer data in a public GitHub issue.
