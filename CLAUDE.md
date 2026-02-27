# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

A collection of reusable **skills** and **snippets** for multi-agent Claude Code workflows. There is no build system, no tests, and no code to run — everything here is markdown-based documentation and Claude Code skill definitions.

## Structure

```
.claude/skills/       # Skills: drop into any project's .claude/skills/
  <skill>/
    SKILL.md          # Skill description, usage, invocation pattern
    agent/
      agent.md        # Agent instructions launched when the skill runs
docs/                 # Reference documentation
snippets/             # CLAUDE.md additions for common agent behavior problems
```

**Skills** are invoked with `/skill-name` in Claude Code. Each skill launches a specialized agent via the Task tool.

**Snippets** are copy-paste blocks intended for a project's `CLAUDE.md` — they fix known Claude Code behavior problems (e.g., the tmux `Enter` key drop).

## Skill Anatomy

Each skill has two parts:
- `SKILL.md`: frontmatter (`name`, `description`, `invocation_pattern`), usage docs, and issue type tables
- `agent/agent.md`: full instructions for the agent that gets launched when the skill is invoked

The frontmatter `description` field is what Claude Code uses to match user requests to skills.

## Adding a New Skill

1. Create `.claude/skills/<skill-name>/SKILL.md` with frontmatter and docs
2. Create `.claude/skills/<skill-name>/agent/agent.md` with agent instructions
3. Add a row to the Skills table in `README.md`
4. Register the skill in `.claude/settings.json` if it needs tool permissions

## _index.md Files

This repo uses `_index.md` manifests. When entering a directory that has one, read it first to find relevant files rather than reading everything.

When you add, remove, or rename a file, update the directory's `_index.md`. Run `/update-indexes` to audit or fix all indexes.
