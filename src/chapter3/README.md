# Users and Groups

In Unix-like systems, the concepts of users and groups are fundamental to the system's security and organization.
Users are the entities (people or processes) that interact with the system, while groups are collections of users that share certain permissions and access rights.

## Users

### Basics

A **user** is an account representing someone or something that interacts with the system (most of the time it will be you).
Each user has a unique identifier called the **user id** (**uid** for short).

Every user has their own home directory.

First, let's see which user you are.
With the `whoami` command, you can see the current user:

```console
$ whoami
someuser
```

In this case, the current user is `someuser`.

Different users have different privileges.
The most "privileged" user is usually `root` - this is similar to the concept of an "admin" user.

If you try to execute a command that needs root privileges and you do not run it with `sudo` or as a user with root privilege, then you will often get a `Permission denied` error.

### User Management

You can add a new user using the `useradd` command.
The `-m` flag will specify the home directory of the new user.

Note that `useradd` is a privileged command, i.e. this will not work:

```console
$ useradd -m exampleuser
useradd: Permission denied.
useradd: cannot lock /etc/passwd; try again later.
```

You need to execute this command as the `root` user or using `sudo`:

```sh
sudo useradd -m exampleuser
```

We can also password protect the new user, this is done via the `passwd` command.

```sh
sudo passwd exampleuser
```

You can also change the name of the user:

```sh
sudo usermod -l renameduser exampleuser
```

You will also see that the new user has their own home directory:

```console
$ ls /home/
alex/  exampleuser/
```

To log in as the new user, you can use the `su` command:

```sh
su - renameduser
```

To log out, you can just use the `exit` command.

You can also delete a user (the `-r` option specifies that the home directory and mail spool of the user should also be deleted):

```sh
sudo usermod -r renameduser
```

We can see all user account and their properties using the `passwd` command:

```sh
sudo passwd -Sa
```

This lists all of the user accounts and their properties. Most of the users are not really interesting for us, but you will see the presence of the `root` user and the presence of your user.

You can also get the uid of the user:

```sh
$ id someuser
uid=1000(someuser) gid=1000(someuser) groups=1000(someuser),24(cdrom),27(sudo),136(sambashare),999(docker)
```

### Groups

A **group** is a collection of users.
Each group has a unique identifier called the **group id** (**gid** for short).

You look up all the groups that a user belongs to. In the following commands, just replace `someuser` with your actual username which you saw above.

```sh
$ groups someuser
someuser sudo cdrom sambashare docker
```

The exact groups will, of course, vary depending on your machine.

You can also list all groups currently present in the system:

```sh
cat /etc/group
```

Let's create a new group and add the user to the group

```sh
sudo groupadd examplegroup
sudo gpasswd -a alex examplegroup
```

> For these changes to take effect, you need to log out and log in again

You can also rename groups:

```sh
$ sudo groupmod -n renamedgroup examplegroup # groupmod -n <new-group-name> <old-group-name>
```

You can also delete existing groups and remove users from a group:

```sh
$ sudo gpasswd -d alex renamedgroup # gpasswd -d <user> <group-name> # remove users from a group
$ sudo groupdel renamedgroup # groupdel <group-name-to-delete> # delete existing groups
```

