# File System

## Working with the Command Line

The command line / terminal is an interface where you can type and execute text-based commands.
It is a powerful tool and works the same on almost every environment you will ever encounter.

To open the terminal on Ubuntu, press `Ctrl` + `Alt` + `T`.

To open a terminal on Mac you can open the search bar (`command` + `space`)

### Commands, Options, and Arguments

To execute some action on the terminal, you enter a command, followed by options and arguments and press `Enter`/`Return`:

```sh
command -options arguments
```

* **Command** - the action you want to perform, it could be anything from moving files, navigating through directories or downloading
files from the internet
* **Options** - Modifiers that alter the behaviour the command. Options are usually a single letter preceded by a dash (e.g. `-l`). They tweak
the command's output and/or behaviour.
* **Arguments** - The target(s) of the command. This could be a file, a directory, a url, and multiple other things, depending
on the command's purpose

> Options can be short (a single dash with a letter, e.g. `-l`) or long (two dashes following by a word, e.g. `--long`). You can usually combine
multiple short options, for example instead of `-l -a`, you can write `-la`.

In the following example, first execute the `ls` command. This command is frequently used and displays the content of the directory you are currently in:

```sh
ls
```

You can also run this command with multiple options like (`-l` for long listing, which will include more detailed information about each file and directory).

```sh
ls -l
```

Other options could be `-t` which will sort the file by modification date or `-a` which will list also hidden files (file prefixed with a dot, e.g. `.config`).

```sh
ls -l -t -a
ls -lta
```

You can also give the `ls` command a target. This target can be a directory, which will the list the contents of the directory you give as the target instead
of the current directory you are in. Supposed you have a directory called `Documents`.

```sh
ls -la Downloads # This will list all of the file contents inside the `Downloads` directory
```

## Hierarchical Directory Structure Basics

In computing, the hierarchical directory structure is a fundamental concept that organizes files and directories 
(also known as folders in some operating systems) in a tree-like pattern.

At the very top of this hierarchy is the "root directory." (`/`).
It is the first directory in the file system, serving as the base or starting point from which all other directories and files come from. 
In graphical representations, the root directory is often depicted at the top or at the center of the tree.

Descending from the root directory are various files and subdirectories. A directory can hold two things:

* a file, which is just a piece of data or content
* a directory, which can hold further directories or files

Therefore the root directory contains files and subdirectories, which contain more files and subdirectories and so on.

You are probably familiar with grahical file managers (if you ever used the native windows file explorer or `finder` on MacOS).
However the command line has no pictures. At any given time (in the command line), we are inside a single directory, here we can see the files and subdirectories
inside the directory and we have a pathway to the directory above us (also called the `parent directory`). The directory we are currently in is also called the `current working directory`.
To display the `current working directory`, you can use the `pwd` (print working directory) command.

Open the terminal and run the commands

```sh
$ pwd
/home/me
```

When we first log in to our system (or start a terminal), our current working directory will be our `home directory`.
Each user account has its own working directory.

### Change the Current Working Directory

To change our current working directory, we can use the `cd` command. 
To do this you type `cd` as the command and the `pathname` (where you want to go) as the target of the command.
The pathname is the route we take along the branches of our tree to get to our directory where we want to go.
There are two types of pathways:

* absolute pathnames
* relative pathnames

#### Absolute Pathnames

An absolute pathname begins with the root directory and follows the tree branch by branch until the path to the desired directory is completed.
For example, there is a directory on our machine where most of our systems programs are installed. The pathname to this directory is `/usr/bin`.
This means that the root directory contains a directory called `usr` and this directory has a directory called `bin`.

```sh
$ cd /usr/bin
$ pwd
/usr/bin
$ ls
... a lot of files...
```

In most of the terminals you will see that the shell prompt changed. This is just a convenience, so the user knows where he is located.
It is essentially the same as running the `pwd` command from before.

#### Relative Pathnames

In contrary to absolute pathnames which always start at the root directory, relative pathnames start from the current working directory.
To show that the current pathname should be a relative path, you can use two kinds of special notations:

* `.` - refers the the current working directory
* `..` - refers to the parent of the current working directory.

For example lets change the current working directory to the parent of `bin`. Using an absolute pathnames, you would do this:

```sh
$ cd /usr/bin
$ pwd
/usr/bin
$ cd /usr
$ pwd
/usr
```

Using relative pathnames, you would do this:

```sh
$ cd /usr/bin
$ cd .. 
$ pwd
/usr
```

If we would like to go back to the `bin` directory, we could do it like this:

