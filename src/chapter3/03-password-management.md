# Password Management

The command which you will use for most of the things related to passwords is the `passwd` command.

To change a users password you can type:

```sh
sudo passwd <username>
```

You can also lock and unlock accounts (preventing people from logging into them):

```sh
sudo passwd -l <username> # lock
sudo passwd -u <username> # unlock
```

You can also force the user to change the password at next login:

```sh
sudo passwd -e <username>
```

Now lets do the following:

1. Create a new user
2. Change the password of the user to a password of your liking
3. Login as this user
4. Logout
5. Disable password login to this user
6. Try log in as the user
7. Unlock the account
8. Login into the account
9. Logout
10. remove the user and the group

_I will show you all of the commands below, but first try it out yourself_

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
