+++
title = 'I Was So Wrong About Sudo'
date = '2025-07-29T21:21:44+01:00'
draft = false
showWordCount= true
showDate = true
enableCodeCopy = true
heroStyle= "background"
+++

{{< badge >}}
New article !
{{< /badge >}}

# I Was Wrong About `sudo`. Let's Talk about SetUID

I thought I had a decent handle on Linux permissions `users`, `groups`, the classic `rwx` bits. Then I came across the pwn.college program misuse [module](https://pwn.college/fundamentals/program-misuse).
While working on a `privilege escalation problem`, something worked that just shouldn't have, based on the mental model I was using at the time. It forced me to see that my whole idea of sudo was wrong.
I always saw `sudo` as some magic keyword built into the OS. Well turns out it's not. The real power is a much simpler, older concept called `SetUID`.SetUID is basically all you need to understand how this stuff really works.

## **TL;DR**

- **`sudo` is just a program that uses `setuid`:** It isn't a magic OS keyword. It's a normal program owned by `root` that has the `setuid` permission bit. It uses its own `setuid`-given power to run _your_ commands as `root`.

- **`setuid` is automatic privilege elevation:** The `setuid` bit (the `s` in `ls -l`) on any program simply means: "Run this as the file's owner, not the person who executed it." No `sudo` command needed. If `root` owns the file, the program runs as `root`.

- **Your "Effective" ID changes, not your "Real" one:** The kernel tracks who you _really_ are (Real UID) versus what you can _currently do_ (Effective UID). `setuid` only changes your Effective UID for that one process, and that's the ID the OS checks for file permissions.

- **This power is persistent (and dangerous):** The elevated privilege lasts for the **entire time** the program is running. It doesn't just turn off automatically, which is why poorly written `setuid` programs can be a huge security risk.

- **Note :** For security, most Linux systems mount temporary directories like `/tmp` with the **`nosuid` mount option**. This rule tells the kernel to completely ignore the `SetUID` bit for any program on that entire filesystem. It's a security control to stop an attacker from running `SetUID` from this location.
  You can check this yourself by running `mount | grep /tmp`.

## **Back to the Basics: How do processes normally get Permissions**

On Linux, everything is about **access control**. The `OS` checks _user_ and _group_ IDs (UID and GID) to decide who can do what. When a program runs, it's just a set of instructions spawning various processes, and that process needs permissions to do its job (like read a file or open a network connection).

Here's the normal rule: **Every process inherits the user and group ID of the parent process that spawned it.** When, user `john`, runs a command, it runs with john's `UID` and `GID`.

## **Privilege Escalation is a FEATURE not a de facto vulnerability as I was thinking**

This is where my first big "aha!" moment happened. `Privilege Escalation (PE)` isn't inherently a **vulnerability**. At its core, it's a **mechanism** that allows a user to perform actions that only a _different_ user has the permissions for. It only becomes a vulnerability when it is _misconfigured_.

On Linux, this mechanism is primarily implemented through two tools we often see: `SetUID` and `sudo`. And as you will soon learn, the latter is completely dependent on the former.

## **SetUID**

`SetUID (Set User ID)` is a **special** permission bit. It has an octal value of `4` and is represented by an `s` in the file owner's permission block (`-rwsr-xr-x`).

Its function is simple but powerful: it allows anyone **with execute permissions on the file** to run that program with the ~~power~~(permissions) of the file's **owner**.

Here’s how the process works step-by-step:

1.  A user starts a program that has the SetUID bit set.
2.  When the Linux kernel sees the `s` bit, it breaks the normal inheritance rule.
3.  Instead of using the _user's ID_, the kernel sets the process's **effective user ID (euid)** to the ID of the user who **owns the file**.
4.  If not explicitly changed in the code, the program continues to run with the owner's privileges until it terminates.

## **So, What Is `sudo` Then ?**

**`sudo` is not a magic do everything command** it is just a normal C program located at `/usr/bin/sudo` that is owned by `root` and has the SetUID bit set.

Its behavior is an exact, but much more secure, implementation of the SetUID mechanism. Here is what really happens when you type `sudo apt update`:

1.  You execute `/usr/bin/sudo`, which starts up with an effective user ID of `root` because of the SetUID bit.
2.  But `sudo` doesn't just run your command. It's a security mechanism put in place to control and keep track of command running with high privilegies. First, it uses its root power to read the `/etc/sudoers` file—a file only `root` can read.
3.  It checks if you, the real user, is listed in that file and have permission to run `apt update`.
4.  If the rules match, it may ask for your password to verify your identity.
5.  **Only if all these checks pass** does `sudo` then spawn a child process to run `apt update`, which now inherits the `root` context from its parent, `sudo`.

## **DIY**

Not being a system programmer I asked `gemini` for a modified version of the `cat` command that prints the `ruid,euid,gid and egid` of the person runing the program along with the content of the file.

1. Copy the code below into a file, for example: `simplecat.c`

{{< highlight c "linenos=inline,style=RPGLE" >}}

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <unistd.h> // For getuid() and geteuid()

int main(int argc, char *argv[]) {
    FILE *fp;
    char buffer[1024];

    // Print Real and Effective UIDs to observe the change
    printf("Real UID: %d\n", getuid());
    printf("Effective UID: %d\n", geteuid());
    printf("Real GID: %d\n", getgid());
    printf("Effective GID: %d\n", getegid());

    if (argc < 2) {
        fprintf(stderr, "Usage: %s <filename>\n", argv[0]);
        return 1;
    }

    fp = fopen(argv[1], "r");
    if (fp == NULL) {
        perror("Error opening file");
        return 1;
    }

    while (fgets(buffer, sizeof(buffer), fp) != NULL) {
        printf("%s", buffer);
    }

    fclose(fp);
    return 0;
}
```

{{< /highlight >}}

2. compile the program\
   `gcc simplecat.c -o mycat`

3. You can try this by compiling _two_ versions of this program and set the `setuid` bit for one and the other one without the `setuid` bit. The latter will be run using `sudo` to access. Create two distinct `users` with different `permissions` and try to access a `file` based on the `permissions` you have set, and note the behaviour when using either the `sudo mycat` command or the permission from the `setuid` bit

{{< alert >}}
**Note :** For security, most Linux distributions mount temporary directories like `/tmp` with the **`nosuid` mount option**. This rule tells the kernel to completely ignore the `SetUID` bit for any program on that entire filesystem. It's a security control to stop an attacker from running `SetUID` from this location.
You can check this yourself by running `mount | grep /tmp`.

That is it. Thanks for reading

I hope you learned something from this post. Any suggestions or areas of improvement? Please let me know, it will help me a lot to write better future posts. Thanks !!
{{< /alert >}}
<br>
{{< keywordList >}}
{{< keyword >}} Linux {{< /keyword >}}
{{< keyword >}} File Permissions {{< /keyword >}}
{{< keyword >}} sudo {{< /keyword >}}
{{< keyword >}} getuid {{< /keyword >}}
{{< keyword >}} suid {{< /keyword >}}
{{< /keywordList >}}
