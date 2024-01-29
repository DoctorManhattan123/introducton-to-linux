# Basic File Commands

In this chapter, we will explain the most basic commands regarding files.
I extremely recommend you to open the terminal and following along by also typing the commands
into the terminal and look at their outputs.

## Open the terminal

To open the terminal on Ubuntu, press `Ctrl` + `Alt` + `T`.

## Navigating the filesystem

1. **`ls` Command (List Files and Directories)**:

The `ls` command is used to view the contents of a directory (like folders and files).
When you open the Terminal, you're usually in your home directory. To see what's inside, type `ls` and press Enter.
To see more details about each file, type `ls -l` and press Enter. This command lists files with details like
permissions, owner, size, and modification date.
To include hidden files (files that start with a dot) in your listing, type `ls -a`.

> In Linux and Unix-like operating systems, hidden files are those that begin with a dot (.) in their filename.
> These files are usually not displayed by default when using file managers or the ls command,
> as they are often used for storing user preferences or system configuration settings.

2. **`cd` Command (Change Directory)**:

The `cd` command is used to move between directories.
For example, to go to the Documents directory, type `cd Documents` and press Enter.
To go back to the previous directory, type `cd ..`
To return to your home directory from anywhere, just type `cd` and press Enter.

3. **`pwd` Command (Print Working Directory)**:

The `pwd` command shows your current directory's path.
If you're not sure where you are in the filesystem, type `pwd` and press Enter.
It will display the full path of your current directory.

4. **mkdir Command (Make Directory)**:

To create a new directory, use the `mkdir` command.
For example, to create a directory named NewFolder, type `mkdir NewFolder` and press Enter.

5. **`touch` Command (Create a New File)**:

The `touch` command allows you to create a new, empty file.
For instance, to create a file named `example.txt`, type `touch example.txt` and press Enter.

6. **`mv` Command (Move or Rename Files/Directories)**:

The mv command is used to move files and directories from one location to another or to rename them.
To rename a file, type `mv oldfilename.txt newfilename.txt` and press Enter.
To move a file to a different directory, type `mv filename.txt /path/to/directory/` and press Enter.

7. **`cp` Command (Copy Files/Directories)**:

The `cp` command is used to copy files and directories.
To copy a file, type `cp file.txt destination/` and press Enter
(where destination/ is the directory where you want to copy the file).
To copy a directory and its contents, use the -r flag: `cp -r directory/ destination/`.

8. **`rm` Command (Remove Files/Directories)**:

The `rm` command is used to delete files.
To delete a file, type `rm filename.txt` and press Enter.
To delete a directory and its contents, use the -r flag: `rm -r directory/`.
Be cautious with this command, as it permanently deletes files and directories.

9. **find Command (Search for Files/Directories)**:

The find command is used to search for files and directories based on different criteria.
To find a file by name, type find `/path/to/search/ -name "filename.txt"` and press Enter.
You can also search based on file type, size, modification date, and more.

10. **`grep` Command (Search Within Files)**:

The grep command is used to search for specific text within files.
To search for a particular word or phrase in a file, type grep "search term" filename.txt and press Enter.
grep can also be used with options for case-insensitive search, searching recursively in directories, displaying line numbers, and more.

11. **`locate` Command (Find Files by Name)**:

The locate command is used to quickly find the location of files and directories by their names.
To find a file, type locate filename.txt and press Enter. It will display paths of all files and directories with "filename.txt" in their names.
The locate command relies on a database that is periodically updated by the system; to update this database manually, you can use the sudo updatedb command.

Not it is time to practice and do it yourself. Perform the following instructions by using the commands
you just learned. You can check the correctness of the current step by just viewing the contents
after doing a command.

1. Open the Terminal on your Ubuntu system.
2. View the contents of your current directory and then view the detailed information about each item
   in this directory.
3. Navigate to the "Documents" directory from your current location.
4. Check which directory you are currently in.
5. Create a new directory called "PracticeFolder" inside "Documents."
6. Move into the newly created "PracticeFolder."
7. Create a new file named "example.txt" in "PracticeFolder."
8. List the contents of "PracticeFolder" to verify the creation of "example.txt."
9. Create another directory inside "PracticeFolder" called "SubFolder."
10. Copy "example.txt" into "SubFolder."
11. Rename "example.txt" to "example-renamed.txt" within "PracticeFolder."
12. Move "example-renamed.txt" from "PracticeFolder" to "SubFolder."
13. Navigate back to the "Documents" directory.
14. Search for the file "example-renamed.txt" starting from "Documents."
15. Navigate into "PracticeFolder/SubFolder."
16. Write some text inside "example-renamed.txt" (you can use a text editor for this).
17. Search for a specific word or phrase within "example-renamed.txt" using grep.
18. Use locate to find the path of another file on your system (e.g., "example.txt").
19. Delete "example-renamed.txt."
20. Navigate back to "PracticeFolder" and delete "SubFolder."
21. Finally, navigate back to "Documents" and delete "PracticeFolder."

## Utility commands

In this chapter we will introduce a few more commands that make it easy to write to files via
the command line.

1. `echo <something>` just prints the thing after the `echo` to the console

```sh
> echo "Hello, world!" # print "Hello, world!"
Hello, world!
```

2. `cat <filename>` will output the content of the file with the given name to the console.
   Assuming we have a file called `example.txt` with the content `Hello, I am an example.`

```sh
> cat example.txt
Hello, I am an example.
```

2. `>` This is technically not a command, but this operator allows you to write the content
   left of `>` into the file specified right of the `>`. For example, if we would like to create a file
   named `example.txt` and put the content `Hello, world!` into it, we would execute the following.

```sh
> touch example.txt
> echo "Hello, world!" > example.txt
> cat example.txt # To test if the content is actually in the file
Hello, world!
```
