# Lecture 5: Command-line Environment

Lec Notes : [https://missing.csail.mit.edu/2020/command-line/](https://missing.csail.mit.edu/2020/command-line/)

## Job Control

- `Ctrl+C` : Sends `SIGINT` signal to the process
- `Ctrl+\` : Sends `SIGQUIT` signal
- `Ctrl+Z` : Sends `SIGSTOP` signal
- `SIGINT` : Terminal interrupt signal
- `SIGKILL` : Kill (cannot be caught or ignored)
- `SIGQUIT` : Terminal quit signal
- `SIGSTOP` : Stop executing (cannot be caught or interrupted)
- `SIGTSTP` : Terminal stop signal
- `jobs` : lists unfinished jobs in the current terminal session
- `jobs $!` : refers to the last background job
- `bg(fg)` : Sends a process to continue in the background(foreground)
- `kill -STOP %1` : suspends the job at position 1

## **Terminal Multiplexers**

Terminal multiplexers allow you to multiplex terminal windows using panes and tabs so you can interact with multiple shell sessions. tmux is a popular terminal multiplexer.

**Using tmux** : Press `Ctrl+b` , release `Ctrl+b` and then press the required key bind.

- **Sessions** - a session is an independent workspace with one or more windows
    - `tmux` starts a new session
    - `tmux new -s NAME` starts it with that name
    - `tmux ls` lists the current sessions
    - Within `tmux` typing `<C-b> d` detaches the current session
    - `tmux a` attaches the last session. You can use `t` flag to specify which session to attach to
    - `tmux rename-session -t 0 database` to rename an existing session
- **Windows** - Equivalent to tabs in editors or browsers, they are visually separate parts of the same session
    - `<C-b> c` Creates a new window. To close it you can just terminate the shells doing `<C-d>`
    - `<C-b> N` Go to the *N* th window. Note they are numbered
    - `<C-b> p` Goes to the previous window
    - `<C-b> n` Goes to the next window
    - `<C-b> ,` Rename the current window
    - `<C-b> w` List current windows
- **Panes** - Like vim splits, panes let you have multiple shells in the same visual display
    - `<C-b> "` Split the current pane horizontally
    - `<C-b> %` Split the current pane vertically
    - `<C-b> <direction>` Move to the pane in the specified *direction*. Direction here means arrow keys
    - `<C-b> z` Toggle zoom for the current pane
    - `<C-b> [` Start scrollback. You can then press `<space>` to start a selection and `<enter>` to copy that selection
    - `<C-b> <space>` Cycle through pane arrangements

## **Aliases**

A shell alias lets you create a short form any command.

> Make shorthand for common flag

`alias ll="ls -lh"` 

> Save a lot of typing for common commands

`alias gs="git status"
alias gc="git commit"
alias v="vim"`

> Save you from mistyping

`alias sl=ls`

> Overwrite existing commands for better defaults

`alias mv="mv -i"`             -i prompts before overwrite

`alias mkdir="mkdir -p"`   -p make parent dirs as needed

`alias df="df -h"`              -h prints human readable format

> Alias can be composed

`alias la="ls -A"`

`alias lla="la -l"`

> To ignore an alias run it prepended with \

`\ls`

> Or disable an alias altogether with unalias

`unalias la`

> To get an alias definition just call it with alias

`alias ll`     will print ll='ls -lh'

## Dotfiles

Many programs are configured using plain-text files known as *dotfiles*(because the file names begin with a `.`, e.g. `~/.vimrc`, so that they are hidden in the directory listing `ls` by default).

## Remote Machines

- To `ssh` into a server you execute a command as follows(user foo in server) : `ssh foo@bar.mit.edu` or `ssh foo@192.168.1.42`
- ssh can run commands directly. Eg: `ssh foobar@server ls | grep PATTERN`will grep locally the remote output of `ls`
- To generate a ssh key, use `ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/id_ed25519`
- `ssh` will look into `.ssh/authorized_keys` to determine which clients it should let in. To copy a public key over you can use: `cat .ssh/id_ed25519.pub | ssh foobar@remote 'cat >> ~/.ssh/authorized_k`    or you can use `ssh-copy-id -i .ssh/id_ed25519 foobar@remote`

**Copying files over SSH:**

- `ssh+tee`  use `ssh` command execution and STDIN input by doing `cat localfile | ssh remote_server tee serverfile`. Recall that `[tee](https://www.man7.org/linux/man-pages/man1/tee.1.html)` writes the output from STDIN into a file.
- Use `[scp](https://www.man7.org/linux/man-pages/man1/scp.1.html)` when copying large amounts of files/directories. The secure copy `scp` command is more convenient since it can easily recurse over paths. The syntax is `scp path/to/local_file remote_host:path/to/remote_file`
- `[rsync](https://www.man7.org/linux/man-pages/man1/rsync.1.html)` improves upon `scp` by detecting identical files in local and remote, and preventing
copying them again. It also provides more fine grained control over symlinks, permissions and has extra features like the `-partial` flag that can resume from a previously interrupted copy. `rsync` has a similar syntax to `scp`.

**SSH Configuration**

Instead of using alias, use `~/.ssh/config` as other programs like `scp`, `rsync`, `mosh`, &c are able to read, write & convert the settings into the corresponding flags.
