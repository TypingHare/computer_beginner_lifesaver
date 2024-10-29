# Paths in Linux

Have you ever come across `FileNotFoundException` or similar exceptions and don't know how to address? Have you ever tried to move the code to another device and then it can't run? This article will be covering everything you need to know about PATHS.

> [!IMPORTANT]
>
> **By the end of this article, readers will be able to:**
>
> * Differentiate between relative and absolute paths, comprehend the concept of the working directory, and explain how they interrelate in the Linux filesystem.
> * Execute essential Linux commands for navigating and managing the filesystem, including understanding how paths are used in practical command-line operations.
> * Navigate and manage command-line sessions effectively, gaining familiarity with terminal environments and shell interaction.
> * Understand the role and configuration of environment paths, and how they influence command execution and software accessibility.

## Relative Path, Absolute Path, and Working Directory

An **absolute path** specifies the complete location of a file or directory from the **root directory**, which is `/` in Linux. For instance, the following are absolute paths:

* `/`
* `/etc/nginx/`
* `/home/james/cpp/src/hw1/Makefile`

Note that in Linux, it can be unclear whether a path refers to a file or a directory, even if the path ends with `/`, since directories are also treated as files. To avoid ambiguity, it's good practice to append a `/` at the end of directory paths, clearly indicating that the path refers to a directory. 

An **relative path** specifies the location of a file or directory relative to the current **working directory**. It doesn't start from the root but from the folder you're currently in. While all absolute paths always start with `/`, relative paths never. The following are relative paths:

* `./foo/bar`
* `src/hw1/Makefile`
* `..`

Operating systems can locate a file or directory using an absolute path. When a relative path is provided, the operating system resolves it by combining the relative path with the current working directory to form an absolute path. For example, if the working directory is `/home/james` and you attempt to open `diary/20240925.txt`, the operating system will concatenate the working directory with the relative path, resulting in `/home/james/diary/20240925.txt`. Such process, combining the working directory with a relative path to form an absolute path, is called **path resolution**.

Every directory in Linux contains two special entries: `.` and `..`. 

- `.` refers to the **current directory**. It is idempotent, meaning it does not change the directory and can be safely removed from a path without affecting the result.
- `..` refers to the **parent directory**, allowing navigation one level up in the directory hierarchy.

For example, `src/components/./Main.tsx` is equivalent to `src/components/Main.tsx` because `.` can be removed without altering the path; `/home/james/java/../kotlin` is equivalent to `/home/james/kotlin` because `..` moves up one directory from `java` to `home/james`, and then navigates to `kotlin`. Such process, removing `.` (current directory) and resolving `..` (parent directory) in a path, is called **path normalization**.

## Essential Linux Commands

To effectively navigate and manage files in Linux, it’s important to familiarize yourself with some essential commands. Practicing these commands is the best way to learn, so I encourage you to open your terminal and try executing the following commands. Note that lines beginning with `#` are comments, while `$` represents the command prompt and is not part of the command itself.

```shell
# The `pwd` (Print Working Directory) command displays your current working directory
$ pwd

# The `ls` command displays all files and directories in the current working directory
$ ls

# The `-a` option lets the `ls` command print all hidden files and directories
# In Linux, files and directories beginning with `.` are hidden
$ ls -a

# This command will create a `demo` directory, if it yet exists
$ mkdir demo

# The `cd` (change directory) command navigates you to another directory
$ cd demo

# Now use the `pwd` command to check if your current woring directory has changed
$ pwd

# This command will create a file called `demo.txt`
$ touch demo.txt

# The `rm` (remove) command removes a file
$ rm demo.txt

# Use the `ls` command to check if the `demo.txt` is removed
$ ls

# Move to one level up in the directory hierarchy; recall that `..` refers to the parent directory
$ cd ..

# The `-r` (recursion) option allows the `rm` command to remove a directory and everything it contains
$ rm -r demo
```

> [!IMPORTANT]
>
> Take five to ten minutes to play around with these Linux commands, and explore how *path resolution* and *path normalization* work in the terminal.
>
> > Practices make perfect.
> >
> > <div style="display: flex; width: 100%; justify-content: flex-end;">
> >   <span style="float: right;">
> >   ——Feliks Zemdegs
> >   </span>
> > </div>

## Command-line Sessions

Each time you open a terminal, a new session is created. If you open a new tab, it starts a separate session, and each session operates independently. Every session maintains its own set of **environment variables**, which store configuration settings that affect how processes run. You can view the environment variables and their values using the `env` command. Below is an excerpt from the `env` command output on the UMass Lowell cloud server.

```text
XDG_SESSION_ID=5657
USER=zchen2
PWD=/home/undergrad/2026/zchen2
SSH_ASKPASS=/usr/libexec/openssh/gnome-ssh-askpass
HOME=/home/undergrad/2026/zchen2
```

Each line represents a **key-value pair** (or an **entry**), where both the key and value are strings. To display the value associated with a specific key, use the command `echo $<key>`:

```shell 
$ echo $USER
zchen2
$ echo $HOME
/home/undergrad/2026/zchen2
$ echo $PATH
/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/undergrad/2026/zchen2/bin
```

You can set a new environment variable using the `export` command. If the key already exists, the new value will override the previous one.

```shell
$ export TEST="test"
$ echo TEST
test
```

As mentioned earlier, each session has its own set of environment variables. This means that if you set a variable in one tab, it won't affect any other tabs. Additionally, if you close the terminal and reopen it, any changes to environment variables will be lost. To ensure that certain environment variables are set automatically every time you start a new session, add the `export` commands to your **profile configuration file** (or simply **profile file**).

