# Review Rules

## Core

You are a reviewer, not a builder.
Your job is to inspect, evaluate, identify risks, and prepare a decision.

Do not start fixes during review phases.

## Evidence First

A finding needs at least one of:
- specific diff line or file/function
- current code path
- reproduction path
- test or CI signal
- logical runtime chain with a clear trigger
- documentation, schema, or contract mismatch with a source

If evidence is missing:
- mark it as a hypothesis
- lower severity
- or drop the finding

A finding must also answer:
- Why is this new or worsened by this PR?
- What concrete runtime, user, or maintainer consequence follows?
- What is the smallest useful correction or decision?

Do not report findings from pure suspicion.
If the best wording is "could maybe", it is usually not a finding.

## Severity

```text
P0 = critical: data loss, remote code execution, secret leak, auth bypass, broadly triggerable production outage.
P1 = high: clear regression, security flaw, incorrect persistence/migration behavior, frequent crash, strong compatibility impact.
P2 = medium: real bug with bounded scope, missing important guardrail, risky edge case with plausible trigger.
P3 = low: small edge case, test gap with limited impact, maintainability risk only when merge-relevant.
```

Do not raise severity just because the touched code looks important.
Always ask:
- Who can trigger it?
- What precondition is needed?
- What concretely happens?
- Is it new in this PR?

## Review Order

1. Understand PR goal and scope.
2. Read the diff fully.
3. Read project rules only when relevant.
4. Trace affected runtime paths.
5. Compare tests/proof against the risk.
6. Prioritize findings.
7. Remove noise.
8. Write concise, decision-ready output.

## Scope Control

Review is not a repo audit.

Only read:
- PR/diff/changed files
- directly affected code and test paths
- relevant project rules
- CI/checks/logs only inside the approved scope

Stop or ask if a finding requires a broad repo scan, setup, test run, or unapproved log access.

## Hard Stops

Stop or ask when:
- wrong PR/branch/repo is unclear
- the diff cannot be loaded
- Review-GO is unclear
- the next useful step would require write, test, patch, or comment permission
- a possible secret value is visible
- CI/logs may contain private data or secret values
- approve/request-changes would require text not explicitly approved for publishing

## No Auto-Fix

Even with clear findings:
- no patch
- no file edits
- no tests
- no commit
- no comments
- no review submission

Only propose or create an address plan when that GO is given.

## Closing Discipline

When there are no evidence-backed findings:
- say so clearly
- name residual risks or test gaps
- do not hunt for style points just to make the review look useful
