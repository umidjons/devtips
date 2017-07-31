# Advanced navigation

`^E` - scroll the window down, cursor stays fixed
`^Y` - scroll the window up, cursor stays fixed
`^F` - scroll down one page
`^B` - scroll up one page
`H` - top of the window
`M` - middle of the window
`L` - bottom of the window

# NERDTree

`m` - list available commands

# Selection

`va"` - select all inside a quote

`dd`/`yy` - delete/yank a line
`p` - paste below
`P` - paste above

## Toggle paste mode when pasting from clipboard (prevent auto-indentation)

- `:set paste`
- paste text from clipboard, e.g: `ctrl-shift-v`
- `:set nopaste`

You can also bind a key in your `vimrc` for toggling paste mode:
```
set pastetoggle=<F3>
```

# Beautify JSON in vim

To reformat/prettify JSON data in a currently opened file execute the following command in vim:

```
:%!python -m json.tool
```