There are a lot of specific groups; we covered a few of them above, but you can find a more complete list [here](https://wiki.archlinux.org/title/users_and_groups#Group_list)

### Password Management

The command which you will use for most of the things related to passwords is the `passwd` command.

To change a user's password you can type:

```sh
sudo passwd $USERNAME
```

You can also lock and unlock accounts (preventing people from logging into them):

```sh
sudo passwd -l $USERNAME # lock
sudo passwd -u $USERNAME # unlock
```

You can also force the user to change the password at the next login:

```sh
sudo passwd -e $USERNAME
```

Now, let's do the following:

1. Create a new user
2. Change the password of the user to a password of your liking
3. Log in as this user
4. Log out
5. Disable password login to this user
6. Try to log in as the user
7. Unlock the account
8. Log in to the account
9. Log out
10. Remove the user and the group

Here is the solution:

```sh
$ sudo useradd exampleuser # add the user
$ sudo passwd exampleuser # change the password
$ su - exampleuser # login as the example user
$ exit # logout
$ sudo passwd -l exampleuser # lock the user
$ su - exampleuser # login, this will fail
$ sudo passwd -u exampleuser # lock the user
$ su - exampleuser # login as the example user
$ exit # logout
$ sudo userdel -r exampleuser # delete the user
```

## Permissions

_Everything is a file_.

Most things in Linux will be managed through files: documents, directories, hard drives, CD-ROMs, modems, keyboards, monitors, and more.
Therefore, you can manage most of the permissions on a system via managing permissions on files:

Every file on a Linux filesystem is owned by a user and a group, there are three types of access permissions:

- read
- write
- execute

Each file is also owned by a user and a group.

Different access permissions can be applied to the owning user, owning group and others.

We already saw that `ls -la` prints the permissions:

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

The first column displays the file's permissions (for example, the file `initramfs-linux.img` has the permissions `-rwxr-xr-x`).
The third and fourth columns display the file's owning user and group, respectively.
In this example, all files are owned by the user `root` and the group `root`.

The first column is split into four parts, the first letter, and three pairs of three letters:

![](images/file_permissions.png)

Each part can have up to three letters `rwx`, where `r` means read access is enabled, `w` means that write access is enabled and `x` means that execute permissions are given.
If there is a `-` instead it means that this permission is not given.

Consider the following file example:

`-rwxr-xr-x 1 root root 1113504 Jun  6  2019 /bin/bash`

- The first sign indicates that this is a file and not a directory
- The first part indicates that the root user has read, write and execute permissions
- The second part indicates that the root group has read and execute (no write) permissions
- The third part indicates that all other users have read and execute (no write) permissions

In reality, these are just bits being set or unset, for example:

```
rwx rwx rwx = 111 111 111 = 7 7 7
rw- rw- rw- = 110 110 110 = 6 6 6
rwx --- --- = 111 000 000 = 7 0 0
```

Keep this in mind, as when you set permissions, you can give a number to indicate the new permissions.
Let's create a file:

```sh
$ cd
$ touch permissions.txt
$ ls -la permissions.txt
-rw-r--r-- 1 alex alex 0 Jan 30 08:48 permissions.txt
```

Let's change the permissions, so every user has `rw` permissions, but no `x` (execute) permissions.
The corresponding number would be:

```
rw- rw- rw- = 110 110 110 = 6 6 6
```

To change the permissions of a file we can run the `chmod <new-permissions> <filename>` command:

```sh
$ chmod 666 permissions.txt
$ ls -la permissions.txt
-rw-rw-rw- 1 alex alex 0 Jan 30 08:48 permissions.txt
```

Now, every user can read and write to this file.

Here is how we can remove all permissions for other users:

```sh
$ chmod 660 permissions.txt
$ ls -la permissions.txt
-rw-rw---- 1 alex alex 0 Jan 30 08:48 permissions.txt
```

The same principle applies to directories, although the `rwx` means slightly different things.
You use this definition if the first bit is `d` (so it is a directory) instead of `-`.

- r - allows the contents of the directory to be listed (e.g. via `ls`) if the x attribute is also set
- w - allows files within the directory to be created, deleted, or renamed if the x attribute is also set
- x - allows a directory to be entered (e.g. cd dir)

Let's create a folder and put a file in there and list the permissions of the folder:

```console
$ mkdir permissions-folder
$ touch permissions-folder/example.txt
$ ls -la permissions-folder

total 8
drwxr-xr-x  2 alex alex 4096 Jan 30 08:56 ./
drwx------ 70 alex alex 4096 Jan 30 08:55 ../
-rw-r--r--  1 alex alex    0 Jan 30 08:56 example.txt
```

Let's move to the parent directory:

```sh
cd ..
```

Now, let's remove the `x` permissions from `permissions-folder` and observe that moving to `permissions-folder` is no longer possible:

```sh
$ chmod 666 permissions-folder
$ cd permissions-folder
cd: Permissions denied: 'permissions-folder/'
```

If you try to list the content of the directory, you will see the following:

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
