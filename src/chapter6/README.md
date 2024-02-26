# Package Management

Package management simplifies software handling by centralizing installations, updates, and removals. 
This approach offers streamlined dependency resolution and enables one-command updates for all installed packages. 
Unlike manual updates in Windows, where each application requires individual attention, package management ensures efficiency by managing everything through a single tool.

## Basic Usage

Package managers are software tools designed to automate the process of installing, updating, and removing software packages on a computer system. 
They act as a centralized hub for managing software installations, ensuring efficiency and consistency across the system. 
For instance, in Ubuntu, the apt package manager handles system-level installations, facilitating commands like apt install <package> to install software like "gcc" or "nginx". 
On macOS, Homebrew (brew) serves a similar purpose, allowing users to easily install and manage software packages via commands like brew install <package>. These package managers simplify the otherwise cumbersome task of manually downloading, configuring, and installing software, offering a seamless experience for users and system administrators alike.

For example lets consider a situation where we would like want to install git.

If you would want to do it manually, this would be the steps you need to take:

* Visit the Git website.
* Download the appropriate package for your operating system.
* Extract the downloaded file.
* Configure Git according to your system requirements.
* Compile the source code.
* Manually add Git to your system PATH.

With a package manager, you just need to execute the package installation command:

```sh
sudo apt install git
```

or on MacOS (brew):

```sh
brew install git
```

Furthermore lets consider that you have not updated your software for a long time.
To do this manually, you would need to go into every application you want to update, search for the option to update and manually update it.
To update all the software you installed via the package manager, you just need to do the following:

```sh
sudo apt update
sudo apt upgrade
```

or on MacOS (brew):

```sh
brew install update
```

## File Structure

To unravel the mystery working of package managers, we will have a deeper look at how they work, for example, where the packages are retrieved from or where they are placed inside of the file system.

> We will examine the following paradigms using the `apt` package manager for Ubuntu, most of the concept will be the same for most package managers.

`/etc/apt` contains the `APT` configuration folders and files.

### Package Repositories

Most of package managers use so called `package repositories` to identify the location, where the package manager should search for the package the user wants to install.
These repositories contain metadata about available packages including version numbers, dependencies and descriptions.
The main Ubuntu repository is maintained by `Canonical` and contains the most used packages.
There are even more repositories like `universe` and `multiverse` that include many more packages, including packages that are maintained by the community.

To see all of your current repositories that you can download from, you can have a look at the `/etc/apt/sources.list` file.

```sh
$ cat /etc/apt/sources.list
```

`APT` relies on the concept of repositories in order to find packages and resolve the dependencies for these packages.
A `repository` is nothing more than a directory containing packages along with an index file. 
This can be a networked (what you use normally) location, or compact disks or other storage media file in general.
The index file inside a repository (also called `Packages Index`) contains metadata about available packages.
It includes all of the relevant metadata like:

* package name
* current version
* maintainer
* dependencies
* filename
* size
and a few more...

An individual entry will look like this:

