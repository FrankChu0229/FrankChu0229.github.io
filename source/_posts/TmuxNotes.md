title: Tmux Summary
date: 2017-09-23 21:33:01
tags: [linux, notes, summary, tmux]
categories: summary
description: Tmux Summary。
---

## Tmux Basic Operation

### Enter Session

- `tmux` to enter tmux and start a unnamed session
- `tmux new -s demo` to enter tmux and start a session named demo
- `tmux a` tmux attach the first session
- `tmux a -t demo` to attach the session named demo
- `tmux ls` to show all sessions.

### Show Shortcut Keys and Commands

- `tmux list-keys` to show all shortcut keys
- `tmux list-commands` to show all tmux commands
- `crtl + b + ?` to list all tmux shortcut keys, press `q` to return

### Exit Tmux

- `crtl + b + d`, detach to exit tmux, tmux is still running in background. 
- `tmux kill-session -t demo` kill session whose name is demo
- `tmux kill-server` kill all sessions.

### Other Basic Operations

- `ctrl + b + t`, to show the time.
- `crtl + b + :` to enter command mode.
- `ctrl + b + [` to enter copy mode, enter q to exit.

## Tmux Session Operation

- `ctrl + b + : new -s session_name` to create new session with name session_name
- `ctrl + b + s ` to list all sessions.
- `ctrl + b + $` to rename cuurrent session name.

## Tmux Window Operation

- `ctrl + b + c ` to new a window
- `ctrl + b + &` to close current window, need to input `y` to confirm
- `ctrl + b + 0-9` to switch window
- `ctrl + b + w` to show all windows in the current session


## Tmux Pane Operation 

- `ctrl + b + "`to add vertical pane.
- `ctrl + b + % ` to add horizontal pane
- `ctrl + b + 方向键` 上下左右切换pane
- `ctrl + b + x` close current pane, need to input `y`
- `ctrl + b + z` to maxmize current pane, and return to its original size if try again

## Reference

- [reference1](http://louiszhai.github.io/2017/09/30/tmux/)
- [reference2](http://wdxtub.com/2016/03/30/tmux-guide/)
- [reference3](https://gist.github.com/FrankChu0229/7755359af245119586d64adb7b88544d)

---

