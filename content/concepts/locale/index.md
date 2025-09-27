+++
title = 'Linux locale command'
date = '2025-08-24T07:54:55+01:00'
draft = true
showWordCount= true
enableCodeCopy = true
showDate = true
showComments = true
+++

{{< figure
    src="featured.png"
    nozoom=true
    alt="Abstract purple artwork"
>}}

At its core, the `locale` command gives information on how a Linux system and its programs are configured to display information related to language and geographical region[^1].This is somehow similar to the *"Language & Region"* settings panel in Windows or macOS. It's where you set things such as :

*   The display language (English, French, Japanese etc...).
*   The format for dates and times (`MM/DD/YYYY` vs. `DD.MM.YYYY`).
*   The symbol for currency (`$` vs. `€`).
*   The character used as a decimal point (`.` vs. `,`).
*   How text should be sorted alphabetically.

The `locale` command is the command-line equivalent of that settings panel. It shows the current values for all these settings in a given terminal session.

---

# How to Use It

## 1. Running `locale` without arguments

When you run the `locale` command with no arguments, it prints a list of environment variables that control these settings. Each variable handles a specific category.

```bash
$ locale
```

**Example Output and What It Means:**

*   **`LANG=en_US.UTF-8`**: This is the most important one. It's the **default** language and region for all categories unless a more specific variable is set. This means US English using the UTF-8 character set.
*   **`LC_CTYPE="en_US.UTF-8"`**: Controls character classification. (What is a letter? What is a number? How are they sorted?)
*   **`LC_NUMERIC="en_US.UTF-8"`**: Controls number formatting. (e.g., `1,000.00`)
*   **`LC_TIME="en_US.UTF-8"`**: Controls date and time formatting. (e.g., `Tuesday, August 21, 2025`)
*   **`LC_COLLATE="en_US.UTF-8"`**: Controls the alphabetical sorting order of strings.
*   **`LC_MONETARY="en_US.UTF-8"`**: Controls currency formatting. (e.g., `$1,234.56`)
*   **`LC_MESSAGES="en_US.UTF-8"`**: Controls the language of system messages and program menus.
*   **`LC_ALL=`**: This one is special. If it's set, it acts as a master override and forces **all** categories to use its value, ignoring `LANG` and all other `LC_*` variables. It's usually empty by default.

## 2. Listing All Available Locales: `locale -a`

It's used to list all the *"language packs"* or *locale definitions* that are **installed on the system**.

```bash
$ locale -a
```

A program can only switch to a locale if that locale's name appears in the output of `locale -a`.

---

# Managing Locales

Locales, are often **not pre-installed** to save space. The process involves generating the locale files from a master list. This is a system administration task and requires `sudo`.

### Verify Installed Locales

Using the `locale -a` command we can verify if the *locale (language)* we want to use hasn't been installed (compile) already.

```bash
$ locale -a
```
This command reads the compiled locale files from the system. If it's not in this list, the system can't use it.

### Installing New Locales (Requires `sudo`[^2])

This is a three-step process: **edit, generate, and update**.
{{<alert>}}
**Note**: Each of these steps requires `root` privileges. So you will need `sudo` to perform each step.
{{</alert>}}
<br>
**Step 1: Edit the Master List**
The list of all possible locales your system knows about is in the file `/etc/locale.gen`.

```bash
$ sudo nano /etc/locale.gen
```

Inside, you will see hundreds of lines, most of them commented out with a `#`. Below is short output of the `/etc/locale.gen` file

```
# ...
#es_US.UTF-8 UTF-8
#eu_ES.UTF-8 UTF-8
#fa_IR UTF-8
#fi_FI.UTF-8 UTF-8
#fr_BE.UTF-8 UTF-8
#fr_CA.UTF-8 UTF-8
#fr_CH.UTF-8 UTF-8
#fr_FR.UTF-8 UTF-8 
#fr_LU.UTF-8 UTF-8
# ...
```

**Step 2: Uncomment the Locales You Want**
Find the line for the locale you want to install (e.g., `fr_FR.UTF-8 UTF-8`) and **remove the `#` at the beginning of the line**. You can uncomment multiple locales if you wish. Save and close the file.

**Step 3: Generate the Locales (update)**
Now, run the `locale-gen` command. This command reads `/etc/locale.gen`, sees the lines you've uncommented, and compiles the necessary binary locale files from the system's source data.

```bash
$ sudo locale-gen

You will see output as it generates the new files:

Generating locales (this might take a while)...
  fr_FR.UTF-8... done
Generation complete.
```

**Verify the Installation**
Run the verification command again. Your newly generated locale should now appear in the list.
```bash
$ locale -a
```

You can further test this using the `env`[^3] command as follow 
```bash
$ env LANG=fr_FR.UTF-8 date 
# replace the *value* of `LANG` with the value of the "language pack" you just compiled.
```

Thanks for reading. Until then 

[^1]: Language and geographical region informations refer to any output that is language dependent it won't change output messages for commands that are not region dependent.
[^2]: Not so sure what `sudo` is ?? check my post on [sudo]({{% ref "sudo-vs-setuid" %}})
[^3]: A post on {{% reltarget path="env" text="`env`" %}}