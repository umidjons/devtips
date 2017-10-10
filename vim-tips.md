# Advanced navigation

`^E` - scroll the window down, cursor stays fixed
`^Y` - scroll the window up, cursor stays fixed
`^F` - scroll down one page
`^B` - scroll up one page
`H` - top of the window
`M` - middle of the window
`L` - bottom of the window

`^O` - go to Nth older position in jump list
`^I` - go to Nth newer position in jump list

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

# Delete/Change around

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

# Delete/Change inside

Suppose we have this text:
```
test string with [brackets].
```

Navigate to `[brackets]` text and execute the following command:

`ci[` - will change text `brackets` inside `[]`.

You can combine it with other symbols too, for example:
```
ci [
ci {
ci (
ci `
ci "
ci '
```

# Paste entry from buffer history

`"2p` - pastes previous entry from buffer
`"2P` - pastes previous entry from buffer into above line
`"3p` - paster 3 entry from buffer

# Insert ENTER on normal mode

Remap double ENTER to insert new line in normal mode:
`nmap <CR><CR> o<ESC>`

# Navigation by methods

```
]m      N  ]m           N times forward to start of method (for Java)
]M      N  ]M           N times forward to end of method (for Java)
```

# Search identifier

```
star    N  *            search forward for the identifier under the cursor
#       N  #            search backward for the identifier under the cursor

gd         gd           goto local declaration of identifier under the cursor
gD         gD           goto global declaration of identifier under the cursor
```

# Marks

```
m        m{a-zA-Z}      mark current position with mark {a-zA-Z}
`a       `{a-z}         go to mark {a-z} within current file
`A       `{A-Z}         go to mark {A-Z} in any file
```

# Useful insert mode commands

`i_CTRL-H` - delete the character before the cursor (backspace)
`i_CTRL-W` - delete word before the cursor
`i_CTRL-U` - delete all entered characters in the current line

`i_CTRL-T` - insert one shiftwidth of indent in front of the current line
`i_CTRL-D` - delete one shiftwidth of indent in front of the current line

`i_CTRL-E` - insert the character from below the cursor
`i_CTRL-Y` - insert the character from above the cursor

# Special inserts

`:r [file]` - insert the contents of [file] below the cursor
`:r! {command}` - insert the standard output of {command} below the cursor


