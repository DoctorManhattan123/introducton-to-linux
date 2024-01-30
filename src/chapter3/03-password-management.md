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
2. Login as this user
3. Logout
4. Disable password login to this user
5. Try log in as the user
6. Unlock the account
7. Login into the account
8. Logout
9. remove the user and the group

_I will show you all of the commands below, but first try it our yourself_

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