```sh
$ pwd
/usr
$ cd ./bin
$ pwd
/usr/bin
```

> Note: You can omit the `./` at the beginning. If we do not specify the `/` for the root directory, 
it will be assumed that the path we input is a relative path.

#### Filenames

Facts about file names:

* Filenames that begin with a period character are hidden, this means that when using the `ls` command, they will not be displayed.
To list them you have to use the command with `-a` argument (`ls -a`).
> Note: originally the existence of hidden files was a [bug](https://tookmund.com/2022/03/rob-pike-shortcuts)
* Filenames and commands are case-sensitive, meaning that `File1` and `file1` are two different files
* Linux has no concept of a file extension like some other operating systems, the content and/or purpose of a file is determined through other means.
* Although Linux supports long filenames that may contain spaces, you should avoid spaces in filenames, and if you want to represent spaces between
words inside a file, use underscore instead.

## Basic File Commands

In this chapter, we will explain the most basic commands regarding files.
I extremely recommend you to open the terminal and following along by also typing the commands
into the terminal and look at their outputs.

### Navigating the Filesystem

1. **`pwd` Command (Print Working Directory)**:

The `pwd` command shows your current directory's path.
If you're not sure where you are in the filesystem, type `pwd` and press Enter.
It will display the full path of your current directory.

2. **`ls` Command (List Files and Directories)**:

The `ls` command is used to view the contents of a directory (like folders and files).
When you open the Terminal, you're usually in your home directory. To see what's inside, type `ls` and press Enter.
To see more details about each file, type `ls -l` and press Enter. This command lists files with details like
permissions, owner, size, and modification date.
To include hidden files (files that start with a dot) in your listing, type `ls -a`.

> In Linux and Unix-like operating systems, hidden files are those that begin with a dot (.) in their filename.
> These files are usually not displayed by default when using file managers or the ls command,
> as they are often used for storing user preferences or system configuration settings.

3. **`cd` Command (Change Directory)**:

The `cd` command is used to move between directories.
For example, to go to the Documents directory, type `cd Documents` and press Enter.
To go back to the previous directory, type `cd ..`
To return to your home directory from anywhere, just type `cd` and press Enter.

Shortcuts:

* `cd` - changes the current working directory to your home directory
* `cd -` changes the current working directory to the previous working directory

4. **mkdir Command (Make Directory)**:

To create a new directory, use the `mkdir` command.
For example, to create a directory named NewFolder, type `mkdir NewFolder` and press Enter.

5. **`touch` Command (Create a New File)**:

The `touch` command allows you to create a new, empty file.
For instance, to create a file named `example.txt`, type `touch example.txt` and press Enter.

6. **`mv` Command (Move or Rename Files/Directories)**:

The mv command is used to move files and directories from one location to another or to rename them.
To rename a file, type `mv oldfilename.txt newfilename.txt` and press Enter.
To move a file to a different directory, type `mv filename.txt /path/to/directory/` and press Enter.

7. **`cp` Command (Copy Files/Directories)**:

The `cp` command is used to copy files and directories.
To copy a file, type `cp file.txt destination/` and press Enter
(where destination/ is the directory where you want to copy the file).
To copy a directory and its contents, use the -r flag: `cp -r directory/ destination/`.

8. **`rm` Command (Remove Files/Directories)**:

The `rm` command is used to delete files.
To delete a file, type `rm filename.txt` and press Enter.
To delete a directory and its contents, use the -r flag: `rm -r directory/`.
Be cautious with this command, as it permanently deletes files and directories.

9. **find Command (Search for Files/Directories)**:

The `find` command is used to search for files and directories based on different criteria.
To find a file by name, type `find /path/to/search/ -name "filename.txt"` and press Enter.
You can also search based on file type, size, modification date, and more.

10. **`grep` Command (Search Within Files)**:

The `grep` command is used to search for specific text within files.
To search for a particular word or phrase in a file, type `grep "search term" filename.txt` and press Enter.
`grep` can also be used with options for case-insensitive search, searching recursively in directories, displaying line numbers, and more.

11. **`locate` Command (Find Files by Name)**:

The `locate` command is used to quickly find the location of files and directories by their names.
To find a file, type locate filename.txt and press Enter. It will display paths of all files and directories with "filename.txt" in their names.
The locate command relies on a database that is periodically updated by the system; to update this database manually, you can use the `sudo updatedb` command.

Not it is time to practice and do it yourself. Perform the following instructions by using the commands
you just learned. You can check the correctness of the current step by just viewing the contents
after doing a command.

1. Open the Terminal on your Ubuntu system.
2. View the contents of your current directory and then view the detailed information about each item
   in this directory.
3. Navigate to the "Documents" directory from your current location.
4. Check which directory you are currently in.
5. Create a new directory called "PracticeFolder" inside "Documents."
6. Move into the newly created "PracticeFolder."
7. Create a new file named "example.txt" in "PracticeFolder."
8. List the contents of "PracticeFolder" to verify the creation of "example.txt."
9. Create another directory inside "PracticeFolder" called "SubFolder."
10. Copy "example.txt" into "SubFolder."
11. Rename "example.txt" to "example-renamed.txt" within "PracticeFolder."
12. Move "example-renamed.txt" from "PracticeFolder" to "SubFolder."
13. Navigate back to the "Documents" directory.
14. Search for the file "example-renamed.txt" starting from "Documents."
15. Navigate into "PracticeFolder/SubFolder."
16. Write some text inside "example-renamed.txt" (you can use a text editor for this).
17. Search for a specific word or phrase within "example-renamed.txt" using grep.
18. Use locate to find the path of another file on your system (e.g., "example.txt").
19. Delete "example-renamed.txt."
20. Navigate back to "PracticeFolder" and delete "SubFolder."
21. Finally, navigate back to "Documents" and delete "PracticeFolder."

### Deep Dive into `mv`, `cp` and `rm`

While using a graphical file manager may seem easier for copying a single file, cp and rm shine when you need to copy or remove multiple files at once. 
This is where the true power of these commands, combined with globs and wildcards, becomes apparent.

#### Understanding Globs and Wildcards

Globs and wildcards are patterns that allow you to match filenames in a flexible way. 
For example, the asterisk (*) wildcard matches any number of characters. 
If you want to copy all PNG images from one directory to another, you could use:

```sh
cp *.png /destination/directory/
```

This command copies all files ending with .png in the current directory to `/destination/directory/`. 
Without the command line, selecting all PNG files for copying could involve manually clicking each file in a GUI file manager.

#### The `-i` flag

The ``-i`` option stands for "interactive" and changes how `cp` and `mv` (move) commands work. 
By default, `cp` and `mv` will overwrite files without asking. This can be dangerous if you accidentally overwrite important files. 
The ``-i`` option prompts you before overwriting, giving you a chance to cancel the operation.

Consider you're copying a file named report.txt to a directory that already has a file with the same name. 
Using cp alone would overwrite the existing file without warning. 
With `cp -i`, you're prompted to confirm the overwrite, reducing the risk of accidental data loss.

```sh
cp -i source/report.txt destination/
```

If `report.txt` exists in `destination/`, you'll see a prompt like:

```sh
overwrite destination/report.txt? (y/n)
```

The same thing is also true for the `mv` command. If you use the `mv` command and the file exists, then it would be overwritten.
With the `-i` flag, you will be asked if you want to overwrite it.

#### The Dangers of `rm`

The rm command in Unix-like operating systems is a powerful tool for deleting files and directories. 
However, its power comes with significant risks if used improperly. Mistakes made with rm can lead to irreversible data loss. 
There are safer alternatives, such as `trash-cli`, which moves files to a trash folder instead of permanently deleting them, 
providing a safety net for recovery. 

A common mistake involves the misuse of wildcards. For example:

* `rm *.html` deletes all files in the current directory that end with .html.
* `rm * .html` (there is a space too much) attempts to delete all files in the directory (*) and additionally looks for a file literally named .html. 
If there's a space between * and .html, the command is interpreted as two separate arguments, which can lead to unintended consequences, such as deleting more files than intended.

The rm -rf Command

The -r option tells rm to delete directories and their contents recursively, and -f forces deletion without prompting for confirmation. Together, rm -rf can be extremely dangerous:

* Running `rm -rf /` as root would attempt to delete every file on the system, rendering it unusable.
* Even more limited commands, like `rm -rf *` in an important directory, can lead to significant data loss.

> When you use `rm` especially with the `-r` and/or `-f` flag should be used _extremely carefully_. In contrary to other operating systems, on Linux you can literally delete anything.
Even your boot loader or files that are crucial for your OS to work properly. In general be careful with wildcard usage and if you are new to Linux, use something like `trash-cli`, to avoid deleting data forever.

> The usage of this command is not a joke, real companies went down due to wrong usage of this command 
Read this [article](https://www.independent.co.uk/tech/man-accidentally-deletes-his-entire-company-with-one-line-of-bad-code-a6984256.html)

_I am not kidding, read this article fully, and try not to use this command_

_Still waiting for you to read it_

### Utility Commands

In this chapter we will introduce a few more commands that make it easy to write to files via
the command line.

1. `echo <something>` just prints the thing after the `echo` to the console

```sh
> echo "Hello, world!" # print "Hello, world!"
Hello, world!
```

2. `cat <filename>` will output the content of the file with the given name to the console.
   Assuming we have a file called `example.txt` with the content `Hello, I am an example.`

```sh
> cat example.txt
Hello, I am an example.
```

2. `>` This is technically not a command, but this operator allows you to write the content
   left of `>` into the file specified right of the `>`. For example, if we would like to create a file
   named `example.txt` and put the content `Hello, world!` into it, we would execute the following.

```sh
> touch example.txt
> echo "Hello, world!" > example.txt
> cat example.txt # To test if the content is actually in the file
Hello, world!
```

## Symbolic Links

Symbolic links (also called _symlinks_), are special types of files that act as pointers or
references to another file or directory. Unlike a regular file, a symlink does not contain the
actual data of the file; instead, it simply contains the path to its target file or directory.

Symbolic links provide an easy way to create shortcuts or references to other files and
directories without duplicating the actual content. For instance, in scenarios where multiple
applications or scripts require access to the same file or directory, instead of copying the
file multiple times, which consumes additional disk space and complicates updates,
a symlink can be created. This symlink points to the original file or directory, ensuring
all applications reference the same data. Additionally, symlinks are particularly useful for
maintaining compatibility with legacy file paths, as they can redirect old paths to new locations.

There are two types of symlinks. Soft links and hard links.

### Soft Links

Soft links are pointers to the original file or directory. They are independent files that contain the path
of the target file or directory. If the original file is moved or deleted, the soft link
becomes a `dangling` link, pointing to a non-existent location.

Lets create a symlink.

First lets create the initial file `original.txt`.

```sh
touch original.txt
```

Now lets create the symlink:

```sh
ln -s original.txt softlink.txt
```

If you now have a look at the file softlink.txt, you will see that the file is empty.

```sh
> cat softlink.txt
```

Now lets add some data into the `original.txt`, for example "Hello, world!".
And now display the content of the `symlink.txt` file.

```sh
> echo "Hello, world!" > original.txt
> cat softlink.txt
Hello, world!
```

You will not see that the `softlink.txt` file also contains the content we have written
originally ino the `original.txt`.

You can also edit the symlink file and see that content inside the `original.txt` is also changed.

```sh
> echo "Hello, softlink file!" > softlink.txt
> cat original.txt
Hello, softlink file!
```

You can also see where the softlink is pointing to by using `readlink -f <filename>`.

```sh
> readlink -f softlink.txt
/home/user/original.txt
```

As mentioned previously, if you delete the original file, then the softlink will be invalid.

```sh
rm original.txt
ls -la softlink.txt # will still point to original.txt
cat softlink.txt # cat: softlink.txt: No such file or directory
```

### Hardlinks

Hard links are direct references to the physical data of the file of the disk. Unlike soft liks,
they do not contain the path to the original file but share the same `inode`.

Deleting or moving the original file does not affect the hard link as both the original file
and the hard link reference the same data on the disk. Hard links cannot be created for directories.

Now lets create a hardlink:

```sh
rm original.txt
touch original.txt
echo "Hello, world!" > original.txt
ln original.txt hardlink.txt
cat hardlink.txt # Hello, world!
```

As you can see, the same data, that we put into the original.txt, is in the `hardlink.txt` file.
But in contrary to softlinks, if we now delete the `original.txt`, the content will still be
in the `hardlink.txt`.

```sh
rm original.txt
cat hardlink.txt # Hello, world!
```

### Deep Dive into Links

To better understand the use cases of links lets assume the following situation:

Imagine a scenario where a program needs to use a shared resource, like a file named "foo," which often changes versions.
It would be helpful to include the version number in the filename so anyone can easily see which version of "foo" is currently installed.
However, this introduces a challenge. 
Every time a new version is installed and the file name changes, we would need to update every program that uses this resource to look for the new name.
This process is cumbersome and far from fun.

This is where symbolic links come to the rescue. Let's say we install version 2.6 of "foo," giving it the filename "foo-2.6," 
and then create a symbolic link called "foo" that points to "foo-2.6." 
Now, when a program opens "foo," it's actually accessing "foo-2.6." This arrangement keeps everyone satisfied. 
Programs can still locate "foo," and the current version installed is transparent.

When the time comes to upgrade to "foo-2.7," we simply introduce the new file, remove the old symbolic link "foo,"
and establish a new link to "foo-2.7." This method not only addresses the version update issue but also allows us to keep multiple versions on our system. 
If "foo-2.7" turns out to have a bug, reverting to the previous version is as simple as updating the symbolic link to point back to "foo-2.6."

## Filesystem Hierarchy Standard

The **FHS (_Filesystem Hierarchy Standard_)** consists of a set of requirements and guidelines
for file and directory placement under UNIX-like operation systems. They are intended to support
interoperability of applications and greater uniformity of these systems.

Understanding and knowing the base structure of your system is essential for doing debugging
and understanding the next chapters, as most of the concepts that will come later are represented
via files. As in unix _everything is a file_.

### Directory Structure

In the **FHS** all files and directories appear under the root directory `/`, no matter where
they are actually stored (for example on different physical or virtual devices). We will cover
the most important directories and files, but we recommend to also skip through the actual
[**FHS Linux docs**](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html).

#### The Root Filesystem

- `/` - this is the _primary hierarchy_ root and the root directory for the whole FHS.
- `/bin` - this directory holds essential user commands binaries that can be used by all users.
  For example the binaries for most of the commands that you learned previously are located here
  (e.g. cat, cp, echo, and many more that you will also learn in the later chapters of this book).
- `/boot` - this directory contains the static files for the boot loader. _I would really recommend
  you not deleting this directory_
- `/dev` - holds special files or device files
- `/etc` - contains host-specific system configuration files. Configuration files are local
  files that are used to control the operation of a program. They have to be static and cannot
  be an executable binary.
- `/home` - contains the users home directory, this is where you will be spending most of your time
- `/lib` - holds shares library images needed to boot the system and run the commands in the
  root file system, e.g. by binaries in `/bin` and `/sbin`
- `/media` - contains subdirectories which are used as mount points for removable media devices
  such as floppy disks and cdroms
- `/mnt` - is the mount point for a temporarily mounted file system. The content of this
  directory is a local issue and should not affect the manner in which any program is run.
  You should not use this directory for installation programs
- `/opt` - is reserved for add-on application software packages
- `/root` - home directory for the root user. This location is the recommended default location
  for the root account's home directory and can be determined by the developer
- `/run` - this directory contains system information data describing the system since it was
  booted. Those files must be cleared at the beginning of the boot process
- `/sbin` - contains utilities used for system administration, e.g. root-only commands. Commands
  here are essential for booting, restoring, recovering and/or repairing the system
- `/tmp` - contains temporary files which are sometimes needed by files. Programs must not asumme
  that any files in this directory are preserved between the invocations of the program

Now it is time to see for yourself. Navigate to the root directory and show yourself the contents
of it (you should be able to already do this yourself).

_I give you a moment to do this yourself and i will reveal it now_

```sh
cd /
ls -la
```

Now try to find every directory we previously talked about and explore. Especially inside the
`/bin` directory you can search for the binaries we introduced in the previous chapters.

#### The `/usr` Hierarchy

The `/usr`directory is the second major section of the filesystem. It is shareable and only contains read-only data.
Large software packages must not use a direct subdirectory under the `/usr` hierarchy.

- `/usr/bin` - This is the primary directory of executable commands on the system. There must not be
  any subdirectories in `/usr/bin`
- `/usr/sbin` - contains any non-essential binaries used exlusively by the system administrator.

There are many more, but subdirectories in the `/usr` directory, but they are no essential to understand. If you
want to have more information, you can explore it [here](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch04.html).

#### The `/var` Hierarchy

The `/var` directory contains variable data files. This includes spool files (files that contain data which is awaiting some
kind of processing, e.g. files that will be printed by a printer later), administrative data, loggin data and temporary files.

We will not go through every subdirectory here as the exact subdirectories will be mostly not relevant in the later chapters.
If you are interested, you can read about it [here](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch05.html).

This section might have seem dull, but knowing this, or at least understanding where specific files are located, helps you
extremely when developing, understanding the concept of the things that come later and helps you debug problems, that you
would otherwise never be able to debug.

> That said, if you do not know what you are doing, you should NOT edit or remove directories or files located in these
> directories. _It seems pretty obvious, that you should not remove the `/boot` or `/bin` or any other directories inside the `/` directory._

Moreover you can also only edit those files when being the root user. Are you interested in what the root user is?

_Then stay tuned for the next season of `Introduction to Linux`._ 

In the next chapter, we will discuss users and groups, which is how unix-like systems manage permissions for different users. 
So that not every user, can just delete you `/boot` directory and kill your whole system.
