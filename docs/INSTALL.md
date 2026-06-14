# Install

Install the Review Agent Workflow skill from GitHub repo `IWhatsskill/Review-Agent-Workflow`.

## For All Local Agents

```bash
openclaw skills install git:IWhatsskill/Review-Agent-Workflow@main --global
```

## For One Specific Agent Or Workspace Only

1. Switch to that agent's workspace.
2. Install without `--global`.

```bash
openclaw skills install git:IWhatsskill/Review-Agent-Workflow@main
```

Then start a new session so the skill snapshot reloads.
