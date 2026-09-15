# MVP Work Breakdown — 3 Developers

Source of truth for scope: `UnderStack AI Shield — Complete Production
Spec` and its extension `Local Ethical Hacker + Continuous
Self-Pentest Engine` (both supplied by the project owner). This
document only translates that spec into parallel, issue-sized work
for three developers; it does not redefine architecture or MVP scope
— that stays the owner's call per `docs/DECISIONS.md`.

Stack: Kotlin, Jetpack Compose, Hilt, Room, multi-module Gradle
(spec §20–21).

## Tracks

Three tracks map to the module list in spec §21, split so
each dev can work with minimal cross-track blocking. Each track owns
its modules end to end (data → domain → UI wiring for its own
screens/rows).

### Track A — Core Engine, Policy & Network

Owns: `core-model`, `core-database`, `core-crypto`, `core-policy`,
`engine-correlation`, `engine-response`, `defense-network`,
`reputation-api`.

Responsible for the pipeline in spec §4
(Observe → Normalize → Score → Correlate → Verify → Contain →
Recover → Log), the risk-level model, Wi-Fi/cellular/VPN/DNS defense
(§5), the reputation-provider abstraction (§27), and — once the
extension phase starts — the Security Posture Engine and Security
Score (extension §3, §6).

### Track B — Device, App & Communication Surfaces

Owns: `defense-web`, `defense-nfc`, `defense-bluetooth`,
`defense-usb`, `defense-apps`, `defense-files`.

Responsible for URL/phishing analysis (§6), NFC (§7), Bluetooth/BLE
(§8), USB/ADB (§9), app/permission/integrity monitoring (§10), file
and APK scanning (§11). Once the extension phase starts, owns the
device/app/network checks that feed the Local Ethical Hacker's
Security Posture Engine (extension §3) and the corresponding Red-Team
test scenarios (extension §25) for these surfaces.

### Track C — UI, Social Engineering, AI/Agent Security & Release

Owns: `core-ui`, `feature-home`, `feature-protection`,
`feature-settings`, `defense-social-engineering`, `localization`,
`reporting`, plus app-shell wiring, CI/CD and release readiness.

Responsible for the three primary screens (§2) exactly as specified
(no fourth screen, ever — including for the extension per its §18),
notification/SMS/clipboard defense (§12), localization EN/ES/DA/DE
(§23), incident reports (§26). Once the extension phase starts, owns
AI Threat Defense and the Agent Action Firewall (extension §11–13),
since both are policy layers that sit closest to the UI/consent
surface this track already owns.

This track is the natural one for the project owner to take, since it
also owns final integration, CI/CD and release gating (§29–31).

## Phased delivery (maps to spec §"Implementation Order" +
extension §26)

Work proceeds in the phases below. Within a phase, the three tracks
work in parallel on their own modules; a phase is "done" only when
all three tracks finish it (keeps the app buildable and demoable at
every phase boundary — the spec's Definition of Done, §34,
is cumulative, not a final-week scramble).

| Phase | Track A | Track B | Track C |
|---|---|---|---|
| 0. Foundation | `core-model`, `core-database`, `core-crypto` scaffolding | — | Gradle multi-module skeleton, Compose theme, permission-state model, localization scaffolding (EN source strings) |
| 1. Three-screen UI | Expose protection-state flow for rows | Expose protection-state flow for rows | Home, Protection, Settings — wired to real (not mock) toggle state per §2 |
| 2. Core engine | `core-policy`, `engine-correlation`, `engine-response`, risk levels (§4) | — | Incident detail sheet (no 4th screen) |
| 3. Network layer | VpnService, DNS, ConnectivityManager, reputation abstraction (§5, §27) | — | Wire Wi-Fi/Cellular/VPN/DNS rows to Track A's state |
| 4. Web/URL | — | URL parser, homograph/punycode checks, deep-link integration (§6) | Wire Web & URL row |
| 5. Device sensors | — | NFC, Bluetooth, USB/ADB, app/permission/integrity (§7–10) | Wire corresponding rows |
| 6. Social engineering | — | — | Notifications, clipboard, permitted SMS/RCS (§12) |
| 7. Files | — | SAF-based scanning, hashing, APK metadata (§11) | Wire File/Download row |
| 8. Adaptive behavior | Local baseline + anomaly detection, explanation generation (§15) | Feed device/app signals into baseline | Render explanations in incident detail |
| 9. Response | Containment/lockdown policy engine (§16–17) | — | Notification copy for contained threats (§18) |
| 10. Hardening | Threat model own app, secrets/crypto, exported-component audit (§22) | Same, for its own modules | Same, for its own modules; also: apply OWASP dependency-check plugin so `ci.yml`'s dependency-scan job activates |
| 11. QA & release | Unit tests for scoring/correlation/policy (§28) | Instrumented + edge-case tests for its sensors (§28) | Compose UI tests (3 screens), localization tests EN/ES/DA/DE, signed AAB, Play Data Safety prep (§29) |
| Ext. Local Ethical Hacker | Security Posture Engine, Security Score (ext. §3, §6) | Device/app/network posture checks feeding it (ext. §3) | Home/Protection/Settings integration (ext. §18) |
| Ext. Self-Pentest | Correlation hooks for test results (ext. §15) | Sandboxed test scenarios for its own surfaces (ext. §4, §25) | Self-test result UI (`X / Y protections passed`), detail report view (ext. §5, §18) |
| Ext. AI/Agent security | Feed agent-firewall decisions into correlation (ext. §15) | — | AI Threat Defense pipeline + Agent Action Firewall (ext. §11–13) |

## Turning this into issues

Each cell above is issue-sized (or splits into 2–3 issues). Use the
existing `.github/ISSUE_TEMPLATE/feature_request.yml` for build-out
work and `security_review.yml` for anything touching permissions,
secrets, or the policy/firewall layers. Suggested labels:
`track:core-network`, `track:device-comms`, `track:ui-social-ai`,
plus a `phase:N` label per row above, so the board can be filtered
either by owner or by phase.

## What this document does not do

It does not define the MVP's functional scope, acceptance criteria,
or timeline — `docs/MVP.md` stays the owner's document, filled in
from the spec. It does not assign specific people to tracks
— that's a one-line edit once the owner decides who takes A/B/C. It
does not touch `docs/ARCHITECTURE.md`, `docs/ROADMAP.md`, or
`docs/DECISIONS.md`, which remain the owner's to write.
