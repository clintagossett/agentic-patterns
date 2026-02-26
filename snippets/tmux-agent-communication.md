# Tmux Agent Communication

Add this to your `CLAUDE.md` to ensure agents use the correct `tmux send-keys` pattern.

## The Problem

Claude Code sends `tmux send-keys -t session "message" Enter` as a single command. The `Enter` key gets eaten and the message never submits. Chaining with `&&` is also unreliable; long messages cause the Enter to get eaten even when chained.

## The Fix

Add this to your project or global `CLAUDE.md`:

```markdown
## Tmux Agent Communication

When sending messages to other agents via tmux, **always** send the text and Enter as two separate commands:

\`\`\`bash
tmux send-keys -t <session> -l "your message here"
tmux send-keys -t <session> C-m
\`\`\`

**Rules:**
- `-l` flag sends text literally (prevents tmux from interpreting key names in the message)
- `C-m` (Ctrl+M = Enter) must be a **separate command** on its own line
- Do NOT chain with `&&`: long messages can cause the Enter to get eaten even when chained
- Do NOT append `Enter` to the same `send-keys` call

**Do NOT do this:**
\`\`\`bash
# WRONG - Enter often doesn't register
tmux send-keys -t agent "message" Enter
tmux send-keys -t agent -l "message" Enter
# WRONG - && chaining is unreliable with long messages
tmux send-keys -t agent -l "message" && tmux send-keys -t agent C-m
\`\`\`
```

## Why This Happens

When Claude Code executes a bash command containing `Enter` as a tmux key name, the key event gets consumed by the shell layer before tmux processes it. Chaining with `&&` appears to work on short messages but fails unpredictably on longer ones. Sending `C-m` (the control character equivalent of Enter) as a completely separate command ensures it reaches the target session reliably.
