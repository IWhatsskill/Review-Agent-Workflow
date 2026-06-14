# Review Agent Workflow

**Manual review-only workflow for pull request and diff review agents**

This is a compact workflow skill for agents that should inspect PRs, diffs, CI signals, or leak risks without automatically fixing, commenting, approving, or requesting changes.

Use it when you want findings-first review output and clear operator control over anything that would write back to a repo or tracker.

<p align="center">
  <a href="SKILL.md"><strong>Skill file</strong></a>
  &nbsp;|&nbsp;
  <a href="docs/INSTALL.md"><strong>Install</strong></a>
  &nbsp;|&nbsp;
  <a href="docs/WORKFLOW.md"><strong>Review guide</strong></a>
  &nbsp;|&nbsp;
  <a href="references/01-output-format.md"><strong>Output format</strong></a>
  &nbsp;|&nbsp;
  <a href="references/02-deep-review.md"><strong>Deep review</strong></a>
  &nbsp;|&nbsp;
  <a href="references/04-publish-and-address.md"><strong>Publish rules</strong></a>
</p>

## What It Covers

| Area | Purpose |
| --- | --- |
| Quick and deep review | Inspect changes and report real findings without noise. |
| CI and leak checks | Read logs or diffs in a limited, read-only scope. |
| Publish control | Prepare review text, but publish only after exact operator GO. |

## Quick Start

Ask your OpenClaw agent to use the skill by name:

```text
Use review-agent-workflow. Phase 0.
Use review-agent-workflow. REVIEW-GO for <PR or diff>.
Use review-agent-workflow. DEEP-REVIEW-GO for <PR>.
```

The detailed review guide lives in [docs/WORKFLOW.md](docs/WORKFLOW.md).

## Repository Map

| Path | Purpose |
| --- | --- |
| [SKILL.md](SKILL.md) | Main skill entry file. |
| [docs/WORKFLOW.md](docs/WORKFLOW.md) | Full README-style guide with review phases, stop conditions, and quality rules. |
| [references/](references/) | Review rules, output formats, deep-review notes, and publish/address rules. |
| [agents/openai.yaml](agents/openai.yaml) | Agent metadata/config included with the skill package. |

## License

MIT. See [LICENSE](LICENSE).
