#### Nano Hackers Extra Session 3

# The Shell

A programmer using a GUI is like a surgeon wearing mittens. The shell is our answer. It'll let us talk directly to the operating system and see what's going on under the hood.

## Launching

1. Open a Finder window
2. Find Terminal in _Applications/Utilities_
3. Double-click Terminal

What do you see? This line of text ending in `$` is called the _prompt_. It means the shell is waiting, asking us to type in a command.

When we type a command, the shell forwards it to the operating system and prints the result.

## Commands

Let's play with some commands.

```
$ date
Fri Jan  8 14:28:40 EST 2016
```

```
$ whoami
jonah
```

What does this do?

```
$ yes
```

Eek! Quit Terminal and restart it!

## Arguments and Options

Just like functions, commands can take arguments.

`echo` prints what we give it:

```
$ echo hello, world
hello, world
```

`cat` (short for _concatenate_) prints a file:

```
$ cat .gitconfig
[user]
   name = Jonah Burke
   ...
```

`wc` (short for _word count_) counts words:

```
$ wc -l .gitconfig
```

See that `-l`? That's called an _option_. Options are prefixed with `-`. This option tells `wc` to count not words but lines.

`man` (short for _manual_) gives us help:

```
$ man echo
```

`man` is super helpful!

## Lab 1: Homebrew

Homebrew is a _package manager_. It helps us install new programs so we can do more things in the shell. In this lab we will do one extremely useful thing.

After installing Homebrew (http://brew.sh), install the package `cowsay` and put it to use ...

## Launching II

As programmers, we use Terminal a lot. Let's make it easier to launch.

First, put Terminal in the Dock. Now, try this shortcut: `⌘-Space` then type `term`

Let's also spruce it up:

1. Switch to Terminal
2. Click Preferences under the Terminal menu
3. Choose a profile you like

## The Filesystem

When we're coding, we often have to view, move, and copy files. The shell helps us do this quickly.

First let's learn how files are organized on the computer.

1. Open up a Finder window
2. Go to the computer with Go > Computer or `Shift-⌘-C` 
3. Click on the disk (the _root_)

Click around a little bit. See how files and folders are arranged in a hierarchy? In computer science, this is called a _tree_.

Now let's do that using the shell.

1. Open a Terminal window
2. `$ cd /` (the _root_)
3. `ls` (_list_)

What do you see? The same files as we saw in the finder under the disk. When we're using the shell, there is a _current directory_ or _present working directory_:

```
$ pwd
/
```

We change that with `cd` followed by the name of a directory:

```
$ cd Applications
$ pwd
/Applications
$ ls
...
```

`~` is the name of a special directory called our _home directory_. We shouldn't modify files in some directories (e.g. the root), but in the home directory, we can do anything we want.

```
$ cd ~
$ pwd
/Users/jonah
```

`cp` copies files:

```
$ cowsay hi > pic1
$ ls
$ cp pic1 pic2
$ ls
$ cat pic1
$ cat pic2
```

`rm` deletes (or removes) them:

```
$ rm pic1
$ rm pic2
$ ls
```

And `mv` moves them:

```
$ cowsay hi > pic1
$ ls
$ mv pic1 pic2
$ ls
$ cat pic1
$ cat pic2
```

## Pipes

With _pipes_, we can use the output of one program as the input to another.

What happened when we ran `ls` in `/Applications`? There were too many files; we couldn't see them all in the shell window. To correct this, we use `more`, a command that breaks input into pages. We direct—or _pipe_—the output of `ls` into `more` using the pipe character `|`:

```
$ cd /Applications
$ ls | more
```

## Redirection

Pipes let us direct output to another program. What if we want to direct it to a file? For this we use a feature called _redirection_. For example, to save a picture of a cow mooing in the file `moo_pic`:

```
$ cowsay moo >moo_pic
$ cat moo_pic
```

`>` is called a _redirection operator_. Data flows in the direction of the arrow—left to right. To take input _from_ a file, then, we use the other arrow:

```
$ echo moo >moo_text
$ cowsay <moo_text
```

We can also use the two redirect operators together:

```
$ echo moo >moo_text
$ cowsay <moo_text >moo_pic
$ cat moo_pic
```

## Shell Scripting

The shell isn't just for interactive use. It can also be used to run simple programs called _shell scripts_.

Shell scripts are simply a series of shell commands in a file. However, this file must start with a special line called a _shebang_, and it must have a certain _file permission_.

### Shebang

The shebang line looks something like this:

```
#!/bin/bash
```

It starts with a _hash_ and a _bang_ (exclamation point). Hence: _shebang_. After `#!` comes the full path to the shell program that will execute the shell script. Usually this is `/bin/bash`.

### Commands

After the shebang comes our shell commands. Below is a very simple but complete script. Of course, we could and usually do add many more shell commands.

```
#!/bin/bash
echo "Hello, world!"
```

Create a file named `hello` containing the above lines. To do this, use your text editor (Sublime, Atom, nano, vim, etc.). You can also use the command `cat >hello` and type the lines into the shell. If you do this, terminate the input with `Ctrl-D`.

### File Permissions

To run our shell script, we `cd` to the directory containing it and run `./hello`. (We'll learn how to get rid of that `./` later.) But what happens?

```
-bash: ./hello: Permission denied
```

The problem is that we have not told the OS that `hello` is a program. In other words, we have not granted ourselves permission to run it.

In Unix-like OSes, files have _permissions_. A file's permissions determine what you can do with it. Can you read it? Can you change it? One of these permissions is _execute_—whether you can _run_ it.

To add the execute permission, we use `chmod`:

```
$ chmod u+x hello
```

`u+x` means that the user who owns the file (`u`) should be granted (`+`) the execute permission (`x`). To verify that the permission has been added, use `ls -l`:

```
$ ls -l hello
-rwxr--r--  1 jonah  staff    33B Jan 10 23:39 hello*
```

The `x` in the leftmost column means we can execute `hello`. (To learn about the other symbols at the left, see `man chmod`.)

Now we can run our script:

```
$ ./hello
Hello, world!
```

### The Path

Let's look at that `./` we typed before `./hello`. Why did we have to type that?

First of all, in bash, `.` means "the current directory." So `./hello` means, "the hello in the current directory."

We needed to specify this because when we run a program, _bash does not look for the program in the current directory_. Instead, it looks in a list of directories called the _path_.

We can print the path like as follows. Note that `:` separates directories in the path.

```
$ echo $PATH
/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

We can add to the path with `export`. Suppose we've put hello in the directory `bin` beneath our home directory (`~`). We add `~/bin` to our path like so:

```
$ export PATH=~/bin:$PATH
$ echo $PATH
/Users/jonah/bin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

Now we can run hello without the `./`. bash will find hello in `~/bin` because `~/bin` is in the path.

```
$ hello
Hello, world!
```

### .bash_profile

If we exit the shell and launch it again, our changes to the path will be lost. We need to tell bash to update our path each time it runs. To do this, we add shell commands to the file `~/.bash_profile`.

Use your favorite editor to add `export PATH=~/bin:$PATH` to `~/.bash_profile` and restart the shell. `echo $PATH` should show you that bash has added `~/bin` to the path.

## Lab 2

Try using Git from the command line. Change to your home directory, clone one of your repos from GitHub, make a change, and push the change backup. You'll need these commands:

```
cd
git clone
git add
git commit
git push
```