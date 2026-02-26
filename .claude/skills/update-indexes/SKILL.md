---
name: update-indexes
description: Validate and update _index.md directory manifest files. Use when the user says "update indexes", "fix indexes", "check indexes", or asks to sync documentation indexes with actual file contents. Detects missing entries, orphaned files, stale links, and format issues. Can run in audit mode (report only) or fix mode (update files).
invocation_pattern: When the user requests to validate, fix, or update _index.md files, immediately use this skill to launch the specialized agent.
---

# Update Indexes

Validate and update `_index.md` directory manifest files to keep them synchronized with actual file contents.

## When to Use This Skill

Use this skill when:
- User says "update indexes" or "fix indexes"
- User asks to "sync documentation indexes"
- User asks to "check index files" or "validate indexes"
- After adding, removing, or renaming documentation files

## How It Works

1. **Scans directories** for `_index.md` files
2. **Validates structure** against the spec in `docs/index-file-specification.md`
3. **Cross-references** index entries against actual files
4. **Detects issues**: missing files, orphaned entries, stale dates, leaked content
5. **Reports or fixes** depending on mode

## Usage

```
/update-indexes [path] [--mode audit|fix] [--verbose]
```

- `[path]`: Directory to check (default: project root)
- `--mode audit`: Report only (default)
- `--mode fix`: Automatically fix issues
- `--verbose`: Detailed output

## Issue Types

| Severity | Issue | Description |
|----------|-------|-------------|
| Critical | `MISSING_INDEX` | Directory has `.md` files but no `_index.md` |
| Critical | `ORPHANED_ENTRY` | Index references a file that doesn't exist |
| Critical | `MISSING_ENTRY` | File exists but isn't in the index |
| Warning | `MISSING_TITLE` | Index lacks H1 heading |
| Warning | `MISSING_PURPOSE` | Index lacks purpose statement |
| Warning | `STALE_DATE` | Last updated date is missing or >30 days old |
| Warning | `LEAKED_CONTENT` | Index contains content beyond manifest purpose |
| Info | `LONG_DESCRIPTION` | Description exceeds 100 characters |

## Content Preservation

If the skill finds narrative content in an `_index.md` that doesn't belong, it extracts it to a new file and adds an entry to the index. Content is never discarded.

## Skill Implementation

When invoked, this skill launches the agent defined in `agent/agent.md` as a background task.

## Directory Structure

```
.claude/skills/update-indexes/
├── SKILL.md          # This file
└── agent/
    └── agent.md      # Agent instructions
```

## Related

- [_index.md File Specification](../../../docs/index-file-specification.md)
