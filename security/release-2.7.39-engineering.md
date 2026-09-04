# Lira RDP Agent 2.7.39 engineering record

Date: 2026-09-04  
Status: global production target; 22 of 25 active agents upgraded automatically

## Scope

This release makes portal-script installations manageable through the native
Windows Apps/Programs interface and repairs two independent defects discovered
while validating the production update path. It does not change the Windows
password-validation boundary: Windows LSA remains responsible for primary
credential validation.

## Native Windows uninstall support

Installations performed by the personalized portal script are not MSI-managed,
so they previously had no entry under Apps & features / Programs and Features.
The agent now creates a machine-wide uninstall registration at:

`HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\LiraRdpAgent`

The entry contains display name, installed version, publisher, install path,
icon, product URL, estimated size, and separate interactive and quiet uninstall
commands. If Windows Installer already owns a Lira MSI registration, the agent
does not create a duplicate entry.

The registered command invokes
`C:\ProgramData\VyvickAgent\Uninstall-LiraAgent.ps1`. It self-elevates when
required, removes the external watchdog task, unregisters the Credential
Provider and Credential Provider Filter, stops and deletes the Windows service,
terminates remaining agent processes, removes installation and state data, and
deletes its Apps registration. Files still locked by Windows are scheduled for
deletion at reboot and the operator is told when a restart is required.

Uninstalling intentionally removes the local agent identity and state. A later
installation therefore requires a new valid enrollment link or MSI enrollment
token.

## Update-origin handling

The backend now derives the package origin from the incoming request only when
that origin matches one of the configured Lira portal/agent origins. An
untrusted Host value is not reflected. The agent treats only the official
`https://lira.vyvick.com:443` and `https://api.vyvick.com:443` edges as the same
public update boundary; arbitrary hosts, plain HTTP, and non-standard ports
remain rejected.

## RSA-PSS interoperability defect

The release builder previously generated a valid RSA-PSS signature with the
maximum permitted salt length. The .NET agent verifies through
`RSASignaturePadding.Pss`, whose Windows implementation expects salt length to
match the SHA-256 digest. As a result, the manifest passed server-side
structural checks but failed inside the agent.

The builder now uses a 32-byte SHA-256 salt and immediately verifies the exact
signature after creation. The unchanged agent-pinned RSA public key verifies
the 2.7.39 manifest. A diagnostic defect that retained the default URL error
after URL validation was also fixed, so a future signature failure is reported
as a signature failure rather than as an origin failure.

## Concurrent update defect

During the 2.7.38 transition, the same update could be initiated at nearly the
same time by heartbeat and realtime/fallback command delivery. Both paths then
opened the same ZIP, producing a transient sharing violation even though one
updater completed successfully. Agent 2.7.39 serializes forced and scheduled
update checks with one process-local semaphore. This prevents duplicate
downloads and updater launches without weakening command signature, replay, or
package validation.

## Trust and verification sequence

Before an update is executed, the agent requires:

1. an HTTPS package URL within the configured or explicitly recognized Lira
   public edge;
2. a supported manifest schema and monotonic target version;
3. an RSA-PSS/SHA-256 signature under the public key pinned in the agent;
4. exact package SHA-256 and byte length matching the signed payload;
5. Authenticode trust for the updater and each staged first-party executable,
   DLL, and PowerShell installer;
6. the expected Lira publisher identity before the staged updater runs.

The release signing key is not included in the agent or repository. Hosts that
do not trust the independently provisioned Lira signing root fail closed and
keep their currently running agent.

## Release evidence

- Update ZIP SHA-256:
  `e4400502278d20cee1e247c94c36a5fa5a7b45b6fe3af30e9265e480b44982d1`
- Full install ZIP SHA-256:
  `0e4e4b4193f5781becd4a7fa7a7a21863b633860714e50c7e92163ead9227c88`
- MSI SHA-256:
  `233f2e18bb35de5de916ec3dfae95bc1fba47419bea227f16ffb0017fd3afa55`
- All six first-party Windows artifacts reported valid Authenticode signatures.
- The signed update manifest was accepted by the production 2.7.36/2.7.38
  validation path.
- Agent 17 completed genuine automatic transitions to 2.7.38 and then 2.7.39.
- On 2.7.39, Windows Apps reported the correct version and both uninstall
  commands; the generated PowerShell script had zero parser errors.
- The canary service remained running, collector telemetry had no error, and
  signed read-only firewall command 2490 succeeded.
- Backend regression after the installer and request-origin work:
  552 passed, one opt-in PostgreSQL RLS test skipped.

The uninstall action itself was not executed on the production canary because
it is destructive: it removes the service and enrollment state. Registration,
command generation, script parsing, service health, and the surrounding update
path were validated without destroying a protected host.

## Fleet rollout

The global target was changed from 2.7.34 to 2.7.39. Twenty-two of 25 active
agents upgraded automatically and remained online. Three hosts failed closed
with WinTrust `0x800B010A` because the independent signing root is not present:

- agent 45 — `10.100.4.93`;
- agent 37 — `192.168.27.127`;
- agent 36 — `192.168.7.80`.

They remain online on 2.7.34 and require authenticated manual or policy-based
root-certificate provisioning before retrying the signed update. A stored
credential dedicated to agent 37 was rejected, and further password attempts
were stopped to avoid account lockout.

## Superseded candidates

Versions 2.7.37 and 2.7.38 were engineering candidates used to isolate the
manifest and concurrency defects. They were not promoted as the global target.
2.7.39 is the release intended for ongoing deployment.
