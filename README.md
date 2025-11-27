1. bash --version
Displays the version of the Bash shell installed. Useful for checking compatibility when writing scripts.

2. echo $PATH
Shows the PATH environment variable. This variable contains a list of directories that the shell searches to find executable programs. Helpful for debugging “command not found” issues.

3. env
Prints all environment variables available in the current shell session. Useful for understanding the environment configuration, debugging issues, and verifying variable values.

---

## Pipes and Redirections

Pipes and redirections are powerful features in Bash for controlling where input comes from and where output goes. They help connect commands, process data, and manage files efficiently.

### 1. Pipes (`|`)

Pipes send the **output of one process** directly into the **input of another process**.

Example:

```
ls | wc -l
```

`ls` lists files, and `wc -l` counts the number of lines. The pipe connects them so the output of `ls` becomes the input of `wc -l`.

More examples:

```
cat file.txt
```

Displays the content of a file.

```
cat file.txt | less
```

Shows the file one page at a time using `less`.
Press `q` to exit from `less`.

```
cat file.txt | wc
```

Sends the contents of `file.txt` to `wc`, which outputs line, word, and byte counts.

---

### 2. Redirections (`>`, `>>`, `<`, etc.)

Redirections send input or output streams to/from files.
There are three standard streams:

* Standard Input (stdin) – 0
* Standard Output (stdout) – 1
* Standard Error (stderr) – 2

#### Output redirection

```
ls > list.txt
```

Creates or overwrites `list.txt` with the output of `ls`.

```
ls >> list.txt
```

Appends the output to `list.txt` instead of overwriting it.

#### Redirecting stdout and stderr separately

```
ls ./notreal 1>output.txt 2>error.txt
```

* `1>` sends normal output to `output.txt`
* `2>` sends error messages to `error.txt`

#### Input redirection

```
cat < list.txt
```

Feeds the contents of `list.txt` into the `cat` command (same result as `cat list.txt`).

---

### 3. Here Documents (Heredocs)

A heredoc sends multiple lines of text as input to a command.

Example:

```
cat << EndOfText
Hi
Hello
Welcome
EndOfText
```

Everything typed between `<< EndOfText` and the ending marker is passed to `cat`.

---

### 4. Removing leading tabs in a heredoc

Add a `-` after `<<` to allow indentation using tabs.
Tabs will be stripped automatically.

Example:

```
cat <<- EOF
    hi
    hello
    how are you
EOF
```
---

## Commands and Builtins

In Bash, many of the tools we use are **external commands**. These are separate programs installed on the system (for example: `/bin/ls`, `/usr/bin/echo`).
Bash also provides **builtins**, which are commands that are part of the Bash shell itself (for example: `echo`, `printf`, `cd`, `enable`).

### External Commands

These are standalone programs that run outside of Bash. Examples include:

```
echo some text
printf hello
```

When you run `echo`, Bash may use either its builtin version or the external command version located in `/usr/bin/echo`.

### Builtin Commands

These run inside Bash itself and do not require a separate executable file.

To check whether a command is a builtin or an external command:

```
command -V echo
```

Example output:

```
echo is a shell builtin
```

This means Bash is using its internal version of `echo` by default.

### Forcing Bash to use the external command

Bash provides the `enable` command to control builtins.

Disable the builtin version of `echo`:

```
enable -n echo
```

Check again:

```
command -V echo
```

Output:

```
echo is /usr/bin/echo
```

Now the external command (`/usr/bin/echo`) will run instead of the builtin.

Re-enable the builtin version:

```
enable echo
```

### Example command usage

```
echo some text
printf some
echo hello
command echo hello
builtin echo hi
```

### Listing all Bash builtins

```
help -d
```

This prints a list of all builtin commands available in Bash.

---
