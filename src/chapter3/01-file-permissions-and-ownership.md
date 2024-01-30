# Permissions

_Everything is a file_. Most things in Linux will be managed through files: documents, directories,
hard-drives, CD-ROMs, modems, keyboards, monitors, and more. Therefore you can manange most of the permissons
on a system via managing permissions on files:

Every file on a Linux filesystem is owned by a user and a group, there are three types of access permissions:

- read
- write
- execute

Different access permissions can be applied to a files user, owning group and others (those without permissions).
You already saw the permissions of a file when executing `ls -la`:

```sh
$ ls -la boot

total 115911
drwxr-xr-x  5 root root     1024 Jan  1  1970  ./
drwxr-xr-x 18 root root     4096 Dec 29 21:25  ../
drwxr-xr-x  6 root root      512 Apr  3  2023  EFI/
drwxr-xr-x  6 root root      512 Apr  3  2023  grub/
-rwxr-xr-x  1 root root 72955758 Jan  4 15:59  initramfs-linux-fallback.img
-rwxr-xr-x  1 root root 32842740 Jan  4 15:59  initramfs-linux.img
-rwxr-xr-x  1 root root 12886816 Jan  4 15:59  vmlinuz-linux
```

The first column displays the file's permissions (for example the file `initramfs-linux.img` has the permissions
`-rwxr-xr-x`). The third and fourth column displays the file's owning user and group, respectively. In this example
all files are owned by the user `root` and the group `root`.

The first column is split into four parts, the first letter, and three pairs of three letters:

![](images/file_permissions.png)

Each pair can have up to three letters `rwx`, where `r` means read access is enabled, `w` means that
write access is enabled and `x` means that execute permissions are given. If there is a `-` instead
it means that this permission is not given. Given the following file example:

`-rwxr-xr-x 1 root root 1113504 Jun  6  2019 /bin/bash`

- First letter indicates that this is a file and not a directory
- First pair indicates that the root user has read, write and execute permissions
- Second pair indicates that the root group has read and execute (no write) permissions
- Third pair indicates that all other users have read and execute (no write) permissions

In reality this are just bits being set or unset, for example:

```
rwx rwx rwx = 111 111 111 = 7 7 7
rw- rw- rw- = 110 110 110 = 6 6 6
rwx --- --- = 111 000 000 = 7 0 0
```

Keep this is mind, as when you set permissions, you give a number to indicate the new permissions.
Lets create a file:

```sh
$ cd
$ touch permissions.txt
$ ls -la permissions.txt
-rw-r--r-- 1 alex alex 0 Jan 30 08:48 permissions.txt
```

Lets change the permissions, so every user has `rw` permissions, but not execute permissions. The corresponding
number would be:

```
rw- rw- rw- = 110 110 110 = 6 6 6
```

To change the permissions of a file we can run the `chmod <new-permissions> <filename>` command:

```sh
$ chmod 666 permissions.txt
$ ls -la permissions.txt
-rw-rw-rw- 1 alex alex 0 Jan 30 08:48 permissions.txt
```

Now every user can read and write to this file.
It is time for you to try removing the access for `other users`, so they
cannot even read anymore, keep the rest of the permissions as it is.

_I'll wait a second_

```sh
$ chmod 660 permissions.txt
$ ls -la permissions.txt
-rw-rw---- 1 alex alex 0 Jan 30 08:48 permissions.txt
```

The same principle applies to directories, although the `rwx` means slightly different things. You use
this definitions if the first bit is `d` (so it is a directory) instead of `-`.

- r - allows the contents of the directory to be lsted if the x attribute is also set
- w - allow files within the directory to be created, deleted, or renamed if the x attribute is also set
- x - allows a directory to be entered (e.g. cd dir)

Lets create a folder and put a file in there and list the permissions of the folder:

```sh
$ mkdir permissions-folder
$ touch permissions-folder/example.txt
$ ls -la permissions-folder

total 8
drwxr-xr-x  2 alex alex 4096 Jan 30 08:56 ./
drwx------ 70 alex alex 4096 Jan 30 08:55 ../
-rw-r--r--  1 alex alex    0 Jan 30 08:56 example.txt
```

Now this looks a bit different from before. The the outer rigth column, you can see the names of the
file/directories inside the directory. You see the `example.txt` file, but what are the other two?

- `./` - points to the current directory, try it out (first `cd` into the `permissions-folder`):
  `cd .` and you will stay in the same directory
- `../` - points to the parent directory, try it out (first `cd` into the `permissions-folder`):
  `cd ..` and you will move to the parent directory

Before continuing make sure, that you are in the parent folder in permissions-folder. Lets remove the
`x` permissions from this folder and try to `cd` into it.

```sh
$ chmod 666 permissions-folder
$ cd permissions-folder
cd: Permissions denied: 'permissions-folder/'
```

Now you restricted your access to this directory. If you try to list the content of the directory,
you will also see almost nothing:

```sh
$ ls -la permissions-folder
ls: cannot access 'permissions-folder/example.txt': Permission denied
ls: cannot access 'permissions-folder/.': Permission denied
ls: cannot access 'permissions-folder/..': Permission denied
total 0
d????????? ? ? ? ?            ? ./
d????????? ? ? ? ?            ? ../
-????????? ? ? ? ?            ? example.txt
```

So you see the filenames, but you cannot see anything else (no permissions, no size, no created date, etc.)

Important commands:

- chmod <new-permissions> <filename>
