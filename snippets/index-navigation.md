# Index Navigation

Add this to your `CLAUDE.md` so agents know how to navigate a repo using `_index.md` files.

## The Problem

Without guidance, agents will either read every file (blowing context) or guess which files matter (missing important ones). The `_index.md` pattern gives agents a map, but they need to know to use it.

## The Snippet

Add this to your project's `CLAUDE.md`:

```markdown
## Navigating Documentation

This project uses `_index.md` files as directory manifests. Follow these rules when reading docs or any file directory that contains an `_index.md`:

1. **Start with the index** - Always read `_index.md` first when entering a directory
2. **Navigate, don't load** - Use the index to find relevant files; don't read everything
3. **Follow the hierarchy** - Root `_index.md` points to section indexes, which point to files
4. **Trust the manifest** - If a file isn't in the index, flag it (it may be new and unlisted)

### Reading a directory

```
# First: read the map
Read _index.md

# Then: read only the files relevant to your current task
Read specific-file.md
```

### When you create, rename, or delete files

1. Update the directory's `_index.md` to reflect the change
2. Add new entries with a one-line description and type (reference/guide/tutorial)
3. Remove entries for deleted files
4. Update the "Last updated" date

### What NOT to do

- Don't read all `.md` files in a directory to "understand the project"
- Don't skip the `_index.md` and go straight to files
- Don't leave an `_index.md` stale after adding or removing files
```

## How It Works

The `_index.md` file acts as a table of contents for its directory. Each entry has a description and type so the agent can decide whether to read the full file. This keeps context consumption low while giving the agent full navigational awareness.

A typical directory looks like:

```
docs/
├── _index.md              # Agent reads this first
├── authentication.md      # Agent reads only if relevant
├── rate-limiting.md       # Agent reads only if relevant
└── architecture/
    ├── _index.md          # Agent reads this to go deeper
    └── decisions/
        ├── _index.md
        └── adr-001.md
```

The agent loads ~200 tokens per index instead of thousands per full document. Across a large repo, this adds up fast.