Use the `echo $0` command to determine which **command-line interpreter** you are using. The most commonly used interpreters on Unix-like systems, such as Linux distributions and macOS, are `bash` and `zsh`. If the output is `bash`, the corresponding profile file is `~/.bashrc` (where `~` represents your **home directory**). If the interpreter is `zsh`, the profile file is `~/.zshrc`.

> [!IMPORTANT]
>
> **PRACTICE 1**    Execute the command `echo ~`. What does the output stand for?
>
> **PRACTICE 2**    Append `export HELLO="hello world"` to your profile file, then open a new terminal session and run the command `echo $HELLO` to see the value of the variable.

## Environment Paths

In the previous chapter, we discussed environment variables, highlighting the significance of the `PATH` variable and several other related variables. It is the culprit of causing a lot of tricky problems that torturing beginners to emotional breakdown. It is time for uncovering its mystery.

Start by running the `echo $PATH` command on your computer. This will display a string containing multiple absolute paths separated by colons (`:`). These paths typically end with `bin`, which stands for **binary directory** and is where many executable files are stored. We can split the long string by piping it to a `tr` command:

```bash
$ echo $PATH | tr : "\n"
/usr/local/bin
/usr/bin
/bin
```

When we enter a command, the system searches for an executable file with the same name in each of these directories, in the order they appear in the `PATH`. If it can't find the file, you'll see an error message: `command not found: xxx`, where "xxx" is the command you attempted to run.

Before discussing the mechanism the operating system applies to locate commands, the command `which` has to be introduced. We can use the `which` command to find the path to a specific executable. For example:

```bash
$ which touch
/usr/bin/touch
$ which g++
/usr/bin/g++
```

Many pre-installed system programs are located in` /usr/bin`. However, if you install a program through a **package manager**, the location of the executable may vary. For instance:

```bash
$ which java
/Library/Java/JavaVirtualMachines/jdk-21.jdk/Contents/Home/bin/java
$ which python
/opt/homebrew/anaconda3/bin/python
```

While `/usr/bin` is typically included in the `PATH` by default, custom directories like `/Library/Java/JavaVirtualMachines/jdk-21.jdk/Contents/Home/bin` and `/opt/homebrew/anaconda3/bin` are not. To make these custom paths accessible from the command line, we need to add them to the `PATH` variable as follows:

```bash
export PATH="$PATH:/Library/Java/JavaVirtualMachines/jdk-21.jdk/Contents/Home/bin"
export PATH="$PATH:/opt/homebrew/anaconda3/bin"
```

To ensure these commands are automatically executed each time a new session is created, we can add them to our profile file. This way, the custom paths will be included in our `PATH` variable every time we start a new terminal session.

In addition to `PATH`, many compilers rely on specific environment variables to locate source files. For instance, g++ uses `CPATH` and `LIBRARY_PATH` to define paths for `.hpp` header files and `.cpp` library files, respectively. If you're using Homebrew to install C++ libraries, you’ll need to set these variables in your profile file:

```bash
export CPATH="$CPATH:/opt/homebrew/include"
export LIBRARY_PATH="$LIBRARY_PATH:/opt/homebrew/lib"
```

Here, `/opt/homebrew` is the default Homebrew root directory on macOS with Apple Silicon, but it may vary depending on your operating system or installation setup. Adding these paths ensures that `g++` can find your Homebrew-installed libraries automatically.

## Exercises

1\. Which of the following is NOT an absolute path?

​        a\. `/home/andrew`

​        b. `./home/kim`

​        c. `/`

​        d. `/java/../cpp/hw1`

2\. Which of the following is NOT an relative path?

​        a\. `..`

​        b. `/.ssh/id_rsa`

​        c. `../kim/.ssh/`

​        d.  `andrew/kotlin/src`

3\. Which of the following path resolution is NOT correct?

​        a\. `/home/james` + `resume` => `/home/jamesresume`

​        b\. `/etc/nginx` + `./conf.d` => `/etc/nginx/./conf.d`

​        c. `/usr/bin/` + `mkdir` => `/usr/bin/mkdir`

​        d. `/home/andrew/php` + `../go` => `/home/andrew/php/../go`

4\. Which of the following path normalization is correct?

​        a\. `/home/andrew/php/../go` => `/home/andrew/php/go`

​        b\. `/usr/bin/./touch` => `/usr/touch`

​        c\. `/home/james/./resume` => `home/james/resume`

​        d\. `/home/andrew/./../pat/resume` => `/home/pat/resume`

5\. Christ is currently in the `/home/christ/cpp` directory and runs the command `cd src/hw1`. Which of the following outcomes are possible? Select all that apply.

​        a. Christ is navigated to `/src/hw1`

​        b. An error message is displayed, and Christ remains in the `/home/christ/cpp` directory.

​        c. Christ is navigated to `/home/christ/cpp/src/hw1`.

​        d. An error message is displayed, and Christ is navigated to `/src/hw1`.

6\. Bob has trouble navigating to `/home/bob/java/src/main`. His terminal history is as follows:

```shell
$ pwd
/home/bob
$ cd java/src
$ cd main
cd: no such file or directory: main
$ ls
test Main.java Makefile
```

Which of the following is NOT correct? Choose all that applies.

​        a. There is no folder named `main` in `/home/bob/java/src`

​        b. When Bob executes the `cd main` command, he is still in the `/home/bob` directory.

​        c. There are three files or directories in `/home/bob/java/src`

​        d. The `cd main` command creates a `main` folder in `/home/bob/java/src`.

7\. 
