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

## Break (15 Minutes)

Try experimenting with the `lynx` and `wget` packages.

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

## Lab 2

Try using Git from the command line. Change to your home directory, clone one of your repos from GitHub, make a change, and push the change backup. You'll need these commands:

```
cd
git clone
git add
git commit
git push
```