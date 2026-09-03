# Lira RDP Platform release notes

Customer-facing updates to the Lira RDP portal and service platform. Windows agent builds are documented separately. The newest update appears first.

[View the release notes on vyvick.com](https://vyvick.com/en/platform-release-notes.html)

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
