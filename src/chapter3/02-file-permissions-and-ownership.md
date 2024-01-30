# Users and Groups

## Users

Now after unstanding file permissions, and as _everything is a file_, most of the permissions are
managed exactly this way, we will look at how to manage users.

First lets see which user you are. With the `whoami` command, you can see the current user.

```sh
> whoami
username
```

> For a lot of the following operations we will need root priviliges as adding users and setting
> passwords is a very powerful operation, therefore only users with root priviliges are able to
> execute some of the commands. To imitate a root user, you need to prepend the command with `sudo`:

```sh
$ ls -la
$ sudo ls -la
```

This will prompt you to input your password to authenticate, that you are the real user.

You will probably see you name here, for example when I executed this command, I got `alex`.

Now lets add a user:

```sh
$ useradd -m exampleuser
useradd: Permission denied.
useradd: cannot lock /etc/passwd; try again later.
```

Well, you already know how to fix this, _Try fixing it yourself_

```sh
$ sudo useradd -m exampleuser
```

We can also password protect the new user, this is done via the `passwd` command.

```sh
$ sudo passwd exampleuser
```

You can also change the users name:

```sh
$ sudo usermod -l renameduser exampleuser
```

You will also see that the new user has its own home directory:

```sh
$ ls /home/
alex/  exampleuser/
```

To login as the new user, you can use the `su` command:

```sh
su - renameduser
```

To logout, you can just use the `exit` command.

You can also delete a user (the `-r` option specifies that the home directory and mail spool of the user
should also be deleted):

```sh
$ sudo usermod -r renameduser
```

## Groups

You look up all the groups that a user belongs to. In the following commands, just replace
<username> with your actual username which you saw above.

```sh
$ groups alex
power wheel input storage video username
```

The exact groups will of course depend on your current setup, but lets explain some of them:

- wheel - This group is a special user group to control access to the `su` and `sudo` command, which allows
  you to pretend to be another user, usually the superuser/root user. If you are on a Debian-like system, this
  group will probably be called `sudo`. With the sudo command you can pretend to be the root and execute
  commands with root permission. _And yes, with this, you will be able to delete the `/boot` directory_.
  **Please DO NOT do this, for your own sake**.

> To execute a command via root user, you just prepend the command you want to execute with `sudo`. For example:

```sh
sudo ls -la
```

If the you try to execute a command which needs root previlige and you do not run it with `sudo` or as
a user with root previlige, then you will get a `Permission denied` error.

You will most likely be asked to input your password, as not everyone should be able to execute commands
with root privilige.

- input - gives you access to input devices like keyboard, mice and other human interfaces
- power - gives you access to power related commnands
- storage - might give you permission to mount or unmount
- video - gives you access to video and hardware operations
- username - in most distributions the user is also in its own group, the groupname matches the username

As you remember _everything is a file_, so this information has to present somewhere. To see this info,
run `passwd -Sa` as the root user (you should know how to do this).

```sh
sudo passwd -Sa
```

This lists all of the user accounts and their properties. Most of the users are not really interesting for us,
but you will see the presence of the `root` user and the presence of your user, in my case `alex`.

You can also get the id of the user:

```sh
$ id alex
uid=1000(alex) gid=1000(alex) groups=1000(alex),98(power),998(wheel),994(input),987(storage),985(video)
```

You can also list all groups, currently present in the system:

```sh
cat /etc/group/
```

Lets create a new group and add the user to the group

```sh
$ sudo groupadd examplegroup
$ sudo gpasswd -a alex examplegroup # gpasswd -a <user> <group>
```

> For this changes to take effect, you need to logout and login again

You can also rename groups:

```sh
$ sudo groupmod -n renamedgroup examplegroup # groupmod -n <new-group-name> <old-group-name>
```

And delete existing groups and remove user from a group:

```sh
$ sudo gpasswd -d alex renamedgroup # gpasswd -d <user> <group-name> # remove users from a group
$ sudo groupdel renamedgroup # groupdel <group-name-to-delete> # delete existing groups
```

There are a lot of specific groups, we covered a few of them above, but you can find a more complete
list [here](https://wiki.archlinux.org/title/users_and_groups#Group_list)

