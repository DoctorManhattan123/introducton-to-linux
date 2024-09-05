# Package Management

## Basics

A **package manager** is a program that automates the process of - well - managing packages.
This includes installing, upgrading and removing packages as well as other operations.

## The Advanced Package Tool

### Repositories

The most used package manager for Ubuntu/Debian is the Advanced Package Tool (**apt** for short).

APT relies on **repositories** to find software and resolve dependencies.

These repositories are mostly contained in the file `/etc/apt/sources.list`.
Additional repositories are often listed in the `/etc/apt/sources.list.d` directory.

This is how an `/etc/apt/sources.list` file might look like:

```console
$ cat /etc/apt/sources.list
deb http://de.archive.ubuntu.com/ubuntu/ jammy main restricted

## Major bug fix updates produced after the final release of the distribution.
deb http://de.archive.ubuntu.com/ubuntu/ jammy-updates main restricted

## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu team.
## Also, please note that software in universe WILL NOT receive any review or updates from the Ubuntu security team.
deb http://de.archive.ubuntu.com/ubuntu/ jammy universe
deb http://de.archive.ubuntu.com/ubuntu/ jammy-updates universe

## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu team, and may not be under a free license.
## Please satisfy yourself as to your rights to use the software.
## Also, please note that software in multiverse WILL NOT receive any review or updates from the Ubuntu security team.
deb http://de.archive.ubuntu.com/ubuntu/ jammy multiverse
deb http://de.archive.ubuntu.com/ubuntu/ jammy-updates multiverse

## N.B. software from this repository may not have been tested as extensively as that contained in the main release,
## although it includes newer versions of some applications which may provide useful features.
## Also, please note that software in backports WILL NOT receive any review or updates from the Ubuntu security team.
deb http://de.archive.ubuntu.com/ubuntu/ jammy-backports main restricted universe multiverse

deb http://security.ubuntu.com/ubuntu jammy-security main restricted
deb http://security.ubuntu.com/ubuntu jammy-security universe
deb http://security.ubuntu.com/ubuntu jammy-security multiverse
```

Each line consists of these parts:

The `deb` indicates a binary repository (as opposed to `deb-src` which indicates a source repository).

This is followed by the URL of the repository.

This is followed by the codename for the Ubuntu release (which is `jammy`, i.e. Ubuntu version `22.04`).

The last parts indicate the section of the repository, where:

- `main` is officially supported software
- `restricted` is supported software that is not available under a completely free license
- `universe` is community-maintained software (open-source but not officially supported)
- `multiverse` is non-free software

Additionally, you might have lists in `sources.list.d`.
For example, when you install Docker, you will usually create a file `/etc/apt/sources.list.d/docker.list` containing the following content:

```
deb [arch=amd64 signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu   jammy stable
```

### Updating

You can run the following command to fetch available packages from the locations specified in `/etc/apt/sources.list` and `/etc/apt/sources.list.d`:

```sh
sudo apt update
```

During the update, you will see that the command hits the repositories one by one and performs the update.

Note that this does not actually upgrade or change any packages - it merely retrieves the newest information about these packages.

### Upgrading

You can use the `upgrade` command to actually install new versions of packages:

```sh
sudo apt upgrade
```

Note that you should always run `apt update` before `apt upgrade` so that your machine is aware of the newest package versions.

The `sudo apt update` followed by a `sudo apt upgrade` is something you should regularly execute to get the newest package versions.

A slightly different way to upgrade is to use the `full-upgrade` command:

```sh
sudo apt full-upgrade
```

This will intelligently handle changing dependencies with new package versions.
Basically, APT has a "smart" conflict resolution system and will attempt to upgrade the most important packages at the expense of less important ones.
It might even remove installed packages if that is required in order to resolve a package conflict.

### Installation

You can install a package like this:

```sh
sudo apt install $PACKAGE_NAME
```

For example, if you want to install the package `trash-cli` (which contains the `trash-put` tool), you would run:

```sh
sudo apt install trash-cli
```

### Removing Packages

To remove a package, you use:

```sh
sudo apt remove $PACKAGE_NAME
```

Note that removing a package will leave its configuration files on the system.
If you want to delete configuration files in addition to removing a package, you can purge it:

```sh
sudo apt purge $PACKAGE_NAME
```

### Searching for Packages

You can use `apt search $KEYWORD` to search for packages and `apt show $PACKAGE_NAME` to display package information.

### Managing PPAs

Personal Package Archives (PPA) allow you to upload Ubuntu source packages to be built and published as an apt repository by Launchpad.
They basically provide a way for individual developers to distribute software to users in an easy way.

You can add a PPA like this:

```sh
sudo add-apt-repository ppa:$PPA_NAME
sudo update
```

You can then install packages from PPA by simply running the `apt install` command.

You can remove a PPA like this:

```sh
sudo add-apt-repository --remove ppa:$PPA_NAME
```

## The `dpkg` Tool

The `dpkg` tool is the backend of `apt` and does the low-level operations.

You can install a package like this:

```sh
sudo dpkg -i $PACKAGE_NAME.deb
```

You can remove a package like this:

```sh
sudo dpkg -r $PACKAGE_NAME.deb
```

You can purge a package like this:

```sh
sudo dpkg --purge $PACKAGE_NAME.deb
```

You can list installed packages like this:

```sh
dpkg -i
```
