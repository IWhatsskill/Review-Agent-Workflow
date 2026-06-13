---
name: review-agent-workflow
description: "Manual review-only PR and diff workflow with GO gates. Use for findings-first reviews, deep review reports, CI read-only checks, security/leak checks, address plans, or explicitly approved review publishing."
license: MIT
---

# Review Agent Workflow

This skill is a manual review-only workflow.
It is not an autopilot and it is not permission to fix code.

## Absolute Rule

Activate only when the operator explicitly names this skill or a review phase.

Without clear GO:
- do not read PRs
- do not read files
- do not run commands
- do not comment
- do not submit reviews
- do not patch
- do not run tests
- do not commit or push

Review-GO allows only reading, evaluating, and proposing.
Fixing, commenting, approving, requesting changes, or publishing needs its own explicit GO.

## Target And Scope

Before any real review, the target must be clear:
- repository or project
- PR, diff, branch, patch, or changed-file set
- review depth: `REVIEW-GO` or `DEEP-REVIEW-GO`
- whether CI/logs/security-leak checking is in scope

If the target or scope is unclear:
- stop
- ask one short clarifying question
- do not read beyond the named scope

Review phases may read:
- PR, diff, and changed files
- directly affected code and test paths
- relevant project rules when they affect the review
- CI/checks only when directly available or named

Review phases must not:
- start broad repo scans out of curiosity
- search unrelated issues or PRs unless they are named or directly linked as context
- expand logs that may contain secrets without explicit GO

## Phases

```text
Phase 0 = STARTTEST
Phase 1 = REVIEW-GO
Phase 2 = DEEP-REVIEW-GO
Phase 3 = CI-READ-GO
Phase 4 = SECURITY-LEAK-GO
Phase 5 = ADDRESS-PLAN-GO
Phase 6 = PUBLISH-REVIEW-GO
WAIT
```

## Phase Router

### STARTTEST

Read only `SKILL.md` and, if needed, `references/00-review-rules.md`.
Output what is forbidden without further GO.

### REVIEW-GO

Use this mode for a short findings-first PR or diff review.

Read:
- `references/00-review-rules.md`
- `references/01-output-format.md`
- `references/03-noise-filter.md`

Allowed:
- inspect PR/diff/changed files read-only
- inspect relevant project rules read-only when they affect the review
- summarize CI status read-only when directly available
- prioritize findings

Forbidden:
- patch
- tests/builds without separate GO
- GitHub/tracker write
- comment or submit review
- approve or request changes

### DEEP-REVIEW-GO

Use this mode for a longer maintainer-oriented review report.

Read:
- `references/00-review-rules.md`
- `references/02-deep-review.md`
- `references/03-noise-filter.md`

Allowed:
- evaluate PR surface, reproducibility, risk, proof quality, and maintainer options
- build an evidence list
- classify security and compatibility risks
- read directly relevant older or adjacent PRs/issues only when named or linked by the PR/operator context

Forbidden:
- patch
- GitHub/tracker write
- repair
- CI rerun

### CI-READ-GO

Read:
- `references/00-review-rules.md`
- `references/03-noise-filter.md`

Only inspect CI/checks/logs read-only and summarize them.
Do not rerun jobs.
Do not fix.
Do not copy long logs.
If logs may contain secrets or private data, summarize only job, step, and error type.

### SECURITY-LEAK-GO

Read:
- `references/00-review-rules.md`
- `references/03-noise-filter.md`

Only inspect security and leak risks in the diff read-only.
Do not output secrets.
If a likely secret appears, describe only path, secret type, and risk. Do not repeat the value.
On a real secret suspicion, stop before further output can expose the value.

### ADDRESS-PLAN-GO

Read:
- `references/00-review-rules.md`
- `references/04-publish-and-address.md`

Only plan how already named findings could be addressed.
Do not edit files.
Do not run tests.
Do not commit.

### PUBLISH-REVIEW-GO

Read:
- `references/01-output-format.md` or `references/02-deep-review.md`
- `references/04-publish-and-address.md`

Execute only the exact named write action, for example:

```text
PUBLISH-REVIEW-GO: top-level comment only
PUBLISH-REVIEW-GO: submit COMMENT review only
PUBLISH-REVIEW-GO: request changes only
PUBLISH-REVIEW-GO: approve only
```

Before publish:
- re-check the review text against the output rules
- no private data
- no unproven severity inflation
- no unapproved extra actions
- re-check target PR, repository, and head SHA/current commit
- approve only if the approved review text contains no blocking findings
- request changes only if the approved review text clearly proves blocking findings

## Review Stance

- Findings first.
- Real bugs before style.
- Evidence before claims.
- Threat model before severity.
- Do not invent concerns.
- If the PR looks solid, say so briefly.
- If no findings are found, say so clearly.

## References

- `references/00-review-rules.md`: review stance, evidence, severity
- `references/01-output-format.md`: short findings-first output
- `references/02-deep-review.md`: longer review report
- `references/03-noise-filter.md`: what not to report
- `references/04-publish-and-address.md`: publish and address-plan gates
