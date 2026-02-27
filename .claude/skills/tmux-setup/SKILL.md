---
name: tmux-setup
description: Create and configure tmux sessions for multi-agent workflows. Sets up named sessions, launches Claude Code in each, and sends initial prompts. Use when the user says "set up agents", "create agent sessions", or "start my agents".
invocation_pattern: When the user requests to set up, create, or initialize tmux sessions for multiple agents.
---

# tmux Setup

Create and configure tmux sessions for multi-agent workflows.

## When to Use This Skill

Use this skill when:
- User says "set up agents" or "start my agents"
- User wants to create multiple tmux sessions for different agents
- User asks to initialize a multi-agent workflow
- At the start of a work session with multiple agents

## How It Works

1. **Reads agent config** from a config file or user instructions
2. **Creates tmux sessions** with named sessions for each agent
3. **Launches Claude Code** in each session (handling the CLAUDECODE env var)
4. **Sends initial prompts** to each agent via tmux send-keys
5. **Reports status** of all sessions

## Usage

```
/tmux-setup [config-path] [--agents agent1,agent2,...]
```

- `[config-path]`: Optional path to an agent config file (see format below)
- `--agents`: Comma-separated list of agent names to set up (overrides config)

## Agent Config Format

Create a `tmux-agents.yml` or `tmux-agents.md` in your project root:

```yaml
agents:
  - name: james-agent
    directory: ~/projects/my-app
    prompt: "You are James, a coding agent. Read CLAUDE.md and begin working on your assigned tasks."

  - name: review-agent
    directory: ~/projects/my-app
    prompt: "You are a review agent. Read .claude/skills/bs-filter/agent/agent.md for your instructions. Wait for drafts to review."

  - name: marketing-agent
    directory: ~/projects/marketing-repo
    prompt: "You are the marketing agent. Read _index.md to understand the repo structure and check for pending tasks."
```

## What It Does

For each agent in the config:

1. Creates a tmux session: `tmux new-session -d -s <name> -x 220 -y 50`
2. Changes to the agent's working directory
3. Unsets the CLAUDECODE env var (required for nested sessions)
4. Launches Claude Code: `unset CLAUDECODE && claude`
5. Waits for Claude Code to start
6. Sends the initial prompt using the correct two-command pattern:
   ```bash
   tmux send-keys -t <name> -l "<prompt>"
   tmux send-keys -t <name> C-m
   ```

## Important: send-keys Pattern

Always send text and Enter as two separate commands. Never chain with `&&`. Never append `Enter` to the same send-keys call. See the [tmux-agent-communication snippet](../../snippets/tmux-agent-communication.md) for details.

## Checking Status

After setup, verify all sessions are running:

```bash
tmux list-sessions
```

Attach to any agent:

```bash
tmux attach -t <agent-name>
```

Detach without stopping: `Ctrl+B, D`

## Skill Implementation

When invoked, this skill launches the agent defined in `agent/agent.md`.

## Directory Structure

```
.claude/skills/tmux-setup/
├── SKILL.md          # This file
└── agent/
    └── agent.md      # Agent instructions
```
