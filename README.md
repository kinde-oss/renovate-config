# renovate-config

Kinde's shared [Renovate](https://docs.renovatebot.com/) preset.

## Using it

In a repo, replace `renovate.json` with:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>kinde-oss/renovate-config"]
}
```

Repo-specific overrides go after the `extends` line, for example `"automerge": false` to opt a repo out of auto-merging.

## What it does

| Update | Behaviour |
| --- | --- |
| Stable minor / patch / digest (npm, pnpm, GitHub Actions) | Grouped into one weekly PR. Auto-merged once CI **and** the `dependency-risk` check pass. |
| Any `0.x` package | Separate PR, never auto-merged (a minor bump can be breaking). |
| Major | Waits for approval in the Dependency Dashboard issue, never auto-merged. |
| Lockfile maintenance | Monday before 6am, auto-merged. |
| Security fixes | Raised immediately, bypassing the 3-day release age. |

Every release must be at least **3 days old** before Renovate proposes it. That's the main protection against compromised npm releases, which are usually unpublished within hours.

## Required repo settings

Auto-merge only happens when all of these are true:

1. **Allow auto-merge** is enabled in the repo's settings.
2. A ruleset on the default branch requires your CI checks and `dependency-risk` to pass.
3. Renovate can merge without a human review, either via the Renovate Approve app or by adding the Renovate bot as a bypass actor on the review rule.

The `dependency-risk` check comes from the reusable workflow in [`kinde-oss/.github`](https://github.com/kinde-oss/.github).
