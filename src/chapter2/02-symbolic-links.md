# Symbolic Links

Symbolic links (also called _symlinks_), are special types of files that act as pointers or
references to another file or directory. Unlike a regular file, a symlink does not contain the
actual data of the file; instead, it simply contains the path to its target file or directory.

Symbolic links provide an easy way to create shortcuts or references to other files and
directories without duplicating the actual content. For instance, in scenarios where multiple
applications or scripts require access to the same file or directory, instead of copying the
file multiple times, which consumes additional disk space and complicates updates,
a symlink can be created. This symlink points to the original file or directory, ensuring
all applications reference the same data. Additionally, symlinks are particularly useful for
maintaining compatibility with legacy file paths, as they can redirect old paths to new locations.

There are two types of symlinks. Soft links and hard links.

## Soft links

Soft links are pointers to the original file or directory. They are independent files that contain the path
of the target file or directory. If the original file is moved or deleted, the soft link
becomes a `dangling` link, pointing to a non-existent location.

Lets create a symlink.

First lets create the initial file `original.txt`.

```sh
touch original.txt
```

Now lets create the symlink:

```sh
ln -s original.txt softlink.txt
```

If you now have a look at the file softlink.txt, you will see that the file is empty.

```sh
> cat softlink.txt
```

Now lets add some data into the `original.txt`, for example "Hello, world!".
And now display the content of the `symlink.txt` file.

```sh
> echo "Hello, world!" > original.txt
> cat softlink.txt
Hello, world!
```

You will not see that the `softlink.txt` file also contains the content we have written
originally ino the `original.txt`.

You can also edit the symlink file and see that content inside the `original.txt` is also changed.

```sh
> echo "Hello, softlink file!" > softlink.txt
> cat original.txt
Hello, softlink file!
```

You can also see where the softlink is pointing to by using `readlink -f <filename>`.

```sh
> readlink -f softlink.txt
/home/user/original.txt
```

As mentioned previously, if you delete the original file, then the softlink will be invalid.

```sh
rm original.txt
ls -la softlink.txt # will still point to original.txt
cat softlink.txt # cat: softlink.txt: No such file or directory
```

## Hardlinks

Hard links are direct references to the physical data of the file of the disk. Unlike soft liks,
they do not contain the path to the original file but share the same `inode`.

Deleting or moving the original file does not affect the hard link as both the original file
and the hard link reference the same data on the disk. Hard links cannot be created for directories.

Now lets create a hardlink:

```sh
rm original.txt
touch original.txt
echo "Hello, world!" > original.txt
ln original.txt hardlink.txt
cat hardlink.txt # Hello, world!
```

As you can see, the same data, that we put into the original.txt, is in the `hardlink.txt` file.
But in contrary to softlinks, if we now delete the `original.txt`, the content will still be
in the `hardlink.txt`.

```sh
rm original.txt
cat hardlink.txt # Hello, world!
```
