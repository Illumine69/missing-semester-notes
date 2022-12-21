# Lecture 4: Data Wrangling

Lec Notes: [https://missing.csail.mit.edu/2020/data-wrangling/](https://missing.csail.mit.edu/2020/data-wrangling/)

To wrangle data, we need two things: data to wrangle, and something to do with it.

- `ssh myserver journalctl | grep sshd` shows lines containing the word ‘sshd’ in the log  file
- `ssh myserver 'journalctl | grep sshd | grep "Disconnected from"' | less` : `less` gives us a “pager” that allows us to scroll up and down through the long output
- `sed` is a stream editor
- `ssh myserver journalctl | grep sshd | grep "Disconnected from" | sed 's/.*Disconnected from //'` :
- The `s` command is written in the form `s/REGEX/SUBSTITUTION/`

## **Regular expressions**

- `.` means “any single character” except newline
- `*` zero or more of the preceding match
- `+` one or more of the preceding match
- `?`zero or one of the preceding match
- `[abc]` any one character of `a`, `b`, and `c`
- `(RX1|RX2)` either something that matches `RX1` or `RX2`
- `^` the start of the line
- `$` the end of the line

Use `sed -E` because sed’s RegEx is bit weird

`| sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \[preauth\])?$/\2/'` has `\2` as capture group

Any text matched by a regex surrounded by parentheses is stored in a numbered capture group (`\2` denotes 2nd text in the parenthesis)

```bash
ssh myserver journalctl
 | grep sshd
 | grep "Disconnected from"
 | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \[preauth\])?$/\2/'
 | sort | uniq -c
 | sort -nk1,1 | tail -n10
 | awk '{print $2}' | paste -sd,
```

- `sort` sorts the input
- `uniq -c` will give only unique inputs in consecutive order along with the count
- `sort -n` will sort in numeric (instead of lexicographic) order. `-k1,1` means “sort by only the first whitespace-separated column”
- `sort -r` sorts in reverse order
- `paste`lets you combine lines (`-s`) by a given single-character delimiter (`-d`; `,`in this case)
- `awk` is a programming language that processes text streams
- `{print $2}` prints content of 2nd field of each line in the output column

```bash
| awk '$1 == 1 && $2 ~ /^c[^ ]*e$/ { print $2 }' | wc -l
# computes the number of single-use usernames that start with c and end with e 
```

$1 == 1 sets the first field of line to be equal to 1

`wc -l` counts the number of lines in the output

```bash
BEGIN { rows = 0 }
$1 == 1 && $2 ~ /^c[^ ]*e$/ { rows += $1 }
END { print rows }
```

`BEGIN` : matches the start of the input

`END` : matches the end of the input

## **Analyzing data**

- `bc` is a calculator which can do math
- `echo “1+2” | bc -l` gives `3`
- `R` is great at data analysis and plotting
- `gnuplot` is used for simple plotting
- `xargs` takes lines of input and turns them in arguments
