﻿# 1- Introduction to Bash scripting

### Questions
- What is a Bash script? Why do I need it?
- How do I write a Bash script?

### Objectives
- Learn to write and execute a simple script
- Extend your script with multiple commands

When we want to reuse commands, we can find older commands using the
up/down arrow keys or `history` command.

When we need to accomplish many tasks and repeat them frequently, using
arrow keys or `history` will be inefficient. It is better to save your
commands to a file. Instead of executing the commands one by one on the
command line, we can execute the file. This file is called a shell
script. If you shell is using Bash, then we can call this file a Bash
script.

Let's start with a simple command and reproduce its function the using a
script. We will use some input files from "shell-novice-data" folder.

Remember `head` command prints out a number of lines from the top of the
file and `tail` command prints out a number of lines from the bottom of
the file. Using these commands let's print out the contents of
"octane.pdb" file omitting the header.

```bash
cd /absolute/path/to/shell-novice-data/bash-scripting/molecules
head -n 12 octane.pdb
head -n 12 octane.pdb | tail -n 10
```

Now, if we want to re-use these commands, a script is the best way to
store them for future use. We will use a text editor called nano to
create our first script.

```bash
nano middle.sh
```

This will open the interface of nano and let's you write to the file. Let's
write the first `head` command in the file:

> Code
```code
    head -n 12 octane.pdb
```

After writing the first line, save (via <kbd>CRTL</kbd>+<kbd>o</kbd>
then hit <kbd>Enter</kbd>/<kbd>Return</kbd>) and exit (via
<kbd>CTRL</kbd>+<kbd>x</kbd>) the file.

Now we can run our first script with `bash` as the interpreter.

```bash
bash middle.sh
```

We should see the same output as before. Let's continue to enter more
commands in our script and run it.

```bash
nano middle.sh
```

> Code
```code
    head -n 12 octane.pdb
    head -n 12 octane.pdb | tail -n 10
```

Now running this script again:

```bash
bash middle.sh
```

Now we will see a single output that is a combination of both commands
that is in "middle.sh" file.

# 2- Variables and Arguments

### Questions
- How do I use arbitrary inputs without changing the script every time?
- How do I ?

### Objectives
- Learn
- Extend

## Variables

In programing, variables are information holders, or labels of
information. You can access the same information through out your
program by calling its label. You can also assign new information to the
labels. Using variables make your code more flexible.

Let's start editing our first Bash script.

```bash
nano middle.sh
```

We will type the following code block in the script file to look at
variable 'myinput'.

> Code
```code
    myinput='octane.pdb'
    echo 'My input file is' $myinput

    head -n 12 $myinput
    head -n 12 $myinput | tail -n 10
```

`$` symbol tells the shell interpreter to treat *myinput* as a
**variable** and look what is inside this label. So `$myinput` returns
the value of the variable which becomes "octane.pdb". In some bash code,
you may see `${myinput}` which is equivalent to `$myinput`

## Arguments

The input to our commands was file "octane.pdb" last time. It would be
ineffective to change our script every time when we want to examine another
file.

It is possible to provide our input as an argument to our script when we
want to run it. Let's edit "middle.sh" again:

```bash
nano middle.sh
```

> Code
```code
    myinput=$1
    echo 'My input file is' $myinput

    head -n 12 $myinput
    head -n 12 $myinput | tail -n 10
```

In this code sample we replaced "octane.pdb" value for `myinput`
variable with `$1`. This is called positional parameter and its value
will be the first argument to our script on the command line.

```bash
bash middle.sh octane.pdb
bash middle.sh pentane.pdb
```
We can also directly supply positional parameter to our commands.

```bash
nano middle.sh
```

> Code
```code
    # myinput=$1
    echo 'My input file is' $1

    head -n 12 $1
    head -n 12 $1 | tail -n 10
```

Here, we first comment out the line setting the `myinput` variable. A
comment starts with a `#` character and runs to the end of the line. The
computer ignores comments. Then, we replaced all instances of `myinput`
with `$1` which takes the value of our first script argument on the
command line.

Let's run our script to see if there is any change in the output:

```bash
bash middle.sh octane.pdb
bash middle.sh pentane.pdb
```

Now we will simplify our script and add more positional parameters so
that we can use more arguments with our script.

```bash
nano middle.sh
```

> Code
```code
    # head -n 12 octane.pdb | tail -n 10
    head -n $2 $1 | tail -n $3
```
After changing our code as above, we run it:

```bash
bash middle.sh octane.pdb 12 10
```
The positional parameters `$1`, `$2` and `$3` receive their values in
the same order as arguments given on the command line. Consequently the
output remained the same.

In case the argument happens to contain any spaces, we surround both the
argument and positional parameter with double-quotes. In general it does

```bash
nano middle.sh
```

> Code
```code
    # head -n 12 octane.pdb | tail -n 10
    head -n "$2" "$1" | tail -n "$3"
```

```bash
bash middle.sh octane.pdb
bash middle.sh pentane.pdb
```

It is a good coding practice to add comments in your script to explain
what is being done to another user of your script.

```bash
nano middle.sh
```

> Code
```code
#!/usr/bin/env bash

# Select lines from the middle of a file.
# Usage: bash middle.sh filename end_line num_lines
# head -n 15 octane.pdb | tail -n 5

head -n "$2" "$1" | tail -n "$3"
```

Comments are invaluable for helping people (including your future self)
understand and use scripts. However each time you modify the script, you
should check that the comment is still accurate.


