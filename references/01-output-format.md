# Output Format: REVIEW-GO

Findings-first review: findings first, concise, concrete.

## When Findings Exist

```text
Findings

[P1] Short title
File/line: path/to/file.ts:123
Problem:
Why this is a bug/risk:
Trigger/impact:
Evidence:
Suggestion:

[P2] Short title
File/line:
Problem:
Why this is a bug/risk:
Trigger/impact:
Evidence:
Suggestion:

Open questions:
- ...

Residual risk/test gaps:
- ...

Not done:
- no patch
- no GitHub/tracker write
```

Rules:
- Highest severity first.
- No long summary before findings.
- File/line should be as exact as possible.
- If no exact line is available, name file plus function/block.
- Suggestion stays a suggestion, not a patch.
- Keep evidence short: diff line, code path, test signal, or clear runtime chain.
- Findings must be merge-relevant.

## When No Findings Exist

```text
Findings: No evidence-backed issues found.

Residual risk/test gaps:
- ...

Not done:
- no patch
- no GitHub/tracker write
```

## Inline Review Style

If the operator later permits publishing, findings should be short enough to work as review comments.

Inline comments:
- one problem per comment
- no private data
- no severity tag unless useful
- no long meta explanation

Good comment:

```text
This can leave stale session overrides active after the primary changes, so normal turns may keep using the fallback model indefinitely. The reset path should run before the stored override is reused.
```

Bad comment:

```text
This code seems suspicious and might have edge cases.
```

## Do Not Report Findings For

- pure style preference
- hypothetical micro-optimizations
- "could be cleaner"
- missing tests without clear risk
- existing debt not worsened by the PR
- out-of-scope issues unless they are acute merge blockers

## Review Text Before Publish

Before `PUBLISH-REVIEW-GO`, check:
- no private data
- no secret value
- no unproven P0/P1
- no unapproved extra task
- comment is directly useful to maintainers
