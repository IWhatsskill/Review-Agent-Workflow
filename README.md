# Review Agent Workflow

Manual review-only workflow for pull request and diff review agents.

[Quick start](#quick-start) | [Review phases](#review-phases) | [Safety](#safety) | [Files](#files) | [Install](#install)

## Overview

`review-agent-workflow` helps an agent review PRs, diffs, CI signals, and security or leak risks without automatically fixing, testing, commenting, approving, or requesting changes.

Use it when you want findings-first review output and a clear pause before any public review action.

## Quick Start

```text
Use review-agent-workflow. Phase 0.
Use review-agent-workflow. REVIEW-GO for <PR or diff>.
Use review-agent-workflow. DEEP-REVIEW-GO for <PR>.
Use review-agent-workflow. CI-READ-GO for <PR or run>.
Use review-agent-workflow. SECURITY-LEAK-GO for <PR or diff>.
```

The skill invocation name is:

```text
review-agent-workflow
```

## Review Phases

```text
STARTTEST -> REVIEW -> DEEP REVIEW -> CI READ -> SECURITY LEAK -> ADDRESS PLAN -> PUBLISH REVIEW
```

The important rule is simple:

```text
Review is not permission to fix or publish.
```

Examples:

- `REVIEW-GO` allows reading and reporting findings only.
- `DEEP-REVIEW-GO` allows deeper read-only maintainer analysis.
- `CI-READ-GO` allows limited CI/log reading only.
- `SECURITY-LEAK-GO` allows leak-risk review without printing secret values.
- `PUBLISH-REVIEW-GO` allows only the exact approved review action.

## Safety

- No PR reads, file reads, commands, tests, patches, comments, reviews, approvals, or requested changes without the matching GO.
- Review output should be findings-first and evidence-backed.
- Do not quote secrets, tokens, private data, or long logs.
- If a likely secret appears, describe only the path, type, and risk, then stop.
- A review recommendation is not permission to publish it.

## Files

```text
SKILL.md
agents/openai.yaml
references/00-review-rules.md
references/01-output-format.md
references/02-deep-review.md
references/03-noise-filter.md
references/04-publish-and-address.md
```

## Install

Clone or copy this folder into your skills directory, then invoke the skill by name:

```text
review-agent-workflow
```

## License

MIT
