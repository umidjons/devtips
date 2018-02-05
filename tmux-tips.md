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

# Installing the latest version of tmux

```bash
#!/usr/bin/env bash

sudo apt update

sudo apt install -y git

sudo apt install -y automake
sudo apt install -y build-essential
sudo apt install -y pkg-config
sudo apt install -y libevent-dev
sudo apt install -y libncurses5-dev

rm -fr /tmp/tmux

git clone https://github.com/tmux/tmux.git /tmp/tmux

cd /tmp/tmux

# Optional
# git checkout 2.6

sh autogen.sh

./configure && make

sudo make install

cd -

rm -fr /tmp/tmux
```

## Enable mouse

`set -g mouse on`

## Support 256 colors

`set -g default-terminal screen-256color`
