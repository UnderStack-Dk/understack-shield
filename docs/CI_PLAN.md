# CI Plan

Two workflows exist under `.github/workflows/`:

- `ci.yml` — build (`assembleDebug`), lint/static analysis (`detekt`,
  Android Lint), unit tests, and a dependency vulnerability scan.
  Each job checks for a committed `gradlew` first and exits cleanly
  if the Android project doesn't exist yet, so doc-only PRs stay
  green. The moment the first Gradle module lands, these jobs
  activate with no further changes needed here.
- `codeql.yml` — CodeQL static analysis for Java/Kotlin (the "basic
  security scan"), on PRs, pushes, and a weekly schedule.

Once the Android project exists and these jobs have run green at
least once, enable them as required status checks on the `main`
ruleset (Settings → Rules → Rulesets → main → Require status checks
to pass, or `gh api repos/OWNER/REPO/rulesets` for a
`required_status_checks` rule referencing the job names above).
`develop` can pick up the same checks as non-blocking signal first.
