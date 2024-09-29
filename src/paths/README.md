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



## Environment Paths



## Exercises

1\. Which of the following is NOT an absolute path?

​	a\. `/home/andrew`

​	b. `./home/kim`

​	c. `/`

​	d. `/java/../cpp/hw1`

2\. Which of the following is NOT an relative path?

​	a\. `..`

​	b. `/.ssh/id_rsa`

​	c. `../kim/.ssh/`

​	d.  `andrew/kotlin/src`

3\. Which of the following path resolution is NOT correct?

​	a\. `/home/james` + `resume` => `/home/jamesresume`

​	b\. `/etc/nginx` + `./conf.d` => `/etc/nginx/./conf.d`

​	c. `/usr/bin/` + `mkdir` => `/usr/bin/mkdir`

​	d. `/home/andrew/php` + `../go` => `/home/andrew/php/../go`

4\. Which of the following path normalization is correct?

​	a\. `/home/andrew/php/../go` => `/home/andrew/php/go`

​	b\. `/usr/bin/./touch` => `/usr/touch`

​	c\. `/home/james/./resume` => `home/james/resume`

​	d\. `/home/andrew/./../pat/resume` => `/home/pat/resume`
