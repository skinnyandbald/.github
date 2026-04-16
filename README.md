# skinnyandbald/.github

Community-health defaults and reusable workflows for every repo owned by `@skinnyandbald`.

## What's here

### `.github/pull_request_template.md`
Default PR template. GitHub automatically uses this for any repo owned by `skinnyandbald` that does **not** define its own `.github/pull_request_template.md` (or `docs/pull_request_template.md` / root `pull_request_template.md`). Per-repo templates always win.

### `.github/workflows/require-closing-keyword.yml`
Reusable workflow that fails a PR check if the body lacks a GitHub closing keyword (`Closes #N` / `Fixes #N` / `Resolves #N`, case-insensitive, optional colon, same-repo or `owner/repo#N` cross-repo).

**Use it from any repo:**

```yaml
# .github/workflows/require-closing-keyword.yml (in the consuming repo)
name: Require Closing Keyword

on:
  pull_request:
    types: [opened, edited, reopened, synchronize, ready_for_review]

permissions:
  pull-requests: read

jobs:
  check:
    uses: skinnyandbald/.github/.github/workflows/require-closing-keyword.yml@main
```

Updates to the regex/logic land here once and apply to every caller automatically.

## Why

On 2026-04-15, PRs merged in `skinnyandbald/SecondBrain` without closing keywords in their bodies, leaving tracking issues open. This repo standardizes the prevention (template + CI guard) so every sibling repo inherits the same discipline without copy-paste drift.

## Cross-repo auto-close caveat

Neither the template nor the CI guard can make cross-repo `Closes owner/other-repo#N` references auto-close the linked issue — that's a GitHub platform limitation, not a config gap. Cross-repo keywords only create a mention link; the target issue stays open. For cross-repo tracking, either mirror an issue in the code repo and close it via the PR, or close the upstream issue manually on merge.
