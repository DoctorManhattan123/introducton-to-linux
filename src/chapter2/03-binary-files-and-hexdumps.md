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

```sh
$ xxd rust-book.png

00000000: 8950 4e47 0d0a 1a0a 0000 000d 4948 4452  .PNG........IHDR
00000010: 0000 00c4 0000 00c4 0806 0000 00c0 a68e  ................
00000020: 6b00 0000 1974 4558 7453 6f66 7477 6172  k....tEXtSoftwar
00000030: 6500 4164 6f62 6520 496d 6167 6552 6561  e.Adobe ImageRea
00000040: 6479 71c9 653c 0000 15d1 4944 4154 78da  dyq.e<....IDATx.
00000050: ec5d 0b8c 5d45 199e de53 5ada 52bb 4091  .]..]E...SZ.R.@.
00000060: 224a 1710 2ac8 63bd 0b08 08f6 1651 a322  "J..*.c......Q."
00000070: ad06 838f 4416 8d4a 30b2 151f 3568 d245  ....D..J0...5h.E
...
...
...
```

After running this command you will see the so called `hexdump` of the file. You will
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
