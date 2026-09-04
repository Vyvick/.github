# Lira RDP Platform release notes

Customer-facing updates to the Lira RDP portal and service platform. Windows agent builds are documented separately. The newest update appears first.

This dated public platform history begins on 2026-08-24, when the customer-facing release series was introduced. Earlier engineering work is not reconstructed as release history without a retained public release record.

[View the release notes on vyvick.com](https://vyvick.com/en/platform-release-notes.html)


## 2026-09-04 — Native uninstall and signed-update interoperability

- Portal-script installations now register Lira in Windows Apps/Programs with interactive and quiet removal commands. MSI installations keep their Windows Installer registration without a duplicate entry.
- The generated elevated uninstaller removes the service, watchdog, Credential Provider and Filter registrations, local agent state, and locked files on reboot when required.
- The release builder now uses the SHA-256 digest-length RSA-PSS salt accepted by the .NET verifier and immediately verifies each generated manifest signature.
- Update URLs remain restricted to HTTPS. Only configured origins and the two controlled public Lira edges are accepted; arbitrary Host values, external hosts, HTTP, and non-443 ports remain rejected.
- Forced and scheduled update paths are serialized so heartbeat, WebSocket, and polling delivery cannot launch competing updaters against the same package.
- Agent 2.7.39 is the global target. Twenty-two of 25 active agents upgraded automatically; three failed closed on missing independent signing-root trust and remain online on 2.7.34 pending manual provisioning.
- Full backend regression: 552 passed, one opt-in test skipped. [Read the complete 2.7.39 engineering record](../security/release-2.7.39-engineering.md).


## 2026-09-04 — Agent event-collection compatibility fix

- During the staged 2.7.33 rollout, agents continued to heartbeat while their Windows Security event checkpoints stopped advancing.
- The root cause was a response-contract mismatch: the idle update manifest returned null numeric fields that 2.7.33 deserialized as required integers. The update-check exception interrupted the collection iteration.
- The backend now returns stable numeric idle values. Existing 2.7.33 agents resumed automatically from their durable checkpoints; Windows Security logs were not cleared.
- Agent 2.7.34 accepts nullable idle-manifest fields defensively and isolates all update-check failures from event collection.
- Twenty-two of 25 active agents now run 2.7.34 and completed signed read-only command checks. The global target is now 2.7.34; three agents failed closed on missing independent signing-root trust and remain safely online on 2.7.31 pending manual provisioning.
- Full backend regression: 549 passed, one opt-in test skipped. [Read the incident engineering record](../security/incident-2026-09-04-agent-event-collection.md).


## 2026-09-03 — Security hardening and staged agent rollout

- Agent command delivery now uses per-agent HMAC-SHA256 envelopes with expiry, agent binding, a local command allowlist, and a durable at-most-once replay ledger.
- The legacy shared agent token has been retired from HTTP and WebSocket authentication; personalized installers use short-lived enrollment credentials.
- MFA policy now exposes explicit compatibility `fail_open` and strict `fail_closed` behavior. The Windows Credential Provider Filter prevents fallback to another password-only provider in the protected fail-closed path; Windows LSA remains responsible for credential validation.
- TOTP secret storage, phishing limitations, signed-update trust, independent recovery requirements, and the privileged control-plane boundary are now documented explicitly.
- The full backend suite completed with 545 passed and one opt-in PostgreSQL test skipped; that direct PostgreSQL 16 RLS test passed separately against a disposable instance.
- Agent 2.7.33 passed staged update and signed read-only command canaries on Windows Server 2016 and Windows Server 2022. The global agent target remains 2.7.31 while trust-root provisioning and the remaining live matrix continue.
- [Read the complete security engineering report](../security/engineering-2026-09-03.md).

## 2026-09-02 — Agent connection reliability

- Server-side routing for the primary live agent channel has been corrected for installations that had switched to polling fallback.
- Existing agents return to the primary channel through their normal automatic retry cycle; no agent reinstall or new Windows agent build is required.
- The polling fallback remains available during a live-channel interruption so agent status and command delivery can recover without manual re-enrollment.

## 2026-08-28 — French portal and documentation corrections

- French pilot metrics now show either the published country rows and privacy-threshold notice or the insufficient-data message, never both, while keeping the total observed country count distinct from the published list.
- The French Privacy Policy now includes the AI Data Processing section explaining the deterministic engineering assistant, the optional support assistant and which operational or secret data is not attached automatically.
- French Security Engineering badges now render as separate brand, result, review-scope and date elements, and portal-credential wording in the secure-start guide has been corrected.
- Homepage spacing and interactive-demo copy, product and service descriptions, and the RDP MFA and Agent Installation guides have received a final professional French editorial pass.

## 2026-08-28 — German portal and documentation localization

- The complete German portal, including authenticated screens, validation messages, notifications and transactional email, has been editorially reviewed for professional DACH infrastructure operations.
- German sign-in, signup and password-recovery pages now return localized initial HTML before the application loads, and language selection is retained across authentication and legal links.
- Terminology for server fleets, agent registration, status signals, allowlists, recovery, MFA, Windows Update, Microsoft Defender and RDP certificates is now consistent across the portal, guides and release notes.
- The public pilot metrics now distinguish the total number of observed source countries from the smaller set published after the privacy threshold is applied.

## 2026-08-26 — Subscription and access continuity

- The portal now warns administrators five days before a subscription ends and clearly shows the following recovery period.
- Existing installations can reconnect for ten days after the subscription ends, allowing service to recover without reinstalling the agent after payment.
- If protection is suspended, the saved MFA policy is retained and restored automatically when the subscription becomes active again.
- Agent pages now distinguish the live control channel from the polling fallback and prevent unsafe removal while MFA enforcement is still active.
- Windows Update installation now enters the in-progress state without briefly repeating an error from an earlier attempt.

## 2026-08-25 — Portal session reliability update

- Portal tabs left open after the session expires now redirect to sign-in on the next action instead of leaving dashboard updates pending. Sign in again to continue.

## 2026-08-24 — Security and reliability update

- The portal now provides authorized administrators with guided server-readiness and access-recovery assistance.
- Registration reliability was improved for periods with multiple simultaneous new visits.
- Browser-side security controls were strengthened to reduce the risk of unauthorized page content being executed.
