# UnderStack Shield

**AI-assisted active mobile security.**

[![Status](https://img.shields.io/badge/status-planning%20%2F%20pre--development-blue)]()
[![License](https://img.shields.io/badge/license-TBD-lightgrey)]()

> No production implementation has started yet. This repository currently
> contains project foundation, governance and documentation only.

## The problem

Mobile devices now hold more trust than most people realize: banking and
identity, personal communication, and increasingly, AI agents acting on the
user's behalf with real permissions. Most mobile security tools are still
built for an older threat model — they scan for known signatures, report
after the fact, and stay silent about anything they weren't explicitly
told to look for.

That model breaks down against two things that are growing fast: attacks
that adapt in real time, and AI agents that can take actions faster than a
person can review them. Users are left either buried in alerts they can't
interpret, or with no visibility at all until something has already gone
wrong.

## What UnderStack Shield is

UnderStack Shield is planned as an **active, AI-assisted mobile security
platform** — one that reasons about risk signals across the device,
the network, and AI-agent activity, and helps the user make a decision
before damage is done, instead of only reporting damage after the fact.

Two principles drive every design decision:

- **Local-first** — process and reason about signals on-device wherever
  possible.
- **Privacy-first** — collect the minimum necessary signal, and never
  enforce silently without the user's visibility.

See [`docs/PRODUCT_VISION.md`](docs/PRODUCT_VISION.md) for the full
vision and [`docs/SECURITY_PRINCIPLES.md`](docs/SECURITY_PRINCIPLES.md)
for the principles behind it.

## Conceptual architecture

```
Signals -> Security Event Layer -> Risk/Policy Evaluation -> User/System Action -> Incident Record
```

Planned areas: Device Security, Network Security, Shield Core, AI/Agent
Security, Event Model, Risk/Policy Layer, Action Layer, Forensics, UI,
Data/Privacy. None of this is implemented yet — see
[`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) for the current,
intentionally non-binding placeholder.

## Working methodology

This project is built by a small team with one person holding final say
over what reaches `main`. The workflow is deliberately simple:

```
developer branch (feature/*, fix/*, refactor/*, chore/*, name/*)
        |
        v
   Pull Request
        |
        v
     develop  (integration, light review)
        |
        v
   Pull Request
        |
        v
       main   (protected: PR required, owner review required,
               no force-push, no deletion, no direct push)
        |
        v
   final owner/maintainer decision
```

- `main` is the stable, release-ready branch. It is protected: every
  change requires a Pull Request, an approving review from the repository
  owner (via `CODEOWNERS`), and resolved review conversations. Force-push
  and branch deletion are blocked for everyone, with no bypass.
- `develop` is the integration branch for work in progress. It is
  protected against force-push and deletion, but does not require a PR
  yet — kept light while the team is small.
- Commits follow a plain convention: `feat:`, `fix:`, `refactor:`,
  `docs:`, `chore:`.

Full contributor steps: [`CONTRIBUTING.md`](CONTRIBUTING.md).
Rationale and phases: [`docs/DEVELOPMENT_WORKFLOW.md`](docs/DEVELOPMENT_WORKFLOW.md)
and [`docs/ROADMAP.md`](docs/ROADMAP.md).

## Repository map

| File | Purpose |
|---|---|
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | How to contribute, step by step |
| [`docs/PRODUCT_VISION.md`](docs/PRODUCT_VISION.md) | Why this project exists |
| [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) | Conceptual boundaries (placeholder) |
| [`docs/SECURITY_PRINCIPLES.md`](docs/SECURITY_PRINCIPLES.md) | Design principles |
| [`docs/DEVELOPMENT_WORKFLOW.md`](docs/DEVELOPMENT_WORKFLOW.md) | Branching and review flow |
| [`docs/ROADMAP.md`](docs/ROADMAP.md) | Planned phases |
| [`docs/MVP.md`](docs/MVP.md) | MVP scope (to be defined) |
| [`docs/DECISIONS.md`](docs/DECISIONS.md) | Architecture decision log |
| [`docs/TEAM_STRUCTURE.md`](docs/TEAM_STRUCTURE.md) | Roles and permission model |
| [`docs/TEAM_ACCESS_SETUP.md`](docs/TEAM_ACCESS_SETUP.md) | How future developers get access |
| [`docs/CI_PLAN.md`](docs/CI_PLAN.md) | Checks planned once CI exists |

## Status

Planning and repository governance are in place. No application code,
no assigned tasks, and no collaborators have been added yet.
