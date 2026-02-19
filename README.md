# Release Guardian Canary

This repository is the dedicated canary environment for validating Release Guardian behavior before broad rollout changes.

It intentionally tests:
- `Go` (clean PR)
- `No-Go` / `Conditional` (introduced risk)
- Comment upsert and status stability
- Regression reports for new Release Guardian versions

## Action Source

This canary consumes the published action tag:

- `lseino121-projects/release-guardian@v1.2.5`

## Default Policy

Defaults are in `.release-guardian.yml`:
- `mode: enforce`
- `severity_threshold: high`
- `allow_conditional: false`

## Smoke Scenarios

1. `Go` scenario
- Open PR with docs-only change.
- Expected: `Go`, `RDI 100`, status context `release-guardian/rdi` successful.

2. `No-Go` scenario
- Open PR adding file `insecure.py` with:
  - `import subprocess`
  - `subprocess.Popen("echo test | sh", shell=True)`
- Expected: introduced code finding, blocking verdict in enforce mode.

3. Recovery scenario
- Fix or remove risky code in same PR.
- Expected: verdict returns to `Go`.

## Reporting Regressions

Use issue template: `.github/ISSUE_TEMPLATE/canary-regression.md`

Include:
- expected vs actual verdict
- PR URL
- top comment snippet
- commit SHA
- any scanner/runtime failure context

## Smoke: Go Scenario

This branch is a docs-only change for expected Go validation.
