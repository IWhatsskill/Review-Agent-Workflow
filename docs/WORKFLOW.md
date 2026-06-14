# Review Agent Workflow

Manual review-only workflow for pull request and diff review agents.

[Who this is for](#who-this-is-for) | [Core rule](#core-rule) | [Invoke](#how-to-invoke-the-skill) | [Review phases](#review-phases) | [Quality rules](#review-quality-rules) | [Files](#included-files)

This skill helps an agent inspect PRs, diffs, CI signals, and security or leak risks without automatically fixing, testing, commenting, approving, or requesting changes. It is designed for review-first work: look, reason, report, then wait for the operator to decide.

## At A Glance

- Review-only workflow for PRs, diffs, CI signals, and leak checks.
- Findings first, summaries second.
- No patches, comments, approvals, or requested changes without explicit GO.
- Supports quick reviews, deep reviews, CI read-only checks, and security/leak review.
- Keeps review agents useful without turning them into autopilot fixers.

## Who This Is For

Use this skill when an agent should review a change and provide decision-ready findings without taking action on its own.

Good use cases:

- Review a pull request or diff for bugs and regressions.
- Produce a short findings-first review.
- Produce a deeper maintainer-oriented review report.
- Read CI results or logs in a limited read-only scope.
- Check a diff for secret, auth, security, or leak risks.
- Prepare an address plan for already accepted findings.
- Publish an approved review only when the operator explicitly allows it.

## Core Rule

Review phases are read-only unless the operator gives an exact publish or address-plan GO.

The skill never authorizes automatic fixes.

Without explicit GO, the agent must not:

- read PRs or files
- run commands
- run tests or builds
- edit files
- commit or push
- comment on a tracker
- submit a review
- approve or request changes

A review recommendation is not permission to publish it.

## How To Invoke The Skill

Use the skill name from `SKILL.md`:

```text
Use review-agent-workflow. Phase 0.
Use review-agent-workflow. REVIEW-GO for <PR or diff>.
Use review-agent-workflow. DEEP-REVIEW-GO for <PR>.
Use review-agent-workflow. CI-READ-GO for <PR or run>.
Use review-agent-workflow. SECURITY-LEAK-GO for <PR or diff>.
Use review-agent-workflow. ADDRESS-PLAN-GO for finding <id>.
Use review-agent-workflow. PUBLISH-REVIEW-GO: submit COMMENT review only.
```

The display folder is `Review Agent Workflow/`. The skill invocation name is `review-agent-workflow`.

## Phase Guide

### Phase 0: STARTTEST

Purpose: wake the agent and confirm review-only safety mode.

Use when:

- starting a fresh review agent
- checking whether the skill is installed and understood
- recovering after context loss

Allowed:

- read `SKILL.md`
- read the review rules if needed
- report what is forbidden without further GO

Not allowed:

- PR or file reads
- commands
- tests
- patches
- comments
- review submission

Example:

```text
Use review-agent-workflow. Phase 0.
```

### Phase 1: REVIEW-GO

Purpose: produce a concise findings-first review for a PR, diff, patch, or changed-file set.

Use when:

- the operator wants a normal code review
- the target is clear
- the expected output should prioritize actionable bugs and risks

Allowed:

- inspect the named PR, diff, or changed files read-only
- inspect directly affected code and test paths
- inspect relevant project rules when they affect the review
- summarize directly available CI status read-only
- prioritize evidence-backed findings

Not allowed:

- patches or file edits
- tests or builds without separate GO
- tracker, GitHub, or GitLab writes
- comments or review submission
- approve or request changes

Expected output:

- findings first, highest severity first
- exact file/line where possible
- problem, impact, evidence, and suggestion
- residual risks or test gaps
- clear `No evidence-backed issues found` when appropriate

### Phase 2: DEEP-REVIEW-GO

Purpose: produce a longer maintainer-oriented review report.

Use when:

- the PR is large or high risk
- the operator wants merge readiness, proof quality, and maintainer options
- compatibility, migration, security, or runtime behavior needs deeper evaluation

Allowed:

- evaluate PR surface, reproducibility, risk, proof quality, and maintainer options
- build an evidence list
- classify security and compatibility risks
- read directly relevant older or adjacent PRs or issues only when named or linked by the PR/operator context

Not allowed:

- patches
- tracker, GitHub, or GitLab writes
- repair work
- CI reruns

Expected output may include:

- verdict
- PR surface
- reproducibility
- merge readiness
- risk before merge
- maintainer options
- evidence reviewed
- security/leak check
- confidence

### Phase 3: CI-READ-GO

Purpose: read CI checks or logs in a limited read-only scope.

Use when:

- a PR has failing checks
- the operator wants a summary of CI status or failure cause
- the agent should not fix anything yet

Allowed:

- inspect named CI checks, jobs, steps, or logs read-only
- summarize job, step, error type, and likely cause

Not allowed:

- rerun jobs
- fix code
- copy long logs
- expose private data or secret values from logs

If logs may contain secrets or private data, summarize only the job, step, and error type.

### Phase 4: SECURITY-LEAK-GO

Purpose: review security and leak risk in the approved diff scope.

Use when:

- the operator wants a security-focused review
- a PR touches auth, secrets, config, logs, network calls, tokens, permissions, or user data

Allowed:

- inspect the approved diff scope read-only
- identify secret, auth, permission, or data-exposure risks
- describe the secret type and risk without repeating secret values

Not allowed:

- output secrets
- expand beyond the approved scope
- patch or test
- write comments or reviews

On real secret suspicion, stop before further output can expose the value.

### Phase 5: ADDRESS-PLAN-GO

Purpose: plan how already named findings could be addressed.

Use when:

- findings already exist
- the operator wants a fix plan but no code changes yet

Allowed:

- create a minimal address plan for already named findings
- list affected files, proof, risks, and open decisions

Not allowed:

- invent new findings during address planning
- edit files
- run tests
- commit or push
- tracker, GitHub, or GitLab writes

Expected output:

```text
Phase: ADDRESS-PLAN
Finding:
Goal:
Minimal fix:
Affected files:
Tests/proof:
Risks:
Open decisions:
Not done:
Next GO:
```

### Phase 6: PUBLISH-REVIEW-GO

Purpose: publish exactly one approved review action.

Use when:

- the operator has approved exact review text or an exact action
- the target repository, PR, review thread, and current head SHA/commit are clear

Allowed only when explicitly named:

```text
PUBLISH-REVIEW-GO: post top-level comment only
PUBLISH-REVIEW-GO: submit COMMENT review only
PUBLISH-REVIEW-GO: submit REQUEST_CHANGES review only
PUBLISH-REVIEW-GO: submit APPROVE review only
```

Before publishing, the agent must check:

- the text is publicly safe
- no private data or secret values are included
- no unproven severity inflation exists
- no hidden extra action is included
- the mode is correct: comment, review, approve, or request changes
- target repo, PR, review thread, and head SHA/current commit match
- the exact text was approved for the exact PR state

Not allowed without separate GO:

- labels
- assignees
- reviewer requests
- CI reruns
- merge or auto-merge
- draft/ready toggle
- branch update
- commit or push

## Review Quality Rules

A finding needs evidence. At least one of these should be true:

- specific diff line or file/function
- current code path
- reproduction path
- test or CI signal
- logical runtime chain with a clear trigger
- documentation, schema, or contract mismatch with a source

A useful finding answers:

- Why is this new or worsened by the PR?
- What concrete runtime, user, security, or maintainer consequence follows?
- What is the smallest useful correction or decision?

Do not report findings for:

- pure style preference
- hypothetical micro-optimizations
- missing tests without clear merge risk
- existing debt not worsened by the PR
- out-of-scope issues unless they are acute merge blockers

## Severity Guide

```text
P0 = critical: data loss, remote code execution, secret leak, auth bypass, broadly triggerable production outage.
P1 = high: clear regression, security flaw, incorrect persistence/migration behavior, frequent crash, strong compatibility impact.
P2 = medium: real bug with bounded scope, missing important guardrail, risky edge case with plausible trigger.
P3 = low: small edge case, test gap with limited impact, maintainability risk only when merge-relevant.
```

Do not raise severity because touched code looks important. Ask who can trigger it, what precondition is required, what concretely happens, and whether it is new in this PR.

## Practical Usage Pattern

1. Start with Phase 0 when waking a new review agent.
2. Use REVIEW-GO for normal findings-first review.
3. Use DEEP-REVIEW-GO for high-risk or larger PRs.
4. Use CI-READ-GO only for read-only CI/log inspection.
5. Use SECURITY-LEAK-GO only for focused security/leak review.
6. Use ADDRESS-PLAN-GO only after findings already exist.
7. Use PUBLISH-REVIEW-GO only for one exact approved publish action.

After each phase, the agent should stop and wait for the next explicit GO.

## Stop Conditions

Stop immediately if any of these appear:

- wrong PR, branch, or repo is unclear
- diff cannot be loaded
- review scope is unclear
- next useful step would require write, test, patch, or comment permission
- a possible secret value is visible
- CI/logs may contain private data or secret values
- approve/request-changes would require text not explicitly approved for publishing

## Included Files

```text
SKILL.md
agents/openai.yaml
references/00-review-rules.md
references/01-output-format.md
references/02-deep-review.md
references/03-noise-filter.md
references/04-publish-and-address.md
LICENSE
.clawhubignore
```

## License

MIT. See `LICENSE`.
