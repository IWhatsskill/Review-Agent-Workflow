# Output Format: DEEP-REVIEW-GO

Use this mode for longer maintainer-oriented review reports.

## Structure

```text
Phase: DEEP-REVIEW
PR:
Review target:

Summary:
- Verdict:

PR surface:
- Source:
- Tests:
- Docs/config:
- Total:

Reproducibility:
- Yes/No/Unclear:
- Evidence:

Review metrics:
- ...

Merge readiness:
- Result:
- Proof:
- Patch quality:
- Blockers:

Risk before merge:
- [P?] ...

Maintainer options:
1. ...
2. ...

Evidence reviewed:
- ...

Security/leak check:
- ...

Findings:
- ...

Confidence:
- high | medium | low

Next useful step:

Not done:
- no patch
- no GitHub/tracker write
```

## What To Check

- Does the PR actually solve the problem?
- Is the fix narrow enough?
- Does it introduce a new regression?
- Do tests/proof match the risk?
- Are there compatibility or migration effects?
- Are there security, secret, auth, network, or tool-execution effects?
- Were docs/config/schemas updated when behavior is visible?
- Do old or adjacent PRs change the context?

Do not automatically check:
- unrelated issues/PRs without direct relation in the current PR/operator context
- long logs without CI-READ-GO
- secret values in plaintext

## Maintainer Options

Options should be decision-ready:

```text
1. Land as-is
2. Ask for targeted proof
3. Request a narrow fix before merge
```

Do not offer fake-balanced options when one recommendation is clearly strongest.

When a blocker is clearly proven:
- do not present "Land as-is" as an equal option
- name the smallest required follow-up

When no blockers are proven:
- do not invent P2/P3 issues from style or preference

## Proof Rating

```text
Proof: strong
Proof: sufficient
Proof: weak
Proof: missing
```

Strong:
- real behavior or production-like path
- before/after or clear repro
- relevant tests run

Sufficient:
- focused tests plus plausible runtime chain
- no large proof gap

Weak:
- source inspection only
- no affected runtime edge
- important environment missing

Missing:
- claim without evidence
- proof does not match the risk

## Deep Review Noise Guard

A long report must not create more findings than a short review.
Length is for context, evidence, and decision support, not more alarm.

Every Risk Before Merge item needs:
- affected path or diff location
- concrete trigger
- concrete impact
- why it matters before merge
