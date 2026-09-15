# Architecture

Conceptual architecture derived from the spec ("UnderStack AI
Shield — Complete Production Spec" and its "Local Ethical
Hacker" extension) and the approved v1 mock (`docs/assets/mock-v1.png`
— Home / Protection / Settings). This defines boundaries and data
flow for a 3-developer team (`docs/WORK_BREAKDOWN.md`); it does not
fix class names or method signatures — that's each track's call
inside its own modules.

## Stack

Kotlin, Jetpack Compose, Hilt, Room, Android Keystore, VpnService,
multi-module Gradle. (spec §20.)

## Core pipeline

Every sensor in every module feeds the same pipeline before any
defensive action is taken (spec §4):

    Observe -> Normalize -> Score -> Correlate -> Verify -> Contain -> Recover -> Log

- Every event carries: source, timestamp, confidence, severity,
  affected component, evidence references.
- No disruptive action fires on a single weak signal alone.
- Deterministic rules handle known-dangerous conditions; anomaly
  models handle behavioral deviation. Model output is never sufficient
  alone to trigger a destructive action — it must combine with
  deterministic evidence (spec §15, extension §2).

## Risk levels

`0 Normal → 1 Low → 2 Suspicious → 3 Probable Threat → 4 Confirmed/Critical`,
exactly as spec §4 defines them. Every module reports into
this scale; `engine-correlation` and `engine-response` are the only
places allowed to escalate a risk level into a user-visible action.

## Module map

Grouped by the 3 dev tracks from `docs/WORK_BREAKDOWN.md`. Names match
spec §21's suggested module structure.

| Module | Responsibility | Track | Depends on |
|---|---|---|---|
| `core-model` | Shared event/finding/incident data classes, risk enum | A | — |
| `core-database` | Room schema for local incident/event history | A | `core-model`, `core-crypto` |
| `core-crypto` | Android Keystore-backed encryption for local storage | A | — |
| `core-policy` | Confidence/severity → allowed-action mapping | A | `core-model` |
| `engine-correlation` | Observe→Normalize→Score→Correlate→Verify | A | `core-model`, `core-policy`, all `defense-*` |
| `engine-response` | Contain→Recover→Log; lockdown mode (§17) | A | `engine-correlation`, `core-policy` |
| `defense-network` | Wi-Fi, cellular, VPN, DNS (§5) | A | `core-network` (ConnectivityManager/VpnService wrapper), `reputation-api` |
| `reputation-api` | Optional external URL/domain/hash reputation, cached, degrades to local-only (§27) | A | `core-model` |
| `defense-web` | URL parsing, homograph/punycode, phishing (§6) | B | `reputation-api` |
| `defense-nfc` | NFC state + NDEF payload analysis (§7) | B | `defense-web` (shares URL analysis) |
| `defense-bluetooth` | Bluetooth/BLE pairing + trusted-device baseline (§8) | B | `core-model` |
| `defense-usb` | USB/ADB/wireless-debugging state (§9) | B | `core-model` |
| `defense-apps` | App inventory, permissions, signing, sideload risk, Play Integrity (§10) | B | `core-model` |
| `defense-files` | SAF-based file/APK scanning, hashing (§11) | B | `reputation-api` |
| `defense-social-engineering` | Notifications, clipboard, permitted SMS/RCS (§12) | C | `core-model`, `defense-web` |
| `core-ui` | Compose theme, shared components, Material 3 baseline | C | — |
| `feature-home` | Home screen | C | `engine-correlation` (status), `engine-response` (incident summary) |
| `feature-protection` | Protection screen — one row per defense module, wired to real state (never mock) | C | every `defense-*` module's public toggle/state API |
| `feature-settings` | Settings screen | C | `core-policy`, `reputation-api` (toggle), `engine-response` (lockdown), `core-database` (clear history/export) |
| `localization` | EN/ES/DA/DE string resources | C | — |
| `reporting` | Incident detail sheet + exportable report (§26) | C | `core-database` |

## UI architecture — 3 screens, no exceptions

Matches `docs/assets/mock-v1.png` structurally (spec §2 caps
this at 3 primary screens, permanently):

- **Home** (`feature-home`) — hero status (Protected / Shield Active),
  one primary control (Start Shield / Shield Active), settings
  entry point. Per spec §2 this screen carries *one* small
  calm status line plus, only when relevant, a single one-line
  contained-threat summary — see the open item in `docs/MVP.md` about
  reconciling this with the mock's 3-line status card.
- **Protection** (`feature-protection`) — vertically scrolling list,
  one row per `defense-*` module: icon, name, short status, toggle
  with three real states (On / Off / Limited by Android — spec §2).
  Each row calls into exactly one module's public API;
  `feature-protection` holds no defense logic itself.
- **Settings** (`feature-settings`) — grouped the way the mock does
  it, which maps cleanly onto spec §Settings items:
  - *General*: Language, Alerts & Noise Control (quiet mode, daily/weekly summary).
  - *Security & Protection*: Autonomous Defense (protection mode +
    automatic response toggle), Lockdown Mode.
  - *Privacy & Data*: Privacy & Data Handling (external reputation
    on/off, local-only mode, export report, clear history), Incident History.
  - *Device & Support*: Battery Optimization, Help & Support (privacy
    policy, licenses, version).

## Extension layer — Local Ethical Hacker / Self-Pentest

Sits alongside `engine-correlation`, not inside it: it produces the
same normalized event/finding shape and feeds the same correlation
engine (extension §15), so it does not require a 4th screen or a
parallel data model. Sequenced after the base defenses ship — see
`docs/WORK_BREAKDOWN.md`'s extension rows.

| Module | Responsibility | Track |
|---|---|---|
| `posture-engine` | Continuous Security Posture Engine + Security Score (ext. §3, §6) | A |
| `self-pentest` | Sandboxed test scenarios per surface, PASS/FAIL/DEGRADED (ext. §4–5) | B (per-surface tests) + A (orchestration/reporting) |
| `ai-threat-defense` | Trust classification / sanitization for external content (ext. §11) | C |
| `agent-firewall` | Agent Action Firewall policy layer (ext. §12–13) | C |

## What this document does not do

It does not pick colors, spacing, or exact string copy — that's
`core-ui` plus whoever owns the design system, working from the
Visual Design Requirements in spec §3. It does not resolve
the two open conflicts between the mock and the spec's
visual rules — see `docs/MVP.md`.
