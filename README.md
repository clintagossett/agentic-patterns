# Claude Code Skills

Skills and snippets for multi-agent AI development with [Claude Code](https://claude.ai/code).

## What's Here

### Skills

Skills are reusable capabilities you drop into `.claude/skills/` in your project.

| Skill | Description |
|-------|-------------|
| [update-indexes](.claude/skills/update-indexes/) | Validate and maintain `_index.md` directory manifests that help agents navigate repos efficiently |

### Snippets

Snippets are `CLAUDE.md` additions that fix common agent behavior issues.

| Snippet | Description |
|---------|-------------|
| [index-navigation](snippets/index-navigation.md) | Teach agents to navigate repos using `_index.md` manifests |
| [tmux-agent-communication](snippets/tmux-agent-communication.md) | Fix `tmux send-keys` so Enter actually submits |

### Docs

| Document | Description |
|----------|-------------|
| [_index.md File Specification](docs/index-file-specification.md) | How to structure directory manifests for agent-friendly navigation |

## Installation

### update-indexes skill

Copy the skill directory into your project:

```bash
cp -r .claude/skills/update-indexes /path/to/your/project/.claude/skills/
cp docs/index-file-specification.md /path/to/your/project/docs/
```

Then use it:
```
/update-indexes --mode audit
/update-indexes --mode fix
```

### CLAUDE.md snippets

Copy the markdown blocks from these files into your project's `CLAUDE.md` or `~/.claude/CLAUDE.md`:

- **[index-navigation](snippets/index-navigation.md)** - Teaches agents to read `_index.md` first, navigate to relevant files, and keep indexes updated
- **[tmux-agent-communication](snippets/tmux-agent-communication.md)** - Fixes the `send-keys Enter` problem so agents can communicate via tmux

## Context

These tools support the multi-agent workflow described in [tmux: Local AI Agent Sessions That Outlive Your Attention](https://clintgossett.substack.com/).
