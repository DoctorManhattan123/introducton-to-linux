# Filesystem Hierarchy Standard

The **FHS (\_Filesystem Hierarchy Standard)** consists of a set of requirements and guidelines
for file and directory placement under UNIX-like operation systems. They are intended to support
interoperability of applications and greater uniformity of these systems.

Understanding and knowing the base structure of your system is essential for doing debugging
and understanding the next chapters, as most of the concepts that will come later are represented
via files. As in unix _everything is a file_.

## Directory structure

In the **FHS** all files and directories appear under the root directory `/`, no matter where
they are actually stored (for example on different physical or virtual devices). We will cover
the most important directories and files, but we recommend to also skip through the actual
[**FHS Linux docs**](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html).

### The root filesystem

- `/` - this is the _primary hierarchy_ root and the root directory for the whole FHS.
- `/bin` - this directory holds essential user commands binaries that can be used by all users.
  For example the binaries for most of the commands that you learned previously are located here
  (e.g. cat, cp, echo, and many more that you will also learn in the later chapters of this book).
- `/boot` - this directory contains the static files for the boot loader. _I would really recommend
  you not deleting this directory_
- `/dev` - holds special files or device files, TODO: example
- `/etc` - contains host-specific system configuration files. Configuration files are local
  files that are used to control the operation of a program. They have to be static and cannot
  be an executable binary.
- `/home` - contains the users home directory, this is where you will be spending most of your time
  as this is where your data like downloads, files in general are located
- `/lib` - holds shares library images needed to boot the system and run the commands in the
  root file system, e.g. by binaries in `/bin` and `/sbin`
- `/media` - contains subdirectories which are used as mount points for removable media devices
  such as floppy disks and cdroms
- `/mnt` - is the mount point for a temporarily mounted file system. The content of this
  directory is a local issue and should not affect the manner in which any program is run.
  You should not use this directory for installation programs
- `/opt` - is reserved for add-on application software packages
- `/root` - home directory for the root user. This location is the recommend default location
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

### The `/usr` hierarchy

The `/usr`directory is the second major section of the filesystem. It is shareable and only contains read-only data.
Large software packages must not use a direct subdirectory under the `/usr` hierarchy.

- `/usr/bin` - This is the primary directory of executable commands on the system. There must not be
  any subdirectories in `/usr/bin`
- `/usr/sbin` - contains any non-essential binaries used exlusively by the system administrator.

There are many more, but subdirectories in the `/usr` directory, but they are no essential to understand. If you
want to have more information, you can explore it (here)[https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch04.html].

### The `/var` hierarchy

The `/var` directory contains variable data files. This includes spool files (files that contain data which is awaiting some
kind of processing, e.g. files that will be printed by a printer later), administrative data, loggin data and temporary files.

We will not go through every subdirectory here as the exact subdirectories will be mostly not relevant in the later chapters.
If you are interested, you can read about it [here](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch05.html).

This section might have seem dull, but knowing this, or at least understanding where specific files are located, helps you
extremely when developing, understanding the concept of the things that come later and helps you debug problems, that you
would otherwise never be able to debug.

> That said, if you do not know what you are doing, you should NOT edit or even remove directories or files located in these
> directories. _It seems pretty obvious, that you should remove the `/boot` or `/bin`._

Moreover you can also only edit those files when being the root user. Are you interested in what the root user is?

_Then stay tuned for the next season of `Introduction to Linux`._ No really, in the next chapter, we will discuss users and groups,
which is how unix-like systems manage permissions for different users. So that not every user, can just delete you `/boot` directory
and kill your whole system.
