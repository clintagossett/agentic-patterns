# _index.md File Specification

`_index.md` files are directory manifests that let AI agents navigate a repo without reading every file. They reduce context consumption and help agents find what's relevant fast.

## Core Idea: Progressive Disclosure

An agent reads the `_index.md` to understand what a directory contains, then loads only the files it needs. The index is a map, not a summary.

**Layer hierarchy:**
1. **Root `_index.md`** (~200 tokens) - Lists all top-level sections
2. **Section `_index.md`** (~200-500 tokens) - Lists docs with descriptions and types
3. **Individual files** (varies) - Full content, loaded on demand

## Required Structure

```markdown
# Section Title

[1-2 sentence purpose statement: what this section covers and when to look here]

## Contents

| Document | Description | Type |
|----------|-------------|------|
| [filename.md](filename.md) | One-line description | reference/guide/tutorial |

## Sections

| Section | Description |
|---------|-------------|
| [subdirectory/](subdirectory/_index.md) | What the subdirectory covers |

---

*Last updated: YYYY-MM-DD*
```

## Field Definitions

| Field | Required | Purpose |
|-------|----------|---------|
| Title (H1) | Yes | Section name |
| Purpose statement | Yes | 1-2 sentences: what this section covers and when to use it |
| Contents table | If files exist | Documents in this directory (not subdirectories) |
| Sections table | If subdirs exist | Child directories with their own `_index.md` |
| Last updated | Yes | Date of last index update |

## Document Types

| Type | Use When |
|------|----------|
| `reference` | Describes what something is: definitions, schemas, specs |
| `guide` | Shows how to do something: step-by-step instructions |
| `tutorial` | Teaches through exercises: hands-on learning |

## Description Best Practices

- State what the document covers, not what it is
- Include keywords agents would search for
- Keep under 100 characters

**Good:** "JWT token configuration and validation rules for API authentication"
**Bad:** "A guide about JWT"

## What NOT to Include

`_index.md` should only contain the manifest structure above. No lengthy intros, no duplicated content, no history or decision records. If you find narrative content in an `_index.md`, extract it to its own file and add an entry in the Contents table.

## Naming Convention

- **File name:** `_index.md` (underscore prefix)
- **Rationale:** Borrowed from Hugo. The underscore signals "this is a manifest, not content."

## Placement

Every directory containing markdown files should have an `_index.md`.

```
project/
├── _index.md
├── getting-started/
│   ├── _index.md
│   └── setup.md
├── architecture/
│   ├── _index.md
│   └── decisions/
│       ├── _index.md
│       └── adr-001.md
```

## Maintenance

Update `_index.md` when files are added, removed, or renamed. A stale index is worse than no index: agents trust the manifest and will miss unlisted files or chase deleted ones.

## Minimal Example

```markdown
# API Documentation

API endpoint specifications and authentication patterns. Read this section when building or debugging API integrations.

## Contents

| Document | Description | Type |
|----------|-------------|------|
| [authentication.md](authentication.md) | OAuth2 and API key authentication flows | reference |
| [rate-limiting.md](rate-limiting.md) | Rate limit policies and retry strategies | reference |

---

*Last updated: 2026-02-25*
```