```
Package: git
Architecture: amd64
Version: 1:2.34.1-1ubuntu1
Multi-Arch: foreign
Priority: optional
Section: vcs
Origin: Ubuntu
Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
Original-Maintainer: Jonathan Nieder <jrnieder@gmail.com>
Bugs: https://bugs.launchpad.net/ubuntu/+filebug
Installed-Size: 18344
Provides: git-completion, git-core
Depends: libc6 (>= 2.34), libcurl3-gnutls (>= 7.56.1), libexpat1 (>= 2.0.1), libpcre2-8-0 (>= 10.34), zlib1g (>= 1:1.2.0), perl, liberror-perl, git-man (>> 1:2.34.1), git-man (<< 1:2.34.1-.)
Recommends: ca-certificates, patch, less, ssh-client
Suggests: gettext-base, git-daemon-run | git-daemon-sysvinit, git-doc, git-email, git-gui, gitk, gitweb, git-cvs, git-mediawiki, git-svn
Breaks: bash-completion (<< 1:1.90-1), cogito (<= 0.18.2+), dgit (<< 5.1~), git-buildpackage (<< 0.6.5), git-el (<< 1:2.32.0~rc2-1~), gitosis (<< 0.2+20090917-7), gitpkg (<< 0.15), guilt (<< 0.33), openssh-client (<< 1:6.8), stgit (<< 0.15), stgit-contrib (<< 0.15)
Filename: pool/main/g/git/git_2.34.1-1ubuntu1_amd64.deb
Size: 3131314
MD5sum: 050ff983968dda80edb1b72156c3f961
SHA1: 4aa143af8eb6ab44bd652692c213a16d646b0437
SHA256: 0a0bd5c85eafb688025d68e69f49919591b22abf8b7d608d7e64307714d361ca
SHA512: 38f3d4bb869905a75c470eb8220b210cf3f0aedbe0d66111d26fc6995a1cb0b879dffbd1c6579e9f6c9c5dbd2e1ccd6bd3de7a9c0577da86f525c6077d3cb117
Homepage: https://git-scm.com/
Description: fast, scalable, distributed revision control system
Task: cloud-image, ubuntu-wsl, server, ubuntu-server-raspi, kubuntu-desktop, lubuntu-desktop, ubuntustudio-desktop-core, ubuntustudio-desktop
Description-md5: c1f968556452a190fe359bffd151c012
```
> This is a real entry from an index file from the ubuntu repository.
Download the file and unzip it and you can see the content of the index file:

```sh
$ curl http://archive.ubuntu.com/ubuntu/dists/jammy/main/binary-amd64/Packages.gz > Packages.gz
$ gzip -d Packages.gz
$ cat Packages # or just open the file with any file editor you might use
```

When you try installing the package or updating packages, this is the starting point for the package manager.
This file is also being cached when you run `apt update` and therefore if you have not updating you system in a long time,
new packages will not be found and updates will also not be found. 
Therefore you first need to update the repositories and the content of the repositories by running `apt update`.
This will refresh this file and cache the current version of this file on your local machine.
Afterwards you can run `apt upgrade` to actually upgrade the currently installed packages to their latest version.

For more understanding lets look at the url, and try to understand each component of the path and what it means:

```
http://archive.ubuntu.com/ubuntu/dists/jammy/main/binary-amd64/Packages.gz
http://<mirror-url>/ubuntu/dists/<ubuntu-release>/<component>/<architecture>/Packages.gz
```

* Mirror Url: refers to an alternative server hosting identical or synchronized copies of the original repository's contents.
The server the same purpose as the original repository but can be geographically distributed to improve download speeds.
Furthermore if one mirror is down, you can use another url to still get your packages.
For example one of the mirrors for `archive.ubuntu.com` is: `us.archive.ubuntu.com`.

* Ubuntu Release: this is the Ubuntu release, each major Ubuntu version (everytime the first number of the version changes), has a codename.
For example Ubuntu 20 is called `Focal Fossa` and Ubuntu 22 is called `Jammy Jellyfish`. You can have a look at the current Ubuntu release [here](https://wiki.ubuntu.com/Releases)

* Component: A component in the context of `APT` refers to a classification system to categorize packages. 
For example the `main` component contains packages that meet the Debian Free Software Guidelines (DFSG) and are fully supported by the maintainers of the current distribution.
For example the `contrib` component contains community-maintained packages and the `non-free` component contains packages that do not comply with the `DFSG` have some kind of licensing restrictions.
For more information about components, see [here](https://wiki.debian.org/SourcesList#Component)

* Architecture: This refers to your current CPU architecture for which the packages are compiled, the most widespread architectures are `amd64`, `i386`, `arm64`, and many more.
You can have a look at you current architecture with this command:

```sh
$ uname -m # this will work on most Linux repositories and most MacOS systems
x86_64 # amd64 
```

> Note: You can also see this in the appropriate `Release` file:
Navigate to the following url:

```
http://archive.ubuntu.com/ubuntu/dists/jammy/main/binary-amd64/Release
```

> Note that none of these things you will specify yourself (the package manager will do it for you),
but demystifying those thing is extremely important to get a better grasp of the system where you will spend a lot of time on, if you want to develop something.
