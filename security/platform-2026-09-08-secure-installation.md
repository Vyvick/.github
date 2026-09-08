# Lira platform update — 8 September 2026

## Scope

This platform update improves the way Lira RDP Security and Lira Endpoint are
installed, enrolled and maintained. The public Windows agent version remains
`2.7.39`; this record does not announce a replacement agent release.

## Safer enrollment and local state

- New installations use a short-lived, organization-bound Install ID. The
  durable agent identity and enrollment credentials are resolved by the portal
  and are not supplied on the Windows service command line.
- The Install ID is written only to the protected machine state directory and
  is removed after successful enrollment. Sensitive MSI properties are hidden
  from normal Windows Installer logging.
- Agent state and the `staging`, `updates` and `update-run` working directories
  are restricted to LocalSystem and local Administrators.
- The updater validates the Authenticode signature of every executable and
  library in a package before copying files into Program Files.
- Installer-created Microsoft Defender exclusions for Lira directories and
  processes were removed. Installation no longer requires a broad antivirus
  exclusion.

## Server and Endpoint installation paths

- The portal Devices section separates Servers, Endpoints and the combined
  device list while retaining one fleet view for operators.
- The installation dialog has dedicated Server and Endpoint tabs. Each path
  presents the Install ID first, followed by the matching MSI download and
  PowerShell deployment command.
- Server and Endpoint packages have distinct Windows Installer product
  identities. Enrollment is bound to the requested device class, agent profile
  and release channel; a mismatched package is rejected.
- Lira Endpoint uses the restricted observation profile. Server-only actions
  such as RDP MFA, RDP TLS changes, blocking, reboot and other mutating commands
  are unavailable to that profile.
- Endpoint license consumption is reported separately in the portal. The
  current commercial price remains free while the Endpoint pilot is being
  evaluated.

## Installer reliability and language coverage

- The graphical MSI wizard accepts the Install ID before entering the standard
  Windows installation flow.
- The installation-directory dialog is now bound to the correct MSI property,
  resolving Windows Installer error `2819` after the connection step.
- The wizard supports all current portal languages: English, Ukrainian,
  Russian, German, Spanish and French.
- Server packages reject workstation Windows editions, and Endpoint packages
  reject Windows Server editions, preventing accidental installation of the
  wrong profile.

## Bootstrap and update verification

- Portal installers carry the Lira signing root and validate its pinned
  SHA-256 fingerprint and certificate thumbprint before adding it to the
  required Windows trust stores.
- Installation metadata is signed with RSA-PSS. The installer verifies the key
  identifier, signature, release channel, version, package size and SHA-256
  digest before extraction.
- Active bootstrap paths use HTTPS and fail closed when the configured portal
  certificate authority or signed metadata cannot be verified.

## Operational recovery

- Health email and Telegram notifications now use a readable operator summary,
  identify the affected component and provide a concrete portal action instead
  of displaying a raw diagnostic dictionary.
- Central-firewall jobs retry with bounded backoff. The production recovery
  cleared 43 failed jobs and nine failed expiry records without removing active
  Windows protection.
- Agent-reported local blocks now receive the configured expiry automatically.
  Legacy open-ended records were assigned an operator-approved 24-hour expiry,
  and new records cannot silently remain indefinite.

## Current status and limits

- The active server fleet completed rollout to agent `2.7.39` after the signing
  root was provisioned on the three hosts that had previously failed closed.
- Existing installations retain compatibility, but the strongest local-state
  and enrollment protections apply after installing or updating to the current
  package.
- Public-trust code signing is being prepared separately. The current release
  chain continues to use the pinned Lira publisher and signed manifests.
- TPM-backed device identity, mutual TLS ingress, asymmetric command approval
  and multi-role update metadata remain future architecture work and are not
  claimed as part of this update.

