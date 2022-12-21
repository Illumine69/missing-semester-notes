# Lecture 2: Shell Tools and Scripting

Lec Notes: [https://missing.csail.mit.edu/2020/shell-tools/](https://missing.csail.mit.edu/2020/shell-tools/)

## `foo=bar`

   Just sets the variable foo to have value bar
   
   `$ echo foo` will return bar
    
## Cannot leave spaces in variable assignment
    
   `foo = bar` will give error
    
## `$foo` changes to the value it has
    
  `echo “$foo”` will return `bar`
  
  `echo ‘$foo’` returns `$foo`
    
## Variables to refer to arguments

- `$0` - Name of the script
- `$1` to `$9`- Arguments to the script. `$1`is the first argument and so on
- `$@` - All the arguments
- `$#` - Number of arguments
- `$?` - Return code of the previous command
- `$$` - Process identification number (PID) for the current script
- `!!` - Entire last command, including arguments
- `$_` - Last argument from the last command
    
## Exit code
    
- `true` has return code 0
- `false` has return code 1
- `false || echo "Oops, fail"` gives `Oops, fail`
- `true || echo "Will not be printed"` gives NULL
- `true && echo "Things went well"` gives `Things went well`
- `false && echo "Will not be printed"` gives NULL
- `;`- separates commands within same line
- `true ; echo "This will always run"` gives `This will always run`
- `false ; echo "This will always run"` gives `This will always run`
    
## Command substitution

- `foo=$(pwd)` will store the content of pwd in foo variable
- `<(CMD)` will execute CMD and then pass the output in a temporary file and substitute the `<()` with the file’s name. For example-
- `cat <(ls) <(ls ..)` will concatenate and display the contents of ls followed by ls ..
- If a directory has different files with different extensions like `image.png`, `example.sh`, etc. , `ls *.sh` will print all the files with .sh extension
- Given files `foo`, `foo1`, `foo2`, `foo10` and `bar`, the command `rm foo?` will delete `foo` and `foo2` whereas `rm foo*` will delete all but `bar`
        
## Using Curly Braces{}
    
    ```shell
    convert image.{png,jpg}
    # Will expand to
    convert image.png image.jpg
    
    cp /path/to/project/{foo,bar,baz}.sh /newpath
    # Will expand to
    cp /path/to/project/foo.sh /path/to/project/bar.sh /path/to/project/baz.sh /newpath
    
    # Globbing techniques can also be combined
    mv *{.py,.sh} folder
    # Will move all *.py and *.sh files
    
    mkdir foo bar
    # This creates files foo/a, foo/b, ... foo/h, bar/a, bar/b, ... bar/h
    touch {foo,bar}/{a..h}
    touch foo/x bar/y
    
    # Show differences between files in foo and bar
    diff <(ls foo) <(ls bar)
    # Outputs
    # < x
    # ---
    # > y
    ```
    
## SpellCheck
    
  Use `spellcheck` to find errors in sh/bash scripts
    
## Shebang
    
  Shebang is basically ‘`#!`’ at the beginning of script
    
  We use `#!/usr/bin/env python` to tell kernel to look for python interpreter using the `PATH` environment variable
    
## Find
    
    ```bash
    # Find all directories named src
    find . -name src -type d
    # Find all python files that have a folder named test in their path
    find . -path '*/test/*.py' -type f
    # Find all files modified in the last day
    find . -mtime -1
    # Finds all the file ending with .tmp
    find . -name "*.tmp"
    # Find all zip files with size in range 500k to 10M
    find . -size +500k -size -10M -name '*.tar.gz'
    # Delete all files with .tmp extension
    find . -name '*.tmp' -exec rm {} \;
    # Find all PNG files and convert them to JPG
    find . -name '*.png' -exec convert {} {}.jpg \;
    ```
    
## Locate
    
  Efficient way to find
   
  `locate` uses a database that is updated using `updatedb`
    
## Grep
    
  Search for contents in files

  `grep foo mcd.sh` will search for ‘foo’ in mcd.sh

  `grep -R foo .` goes through entire directory to search for ‘foo’

## History
    
- Can use up arrow to scroll through commands
- `history` will give the the past used commands
- `history | grep find` will print commands that contain the substring “find”
- Use `Ctrl+R` to perform backwards search through your history
- `fzf` fuzzily match through your history and present results in a convenient and visually pleasing manner
