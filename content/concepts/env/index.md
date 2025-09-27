+++
title = 'env the Command Launcher you Never Knew you Had'
date = '2025-08-24T08:12:05+01:00'
draft = false
showWordCount= true
enableCodeCopy = true
showDate = true
heroStyle= "background"
+++

{{< figure
src="always.png"
nozoom=true
alt="Abstract purple artwork">}}

# Environment variables

An **environment** is a collection of dynamic, named values—called **environment variables**—that can affect the way running processes will behave on a computer.

Every single process has its own copy of this environment.

Some of the most common environment variables include :

- **`USER`** (or **`LOGNAME`**): the name of the user currently login.
- **`HOME`**: The absolute path to the `USER` home directory (e.g., `/home/hacker`). This is where the shell starts you off and where programs look for user-specific configuration files.
- **`PATH`**: Arguably the most important one. It's a colon-separated list of directories. When you type a command like `cat` without specifying its full path (`/bin/cat`), the shell searches through the directories listed in `PATH` until it finds an executable file with that name.
- **`PWD`**: The path to your **P**resent **W**orking **D**irectory. It changes every time you `cd`.
- **`LANG`**: Controls the language, character set, and formatting for things like dates and currency.

# How to View and Work with environment variables

- **To see all environment variables:**
  {{< highlight bash "linenos=inline, hl_lines=1 3, style=base16-snazzy" >}}
  $ env
  # or
  $ printenv
  {{< /highlight >}}
- **To see the value of a single variable:** Use `echo` and a `$` followed by the variable name.
  {{<alert>}}
  **Note**: There is no space between the `$` sign and the `variable` name
  {{</alert>}}
  {{< highlight bash "linenos=inline, hl_Lines=2 3, style=average" >}}
  #echo $VAR_NAME
  $ echo $PATH # Output: /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
  {{< /highlight >}}

# Inheritance

When a new process is created, it **inherits a copy of its parent's environment** [^1].

This is why everything works so seamlessly accross different commands and programs.

1.  You log in, and your main shell process (`bash`) is created with a full environment (`USER=john`, `PATH=...`, etc. ).
2.  You run the `ls` command.
3.  The shell creates a new child process for `ls`. This `ls` process gets a **complete copy** of the shell's environment variables.
4.  The `ls` program can now look at its own environment to get information if it needs to, without having to ask the parent shell.

# Using the `env` Command

So, if every process automatically inherits the environment, why do we need a command called `env`?

The `env` command has two main purposes:

1.  **To view the current environment** (when run with no arguments).
2.  **To `run` a command in a _modified_ environment**, without changing your current shell's environment.

**Running commands in a modified environment**

Let's say your `LANG` environment variable responsible for your language preference is English, and you want to see the date in French without changing your system language for example.

{{< highlight bash "linenos=inline, hl_lines=1-7, style=average" >}}

# This will use our default English environment

$ date

# Output: Tue 21 Aug 2025 10:30:00 AM UTC

# This runs 'date' in a temporary, modified environment

$ env LANG=fr_FR.UTF-8 date

# Output: mar. 21 août 2025 10:30:00 UTC

{{< /highlight >}}

That is it. Thanks for reading

I hope you learned something from this post. Any suggestions or areas of improvement? Please let me know, it will help me a lot to write better future posts. Thanks !!

{{<alert>}}

1. This can be abused if the `env` command has a `root setuid` bit set.
   {{</alert>}}

[^1]: The following [youtube](https://youtu.be/6GwNvaUUgWU?si=QWw3wGjjLDJdVHfW) playlist from _Indronil Banerjee_ has a good playlist explaining how processes work on a linux Os.
