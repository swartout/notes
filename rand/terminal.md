---
layout: default
title: Terminal Stuff
parent: Rand
---

# Terminal Stuff

---

## TMUX

**TMUX Cheat Sheet**

I setup TMUX to work with nvim using [vim-tmux-navigator](https://github.com/christoomey/vim-tmux-navigator), so my defaults for moving left, right, up, and down are vim defaults.

- Start tmux using `tmux`
- Think of windows as "screens" and panes as tabs
- Start tmux with `name` using `tmux new -s name`
- List tmux sessions: `tmux ls`
- Attach to a session: `tmux a #`
- Attach to a named session: `tmux a -t name`
- Kill a session: `tmux kill-session -t name`
- Kill tmux server: `tmux kill-server`
- Use `Ctrl+hjkl` to switch between panes
- Press `Ctrl-a` to prefix tmux keyboard shortcuts (remapped)
- `%` to split screen horizontally
- `"` to split screen vertically
- `d` to detach (leaving it running in background)
- `x` to kill current pane
- `?` to list all keybindings
- `{` move current pane left
- `}` move current pane right
- `o` go to next pane
- `z` toggle full-screen mode for current pane

References: [TMUX Guide](https://tmuxguide.readthedocs.io/en/latest/tmux/tmux.html)
