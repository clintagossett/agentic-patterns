# Agentic Patterns

Reusable skills, agent definitions, and snippets for multi-agent AI development with [Claude Code](https://claude.ai/code).

## What's Here

### Skills

Skills are reusable capabilities you drop into `.claude/skills/` in your project.

| Skill | Description |
|-------|-------------|
| [update-indexes](.claude/skills/update-indexes/) | Validate and maintain `_index.md` directory manifests that help agents navigate repos efficiently |
| [bs-filter](.claude/skills/bs-filter/) | Launch a skeptical reviewer agent that tears apart drafts. Flags unsupported claims, overstated framing, filler, and hype language |
| [tmux-setup](.claude/skills/tmux-setup/) | Create and configure tmux sessions for multi-agent workflows. Launches Claude Code in each session and sends initial prompts |

### Snippets

Snippets are `CLAUDE.md` additions that fix common agent behavior issues.

| Snippet | Description |
|---------|-------------|
| [index-navigation](snippets/index-navigation.md) | Teach agents to navigate repos using `_index.md` manifests |
| [tmux-agent-communication](snippets/tmux-agent-communication.md) | Reliable agent-to-agent messaging through tmux sessions (fixes the `send-keys Enter` problem) |

### Docs

| Document | Description |
|----------|-------------|
| [_index.md File Specification](docs/index-file-specification.md) | How to structure directory manifests for agent-friendly navigation |

## Installation

### Skills

Copy a skill directory into your project:

```bash
cp -r .claude/skills/bs-filter /path/to/your/project/.claude/skills/
```

Then use it:
```
/bs-filter path/to/draft.md
```

### Snippets

Copy the markdown blocks from snippet files into your project's `CLAUDE.md` or `~/.claude/CLAUDE.md`.

## Quick Start

**"I want an honest review of my draft"**
Copy [bs-filter](.claude/skills/bs-filter/) into your project's `.claude/skills/`. Run `/bs-filter path/to/draft.md`.

**"I want to set up multiple agents"**
Copy [tmux-setup](.claude/skills/tmux-setup/) into your project's `.claude/skills/`. Create a `tmux-agents.yml` config and run `/tmux-setup`.

**"My agents lose focus after reading too many files"**
Start with [index-navigation](snippets/index-navigation.md) and the [_index.md Specification](docs/index-file-specification.md).

**"I want two agents to review each other's work"**
Start with [tmux-agent-communication](snippets/tmux-agent-communication.md) and [bs-filter](.claude/skills/bs-filter/).

## Context

These tools support the multi-agent workflow described in [tmux: Local Agent-to-Agent Sessions That Outlive Your Attention](https://clintgossett.substack.com/).
