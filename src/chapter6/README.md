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
