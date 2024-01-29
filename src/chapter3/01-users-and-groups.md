# Users and Groups

## Users

A user is an account representing someone or something that interacts with the system (it will most of the
time be you). Each user has a unique identifier called user id (uid). Every user has its own
home directory (_you should know where the home directory is located by now_), which ensurs that
personal files and settings are kept private.

## Groups

A group as a collection of users. Each group has a unique identifier called the group id (gid).
Groups simplify permission management. Instead of assigning permissions to each user individually,
you can assign the permission to a group and each user of this group has this permission.

Now after the tedious _long_ definitions (_I tried to keep them small, so you do not fall asleep_), lets
dive and explore this in the commands line.

First lets see which user you are. With the `whoami` command, you can see the current user.

```sh
> whoami
username
```

You will probably see you name here, for example when i executed this command i got `alex`.

You can also look up all the groups that this user belongs to. In the following commands, just replace
<username> with your actual username which you saw above.

```sh
> groups username
power wheel input storage video username
```

The exact groups will of course depend on your current setup, but lets explain some of them:

* power - this group related to the power management priviliges of the system, which more or less
gives you the permission to shutdown, reboot and do other power related thing with your device. On the
first sight this seems kind of useless, as every user _should_ have the permission to poweroff the system.
But imagine you are on some critical hardware, which should never shutdown. In this case you would just
remove the `power` group from the user, and he will not be able to shutdown the system, _except of course
by banging throwing the laptop at the wall_
* wheel - This group is a special user group to control access to the `su` and `sudo` command, which allows
you to pretend to be another user, usually the superuser/root user. If you are on a Debian-like system, this
group will probably be called `sudo`. With the sudo command you can pretend to be the root and execute
commands with root permission. _And yes, with this, you will be able to delete the `/boot` directory_. 
**Please DO NOT do this, for your own sake**.

> To execute a command via root user, you just prepend the command you want to execute with `sudo`. For example:
```sh
sudo ls -la
```
You will most likely be asked to input your password, as not everyone should be able to execute commands
with root privilige.

* input - gives you access to input devices like keyboard, mice and other human interfaces

