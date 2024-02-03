# Basic Shell Scripting

## Hello, World

Create a script `test.sh`:

```sh
echo "Hello, world!"
```

Execute it:

```sh
sh test.sh
```

## Variables

You declare a variable using `variable=value` and use the variable by prefixing it with `$`.
For example:

```sh
username="John"
echo $username
```

Environment variables are special variables that are defined outside your script.
They are part of the environment in which the script runs.
These variables contain information related to the system or user environment.

For example `USER` contains the current user and `HOME` the home directory of the current user:

```sh
echo $USER
echo $HOME
```

## Conditionals

You can use `if`, `elif` and `else`:

```sh
if [ condition1 ]; then
    # commands for condition1
elif [ condition2 ]; then
    # commands for condition2
else
    # commands if neither condition1 nor condition2 is true
fi
```

## Loops

You can use a `for` loop:

```sh
for item in list; do
    # commands to execute
done
```

Example:

```sh
for i in 1 2 3; do
    echo $i
done
```

You can use a while loop:

```sh
while [ condition ]; do
    # commands to execute
done
```

Example:

```sh
count=1
while [ $count -le 5 ]; do
    echo $count
    count=$((count + 1))
done
```

## Functions

Define a function:

```sh
function_name() {
    # commands
}
```

Example:

```sh
greet() {
    echo "Hello, $1!"
}

greet "World"  # Outputs: Hello, World!
```

Functions can take parameters:

```sh
add() {
    sum=$(( $1 + $2 ))
    echo "Sum is: $sum"
}

add 5 10  # Calls the function with 5 and 10 as arguments
```

## Redirection

Programs usually send results to **stdout** and errors to **stderr** and take input from **stdin**.
They are internaly referenced as 0 (stdin), 1 (stdout), 2 (stderr).

You can use I/O redirection to redirect where stdout goes with `>` or `>>`.
To redirect stderr we use `2>`.

We can redirect stdout and stderr to a file using `> filename 2>&1` (i.e. redirect stdout to file and stderr to stdout).
Alternatively you can use `&>` for combined redirection.

Use `<` to redirect stdin.

We can create the worlds simplest text editor with `cat > output`.

## Pipe Operator

The pipe operator `|` can be used to pipe stdout of one command to stdin of another command.

## Interesting Commands

There are basically four types of commands:

- executable programs (compiled binaries or scripts)
- shell builtins
- shell functions
- aliases

Use `type` to display a command type:

```console
$ type cd
cd is a shell builtin
$ type ls
ls is aliased to `ls --color=auto'
$ type mv
mv is /usr/bin/mv
```

Use `which` to get the locatino of an executable:

```console
$ which mv
/usr/bin/mv
```

Use `help` to get help for shell builtins:

```sh
help cd
```

For most commands you can try `--help`:

```sh
mv --help
```

Use `whatis` for a one-line description:

```sh
whatis mv
```

You can also try to view the **man page** (manual page):

```sh
man mv
```

You can search the man pages with `apropos`:

```sh
apropos "remove file"
```

## Aliases

You can create aliases with the `alias` command and `unalias` to remove an alias.
