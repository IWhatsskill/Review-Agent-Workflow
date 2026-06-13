# Publish And Address

This file applies only when the operator gives ADDRESS-PLAN-GO or PUBLISH-REVIEW-GO.

## ADDRESS-PLAN-GO

Plan only. No patch.
Plan only for already named findings.
Do not invent new findings during address planning.

Output:

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
- no files changed
- no tests run
- no GitHub/tracker write
Next GO:
```

## PUBLISH-REVIEW-GO

Only the exact allowed action.

Before publish, check:
- Is the text publicly safe?
- No private data?
- No unproven severity?
- No hidden extra action?
- Is the mode correct: comment, review, approve, or request changes?
- Do target repo/PR/review thread and head SHA/current commit match?
- Was this exact text approved for this exact PR state?

Allowed examples:

```text
PUBLISH-REVIEW-GO: post top-level comment only
PUBLISH-REVIEW-GO: submit COMMENT review only
PUBLISH-REVIEW-GO: submit REQUEST_CHANGES review only
PUBLISH-REVIEW-GO: submit APPROVE review only
```

Approve rule:
- Approve only if the approved review text contains no blocking findings.
- Do not "approve with comments" unless exactly allowed.

Request-changes rule:
- Request changes only if at least one blocking finding with evidence is in the approved text.
- Do not escalate to request changes for style, unproven hypotheses, or mere test wishes.

Top-level comment:
- Do not claim inline-specific line context when the comment is top-level.
- Do not cite private sources or unpublished notes.

Not allowed without its own GO:
- labels
- assignees
- reviewer requests
- CI rerun
- merge
- auto-merge
- draft/ready toggle
- branch update
- commit/push

After publish:

```text
Phase: PUBLISH-REVIEW
Action:
Target:
Posted:
Link/ID:
Not done:
Next GO:
```
