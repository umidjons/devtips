# Basics

## Windows

`^b-c` - create new window
`^b-,` - rename the window
`^b-w` - list available windows
`^b-p` - previous window
`^b-n` - next window

## Panes

`^b-%` - split vertically
`^b-"` - split horizontally

## Sessions

`tmux new -s <session name>` - create a new named session
`^b-d` - detach from the session
`^b-$` - rename a session

# Configuration options

Create configuration file:

```bash
tmux show -g | cat > ~/.tmux.conf
```

Set `set -g ` prefix to each line.

## Set alternate key

`set -g prefix2 C-a` - now `CTRL-a` can be used as alternative to `CTRL-b`
