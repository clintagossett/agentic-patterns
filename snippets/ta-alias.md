# ta Alias (tmux Attach with Tab Completion)

Add this to your shell config to get a short `ta <session>` command with tab completion for session names.

## Mac (zsh)

Add to `~/.zshrc`:

```bash
alias ta='tmux attach -t'
_tmux_complete_session() {
    compadd $(tmux list-sessions -F '#{session_name}' 2>/dev/null)
}
compdef _tmux_complete_session ta
```

## Ubuntu (bash)

Add to `~/.bashrc`:

```bash
alias ta='tmux attach -t'
_tmux_complete_session() {
    local cur="${COMP_WORDS[COMP_CWORD]}"
    local sessions
    sessions=$(tmux list-sessions -F '#{session_name}' 2>/dev/null)
    COMPREPLY=($(compgen -W "$sessions" -- "$cur"))
}
complete -F _tmux_complete_session ta
```

## Usage

```bash
ta <Tab>        # lists all running tmux sessions
ta james-agent  # attaches to the james-agent session
```

Detach without stopping the session: `Ctrl+B, D`
