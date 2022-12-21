# Lecture 3: Editors (vim)

Lec Notes: [https://missing.csail.mit.edu/2020/editors/](https://missing.csail.mit.edu/2020/editors/)

## Modal Editing

- **Normal**: for moving around a file and making edits
- **Insert `i`**: for inserting text
- **Replace `r`**: for replacing text
- **Visual** (plain `v`, line `V`, or block `^V`): for selecting blocks of text
- **Command-line `:`**: for running a command

Use `esc` to switch from any mode to Normal mode

## Commands

- `:q` quit (close window)
- `:w` save (“write”)
- `:wq` save and quit
- `:x` same as `:wq` but exits if the buffer hasn’t changed
- `:e {name of file}` open file for editing
- `:ls` show open buffers
- `:help {topic}` open help
    - `:help :w` opens help for the `:w` command
    - `:help w` opens help for the `w` movement

## Movement

- Basic movement: `hjkl` (left, down, up, right)
- Words: `w` (next word), `b` (beginning of word), `e` (end of word)
- Lines: `0` (beginning of line), `^` (first non-blank character), `$` (end of line)
- Screen: `H` (top of screen), `M` (middle of screen), `L` (bottom of screen)
- Scroll: `Ctrl-u` (up), `Ctrl-d` (down)
- File: `gg` (beginning of file), `G` (end of file)
- Line numbers: `:{number}<CR>` or `{number}G` (line {number})
- Misc: `%` (corresponding item)
- Find: `f{character}`, `t{character}`, `F{character}`, `T{character}`
    - find/to forward/backward {character} on the current line
    - `,` / `;` for navigating matches
- Search: `/{regex}`, `n` / `N` for navigating matches

## Selection

**Visual modes**:
- Visual: `v`
- Visual Line: `V`
- Visual Block: `Ctrl-v`

Can use movement keys to make selection.

## Edits

- `i` enter Insert mode

   But for manipulating/deleting text, we want to use something more than backspace
- `o` / `O` insert line below / above
- `d{motion}` delete {motion}
    - e.g. `dw` is delete word, `d$` is delete to end of line, `d0` is delete to beginning of line
- `c{motion}` change {motion}
    - e.g. `cw` is change word
    - like `d{motion}` followed by `i`
- `x` delete character (equal do `dl`)
- `s` substitute character (equal to `cl`)
- Visual mode + manipulation
    - select text, `d` to delete it or `c` to change it
- `u` to undo, `^r` to redo
- `y` to copy / “yank” (some other commands like `d` also copy)
- `p` to paste
- Lots of more to learn: e.g. `~` flips the case of a character

## Modifiers

- `ci(` change the contents inside the current pair of parentheses
- `ci[` change the contents inside the current pair of square brackets
- `da'` delete a single-quoted string, including the surrounding single quotes
