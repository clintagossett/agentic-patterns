---
name: bs-filter
description: Launch a skeptical reviewer agent that tears apart a draft. Flags unsupported claims, overstated framing, filler, missing counterarguments, and hype language. Produces a structured report with a ship/no-ship verdict.
invocation_pattern: When the user says "bs filter", "no-bs review", "review for bs", or asks for a critical/honest review of a draft.
---

# BS Filter

Launch an independent agent to critically review a draft with zero charitable interpretation.

## When to Use This Skill

Use this skill when:
- You want a brutally honest review of a draft
- User says "bs filter" or "no-bs review"
- User asks for a critical review before publishing
- After major revisions, to catch new issues introduced by edits

## How It Works

1. **Reads the draft** end-to-end
2. **Reads project QA standards** if they exist (e.g., `workflow/qa-process-and-standards.md`)
3. **Evaluates every claim** against what the draft actually demonstrates
4. **Flags issues** by category and severity
5. **Produces a report** with specific suggestions and a verdict

## Usage

```
/bs-filter <path-to-draft> [--focus area]
```

- `<path-to-draft>`: Path to the markdown draft file
- `--focus`: Optional focus area (e.g., `--focus claims`, `--focus tone`, `--focus structure`)

## Issue Types

| Severity | Issue | Description |
|----------|-------|-------------|
| Critical | `UNSUPPORTED_CLAIM` | Assertion with no backing example or evidence in the draft |
| Critical | `MISSING_COUNTERARGUMENT` | Obvious objection a reader would raise that isn't addressed |
| Warning | `OVERSTATED_FRAMING` | Language that oversells what's actually demonstrated |
| Warning | `FILLER` | Section or paragraph that doesn't earn its place |
| Warning | `AUDIENCE_MISMATCH` | Content pitched at wrong level for target audience |
| Info | `HYPE_LANGUAGE` | Words or phrases that sound like AI marketing copy |

## What Makes This Different From a Normal Review

A normal review asks "is this good?" The BS filter asks "why should I believe this?" Every claim gets challenged. Every example gets tested against what it actually proves. The agent assumes the reader is skeptical and technically literate.

## Skill Implementation

When invoked, this skill launches the agent defined in `agent/agent.md` as an independent task.

## Directory Structure

```
.claude/skills/bs-filter/
├── SKILL.md          # This file
└── agent/
    └── agent.md      # Agent instructions
```
