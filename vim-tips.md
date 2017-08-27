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

## Controlling window size

`^w-` - decrease size (everything)
`^w+` - increase size (everything)
`^w>` - increase size vertically
`^w<` - decrease size vertically

## Searching a text in files

`:vimgrep`

Example: `:vimgrep /database/ ../**/*.java`

`:cn[ext]` - next result
`:cp[revious]` - previous result
`:cnf` - next result in a next file
`:cpf` - previous result in a previous file
`:cr[ewind]` - first result
`:cla[st]` - last result

# Delete around

Suppose we have this text:
```
test string with [brackets].
```

Navigate to `[brackets]` text and execute the following command:

`da[` - will delete text `[brackets]`.

You can combine it with other symbols too, for example:
```
da [
da {
da (
da `
da "
da '
```
