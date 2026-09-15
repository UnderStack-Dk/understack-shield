# MVP (v1 scope)

Scope for the first buildable release, for a 3-developer team
(`docs/WORK_BREAKDOWN.md`), based on the approved mock
(`docs/assets/mock-v1.png`) and the spec. "MVP" here means
*first release scope*, not reduced quality — the spec is
explicit that nothing shipped should be a placeholder, mock data, or
fake protection (§34), and that stays true for v1 too. What's "out of
scope" below is deferred to a later phase in full, not shipped as a
stub.

## Scope

Exactly 3 primary screens, matching the mock: **Home**, **Protection**,
**Settings**. See `docs/ARCHITECTURE.md` for the module wiring behind
each.

### Protection — v1 defenses (10, per the mock)

Each row is real capability, not a mock switch (spec §34),
with three states (On / Off / Limited by Android):

1. Wi-Fi & Network Defense — spec §5 Wi-Fi
2. Mobile Data Monitoring — §5 Cellular data
3. Safe Browsing & Anti-Phishing — §6 Web/URL
4. DNS Protection — §5 DNS
5. App & Permission Guard — §10 (covers sideload risk too)
6. NFC Protection — §7
7. Bluetooth & BLE Defense — §8
8. USB / ADB Defense — §9
9. Notification & SMS Defense — §12
10. Device Integrity — §10 (root/bootloader risk)

### Settings — v1 sections (per the mock's grouping)

General (Language, Alerts & Noise Control), Security & Protection
(Autonomous Defense, Lockdown Mode), Privacy & Data (Privacy & Data
Handling, Incident History), Device & Support (Battery Optimization,
Help & Support). Spec items without an obvious top-level row
in the mock get nested inside these, not dropped:

- Protection mode (Balanced/Strict/Custom) → inside Autonomous Defense.
- Local-only mode, external reputation lookups on/off, export security
  report, clear local history → inside Privacy & Data Handling.
- VPN/DNS engine status, notification-access status, accessibility
  status (only if a feature needs it) → inside Autonomous Defense or a
  technical-status sub-view, per spec §3's requirement that
  the app "never pretend full control when unavailable."
- Privacy policy, licenses, app version/build → inside Help & Support.

### Home

Status hero (Protected / Shield Active), one primary control
(Start Shield / Shield Active), settings entry point — matches the
mock. See "Open decisions" below for the status-card question.

## Out of scope for v1 (deferred, not cut)

Everything below still gets built in full, later — sequenced in
`docs/WORK_BREAKDOWN.md`:

- Remaining spec §3 protections not in the mock's 10:
  Hotspot/Tethering, Clipboard Tampering, Screen Overlay/Accessibility
  Abuse, Camera/Microphone Access Monitoring, Location Access
  Monitoring, File/Download Scanning, Deep Link/Intent Protection,
  TLS/Certificate Anomaly Detection, SIM/eSIM Change Signals,
  Behavioral Anomaly Detection.
- The full Local Ethical Hacker / Self-Pentest Engine / Security Score
  / AI Threat Defense / Agent Action Firewall extension.
- Spanish, Danish, German localization (English ships first; all four
  are required before this reaches Google Play per spec §23
  and §34 — not required for an internal v1 build).
- External reputation provider integration (ships local-only first,
  per spec §27's own fallback requirement).

## Acceptance criteria (v1)

Scoped down from spec §34's Definition of Done:

- All 3 screens complete and polished in light/dark mode.
- All 10 v1 protection toggles wired to real capability state — no
  mock switches.
- Core VPN/DNS/network engine (`defense-network`) works on real
  Android hardware.
- NFC, Bluetooth, USB, app/permission, device-integrity modules use
  real platform APIs; anything Android doesn't expose degrades to
  "Limited by Android," never faked.
- Automatic response policy (`engine-response`) implemented and
  tested for the v1 defenses.
- No hack-back logic anywhere.
- English strings complete and reviewed; ES/DA/DE scaffolded but not
  blocking.
- Battery/lifecycle behavior tested over an extended run.
- Encryption, retention limits, and clear-history all implemented.
- `ci.yml`'s build/lint/test jobs green on real Gradle code (not just
  the no-op skip state).

## Open decisions (mock vs. spec — not resolved here)

Two things in `docs/assets/mock-v1.png` conflict with spec
§2–3 and need an explicit call from the project owner before Track C
builds Home for real:

1. **The glow behind the shield icon.** Spec §3 explicitly
   rules out "glowing AI gradients" and "floating orbs." The mock's
   hero treatment is exactly that. Either the spec's rule
   holds (flatten the hero to a plain line/filled icon, no radial
   glow) or the owner amends that rule to allow this one signature
   effect. Not picked here.
2. **Home's 3-item status card** ("Autonomous protection active /
   12 protections enabled / No action required"). Spec §2
   asks for a single small calm status line, plus — only when a
   threat was actually contained — one additional one-line summary.
   The mock's permanent 3-line card is more than that spec allows.
   Recommend collapsing it to the single optional line, but this is
   the owner's call, not assumed here.

## What this document does not do

It does not assign people to tracks (one edit in
`docs/WORK_BREAKDOWN.md` once decided). It does not pick dates — see
`docs/ROADMAP.md`, which stays phase-only by the owner's own design.
