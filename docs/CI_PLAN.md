# CI Plan (not active yet)

No CI/CD exists in this repository yet, so no status checks are
required on `main` or `develop`.

Once implementation starts, the intended required checks on `main`
are expected to be:

- build
- lint
- typecheck / static analysis
- unit tests
- basic security scan

When a GitHub Actions workflow providing these exists and is stable,
enable them as required status checks on the `main` ruleset (see
`gh api repos/OWNER/REPO/rulesets` to add a `required_status_checks`
rule referencing the workflow job names).
