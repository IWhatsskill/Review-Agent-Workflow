# Noise Filter

Goal: few, real findings.
A review should not look useful because it reports many small non-issues.

## Do Not Report

- style issues without risk
- pure renames
- subjective architecture preference
- "would be nicer if"
- missing comments
- tests that would be nice but do not cover a concrete risk
- existing bugs not touched or worsened by the PR
- hypothetical inputs without realistic trigger
- "cannot happen" cases without code path
- formatter/lint issues that CI clearly reports

## Downgrade Instead Of Inflating

Downgrade when:
- trigger is rare or only operator misconfiguration
- impact is unclear
- only a test is missing
- behavior was already broken and is not worsened
- fix is visibly and reasonably out of scope
- affected function is internal/experimental and has no clear user consequence
- the review sees only a test gap, not a bug path

## Security Discipline

A security finding needs:
- actor
- required access
- attack path
- gain/impact
- why this PR newly enables or worsens it

Do not repeat secret values.
Do not quote private accounts, paths, or tokens.

If a possible secret value appears in a diff/log:
- treat it as a leak risk
- do not copy the value
- describe only path, type of secret, and risk
- do not publish without explicit security/operator GO

## Review Focus

Prefer reporting:
- incorrect state transitions
- persistence or migration errors
- auth or permission errors
- mishandled fallbacks
- race conditions
- missing cleanup or rollback paths
- inconsistent config/schema/docs
- tests that assert something different from the code behavior
- proof that does not cover the risk

## Findings Bar

Before reporting a finding, check:

```text
Can a maintainer immediately decide or act on this?
Is the trigger realistic?
Is the impact concrete?
Is it new or worsened by this PR?
Is it more important than style/refactor preference?
```

If any answer is no, drop it, downgrade it to residual risk, or make it an open question.
