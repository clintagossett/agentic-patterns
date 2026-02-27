# tn Function (tmux New Session)

`tn` creates a new tmux session with optional path and startup commands. Pairs with the [`ta` alias](ta-alias.md) for a full session management workflow.

The function body is the same on Mac and Ubuntu — only the `ta` autocomplete wiring differs (see [ta-alias](ta-alias.md)).

## Mac (zsh) — add to `~/.zshrc`

```bash
tn() {
    local session_name="$1"; shift
    local target_path="" commands=() attach=false
    while [ $# -gt 0 ]; do
        case "$1" in
            -p) target_path="$2"; shift 2 ;;
            -c) commands+=("$2"); shift 2 ;;
            -a|--attach) attach=true; shift ;;
        esac
    done
    if [ -n "$target_path" ]; then
        tmux new-session -d -s "$session_name" -c "$target_path"
    else
        tmux new-session -d -s "$session_name"
    fi
    for cmd in "${commands[@]}"; do
        tmux send-keys -t "$session_name" "$cmd"
        tmux send-keys -t "$session_name" C-m
    done
    if [ "$attach" = true ]; then
        tmux attach -t "$session_name"
    else
        echo "Session '$session_name' created"
    fi
}
```

## Ubuntu (bash) — add to `~/.bashrc`

```bash
tn() {
    local session_name="$1"; shift
    local target_path="" commands=() attach=false
    while [ $# -gt 0 ]; do
        case "$1" in
            -p) target_path="$2"; shift 2 ;;
            -c) commands+=("$2"); shift 2 ;;
            -a|--attach) attach=true; shift ;;
        esac
    done
    if [ -n "$target_path" ]; then
        tmux new-session -d -s "$session_name" -c "$target_path"
    else
        tmux new-session -d -s "$session_name"
    fi
    for cmd in "${commands[@]}"; do
        tmux send-keys -t "$session_name" "$cmd"
        tmux send-keys -t "$session_name" C-m
    done
    if [ "$attach" = true ]; then
        tmux attach -t "$session_name"
    else
        echo "Session '$session_name' created"
    fi
}
```

## Usage

```bash
tn my-session                                   # create session in current directory
tn my-session -p ~/projects/my-app              # create in specific directory
tn my-session -p ~/projects/my-app -a           # create and attach immediately
tn my-session -p ~/projects/my-app -c "claude"  # create, run a command, stay detached
```

Flags:
- `-p <path>`: starting directory for the session
- `-c <cmd>`: command to run after creation (repeatable)
- `-a` / `--attach`: attach to the session immediately after creating it
