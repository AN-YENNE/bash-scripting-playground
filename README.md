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
Here is a **simple, plain-text, well-explained version** you can copy directly into your README:

---

## Bash Expansions and Substitutions

Bash performs expansions before running commands. These expansions replace symbols, patterns, or sequences with their evaluated values. This makes Bash flexible and powerful for generating file names, sequences, and paths.

### 1. Tilde Expansion (`~`)

The tilde expands to your home directory.

Example:

```
echo ~
/home/codespace
```

Check the current user:

```
whoami
codespace
```

Bash also remembers the previous working directory.
`~-` expands to the directory you were in before the current one.

Example:

```
cd ..
cd dir/
echo ~-
/workspaces/bash-scripting-playground/Files
```

---

### 2. Brace Expansion

Brace expansion creates sequences or sets of strings. It does **not** depend on existing files; it simply generates text.

#### Number sequences:

```
echo {1..10}
1 2 3 4 5 6 7 8 9 10
```

Reverse order:

```
echo {10..1}
10 9 8 7 6 5 4 3 2 1
```

Zero-padded numbers:

```
echo {01..20}
01 02 03 04 05 06 07 08 09 10 11 12 13 14 15 16 17 18 19 20
```

#### Alphabet sequences:

```
echo {a..z}
a b c d e f g h i j k l m n o p q r s t u v w x y z
```

Uppercase reverse:

```
echo {Z..A}
Z Y X W V U T S R Q P O N M L K J I H G F E D C B A
```

#### Step values:

```
echo {1..30..3}
1 4 7 10 13 16 19 22 25 28
```

```
echo {a..z..2}
a c e g i k m o q s u w y
```

#### Multiple braces (combinations):

You can generate many filenames at once:

```
touch file{1..5}{a..d}
```

This creates:

* file1a, file1b, file1c, file1d
* file2a, file2b, file2c, file2d
* file3a …
* file4a …
* file5a …

Total of 20 files.

---

## Parameter Expansion

Parameter expansion allows you to access and manipulate the value of variables in Bash. It is one of the most useful features for string handling in shell scripts.

### Basic expansion

Assign a value:

```
a="Hello World"
```

Display the value:

```
echo $a
Hello World
```

Using braces (recommended for clarity):

```
echo ${a}
Hello World
```

### Substring extraction

Format:
`${variable:offset:length}`
Offsets start at 0.

Example:

```
echo ${a:1:9}
ello worl
```

This extracts 9 characters starting from index 1.

### String replacement

Replace first match:

```
echo ${a/World/Everybody}
Hello Everybody
```

Using a variable:

```
greeting="Hello World"
echo ${greeting/World/Friend}
Hello Friend
```

The original variable is unchanged:

```
echo ${greeting}
Hello World
```

Replace **all** occurrences:

```
echo ${greeting//e/a}
Hallo World
```

Replace **first** occurrence only:

```
echo ${greeting/e/a}
Hallo World
```

### Common mistake example

If you forget braces when using substring syntax:

```
echo $greeting:4:3
```

This outputs:

```
Hello World:4:3
```

because Bash treats `:4:3` as literal text, not part of the expansion.
Correct usage:

```
echo ${greeting:4:3}
```

---
Here is a **simple, plain-text, well-explained version** you can copy directly into your README:

---

## Command Substitution

Command substitution allows you to store or embed the **output of a command** inside another command.
This is useful when you need dynamic values, such as system info, dates, or processed command output.

The modern and recommended syntax is:

```
$(command)
```

### Basic example

```
uname -r
6.8.0-1030-azure
```

Use command substitution inside a string:

```
echo "The kernel is $(uname -r)"
```

Output:

```
The kernel is 6.8.0-1030-azure
```

### Using other commands

```
echo "The Python version is $(python3 -V)"
```

Output:

```
The Python version is Python 3.12.1
```

### Passing command output through a pipeline

Command substitution works with pipes as well:

```
echo "Result: $(python3 -c 'print("Hello from Python!")' | tr [a-z] [A-Z])"
```

This runs Python, prints a string, converts it to uppercase using `tr`, and substitutes the final result.

Output:

```
Result: HELLO FROM PYTHON!
```

---
Here is a **simple, plain-text, well-explained version** you can paste directly into your README:

---

## Arithmetic Expansion

Arithmetic expansion allows you to perform calculations directly inside Bash.
The syntax is:

```
$(( expression ))
```

Bash supports integer arithmetic (no decimals), including addition, subtraction, multiplication, division, and more.

### Examples

Addition:

```
echo $((2+2))
4
```

Subtraction:

```
echo $((4-2))
2
```

Multiplication:

```
echo $((4*5))
20
```

Integer division:

```
echo $((4/5))
0
```

Because Bash uses **integer math**, `4/5` results in `0` (no decimals).

---

Here is a **simple, plain-text, well-explained version** you can paste into your README:

---

## Creating a Bash Script File

You can create and run your own Bash scripts directly from the terminal.

### 1. Create a script file

Use any text editor (e.g., `nano`) to create a file:

```
nano Hello.sh
```

Inside the file, add something like:

```
#!/bin/bash
echo "Hello!"
```

Save and exit the editor.

### 2. Run the script using Bash

You can execute the script by explicitly calling `bash`:

```
bash ./Hello.sh
```

Example output:

```
Hello!
```

### 3. Make the script executable

To run the script directly, first give it execute permission:

```
chmod +x ./Hello.sh
```

### 4. Run the script directly

Now you can run it like any other command:

```
./Hello.sh
```

Output:

```
Hello!
```

---
Here is a **clean, plain-text, well-explained version** you can paste directly into your README:

---

## Printing Text With `echo`

The `echo` command is used to print text, variables, and command output in Bash. It supports quoting, escaping, command substitution, and options like `-n`.

### Printing variables

```
worldsize=big
echo hello $worldsize world
```

Output:

```
hello big world
```

### Printing command substitution

```
echo "Kernel is $(uname -r)"
```

or without quotes:

```
echo Kernel is $(uname -r)
```

Both output:

```
Kernel is 6.8.0-1030-azure
```

### Parentheses and escaping

Parentheses have meaning in Bash, so unescaped parentheses cause errors:

```
echo (Kernel) is $(uname -r)
```

Result:

```
bash: syntax error near unexpected token `Kernel'
```

Escape them to print literally:

```
echo \(Kernel\) is $(uname -r)
```

Output:

```
(Kernel) is 6.8.0-1030-azure
```

### Single quotes vs double quotes

Single quotes prevent expansions:

```
echo '(Kernel) is $(uname -r)'
```

Output:

```
(Kernel) is $(uname -r)
```

Double quotes allow expansions:

```
echo "(Kernel) is $(uname -r)"
```

Output:

```
(Kernel) is 6.8.0-1030-azure
```

Escape `$` to prevent substitution:

```
echo "(Kernel) is \$(uname -r)"
```

Output:

```
(Kernel) is $(uname -r)
```

### Blank lines and multiple commands

`echo` with no arguments prints an empty line:

```
echo
```

Multiple echo commands can be separated with `;`:

```
echo; echo Hi; echo
```

Output:

```

Hi

```

### No newline (`-n`)

The `-n` option prevents `echo` from adding a newline at the end:

```
echo -n "No new line"
```

Output stays on the same line:

```
No new line
```

Combining multiple `echo -n`:

```
echo -n "Part of"; echo -n "Statement"
```

Output:

```
Part ofStatement
```

---
