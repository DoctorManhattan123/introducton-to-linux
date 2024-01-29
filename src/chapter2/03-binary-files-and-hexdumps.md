# Binary Files and Hexdumps

Binary files are computer files that are not text files. They are encoded in binary format,
which means they consist of binary data (a series of 0s and 1s). Unlike text files,
which store data in a readable format using characters (like letters, numbers, and symbols),
binary files store data in a format that is typically not human-readable.

There are a lot of use cases for binary files, for example executable, like `.exe` file on windows,
media files (e.g. `.jpg`, `.png` or `.mp3`). For example download the following picture
by right clicking on it and pressing `Save image as` and save it into your home folder.

![](images/rust-book.png)

Now run `cat rust-book.png` and you will get some weird collection of text, where most of the content
is not human readable. The reason is pretty simple, as the `cat` command tries to interpret the
binary data inside this files as `utf-8` encoded text, which it is not.

To actually see the 1s and 0s in hexadecimal format we can use the `xxd` command.

```sh
xxd rust-book.png
```

After running this command you will see the corrext so called `hexdump` of the file. You will
also see the ASCII representation of this hexdump to the right, where most of the stuff will be
unreadable to you, but there will be a few things that is actually text, for example in the
beginning of the file you can see `.PNG` and later you read text like `Adobe ImageReader`.

> Note: Often used formats (like `.pdf`, `.png`, `.jpg`) start with a specific sequence which
> describes the file format. This is usually called `file signature` and is used by editors
> like `Adobe Reader` to identify the file format. Try it out yourself! Download some pdf, and
> hexdump it, most likely you will get `.PDF` in the beginning of the file. This is particularly
> interesting, because this means, that often the file ending, does not really play a role, but the
> actual file format is found in the binary data of the file itself.

> Small story: There was once a person, that just renamed all of the endings of the `.png` files
> to `.pdf` and thought that now no person could find out that there are `.png` file on the disk.
> But as the person did not read this book, they could not know, that the actual file format is
> also found in the hexdump of the file itself.
