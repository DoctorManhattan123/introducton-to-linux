# Basic Shell Scripting

## Hello, World

So far we only executed simple commands in the terminal.
For more complex scenarios, we might want execute a sequence of commands after each other, chain commands together, repeat commands or maybe even execute commands conditionally.

We can achieve this by writing **shell scripts**.

Let's create a very simple example script `example.sh` with the following content:

```sh
echo "Hello, world!"
```

We can execute that example script by running:

```sh
sh test.sh
```

This will output `Hello, world!` to the terminal.

## Choosing Your Flavour

The are many different shells available for Linux - `sh`, `bash`, `ksh`, `csh` and `fish` are just a few examples.

We will primarily focus on `sh` and `bash`.
The `sh` shell is an older shell whose features later became standardized in the so called **POSIX standard**.
The `bash` shell is a newer shell that implements all `sh` features and has some additional functionality.
We will primarily talk about the `sh` compliant parts of bash.
If some `bash` feature is not compatible with `sh` we will explicitly mention it.

Because there are many different shells, it is often good practice to add a **shebang** on top of your shell to specify how your script should be executed.
For example if we want to execute our script with `sh` we would specify the location of `bash` as the shebang:

```sh
#!/bin/sh

echo "Hello, world!"
```

Alternatively, we could specify that our script should be executed with `bash` like this:

```sh
#!/bin/bash

echo "Hello, world!"
```

Shebangs are a good idea, because often scripts are executed not via `sh` or `bash` but by marking them as executable and then simply specifying the path:

```sh
chmod u+x example.sh
./example.sh
```

The shebang will make sure that the script will be executed by the correct shell.

## Variables

You declare a variable using the `variable=value` notation.
You can then use the variable by prefixing it with the `$` character.

Unlike most other programming languages, `sh` doesn't really have the concept of a data type.
All variables are strings, but, depending on the context, you can do arithmetic operations and comparisons with variables.
Basically, if a variable contains only digits, it can be treated as a number.

Consider this example:

```sh
# You can assign a string to a variable.
# If the string contains no whitespaces, you don't need
# to put it in quotes, although it's usually recommended to do so.
short_username=John
echo $short_username

short_username_2="John"
echo $short_username_2

long_username="John Doe"
echo $long_username

# The number variable is still a string, but you can do numeric operations on it,
# as we will see later
number=1
echo $number
```

This will output the following:

```
John
John
John Doe
1
```

You can use variables inside strings like this:

```sh
# You can use a variable in a string if you prefix it with $
echo "Hello, $short_username"

# You can also surround the variable with curly braces
# in ambiguous expressions
echo "Hello, ${short_username}1"
```

This will output the following:

```
Hello, John
Hello, John1
```

An extremely common error that beginners make is adding spaces in assignments:

```sh
username = "John"
```

This doesn't work and will result in an error:

```sh
./example.sh: line 1: username: command not found
```

## Command Substitution

You can use the output of a command in your script via the **command substitution** mechanism.
The syntax for this is `$(command)`.

For example, here is how you can store the output of `ls` in the variable `ls_output`:

```sh
ls_output=$(ls)
echo $ls_output
```

## Arithmetic

The shell supports the usual arithmetic operators `+`, `-`, `*`, `/`, `%` (modulo) and `**` (exponentiation).

You can perform arithmetic expansion using the `$((expression))` notation:

```sh
echo $((5 + 2)) # 7
echo $((5 - 2)) # 3
echo $((5 * 2)) # 10
echo $((5 / 2)) # 2
echo $((5 % 2)) # 1
echo $((5 ** 2)) # 25
```

Note that division is truncated to integers and arithmetic expansion only supports integers.

For example, trying to do this will result in a syntax error:

```sh
echo $((5.2))
```

## Functions

Just like most other programming languages, `sh` supports functions.

However, you should not think about shell functions the same way you would think about e.g. Python or TypeScript functions.
Instead, you should think of shell functions as kind of "mini-scripts" (or "mini-commands").
This is because `sh` functions have peculiar syntax when you want to pass arguments or return a value.

You can define a function like this:

```sh
function_name() {
    # commands
}
```

You can the call the function by simply writing `function_name` (just like you would call a command).

For example:

```sh
greet() {
    echo "Hello, world!"
}

greet
```

This will output `Hello, world!`.

Note that you can also define a function like this:

```sh
function greet {
    echo "Hello, world!"

greet
}
```

However, this is a `bash` extension and not POSIX-compliant.

If you need to pass parameters to a function, you don't write `greet(arg1, arg2)`.
Instead, arguments passed to a function can be accessed using `$1`, `$2` etc:

```sh
greet() {
    echo "Hello, $1!"
}

greet "World"
```

This will output `Hello, world!`.

Here is an example with multiple parameters:

```sh
add() {
    sum=$(($1 + $2 + $3))
    echo "Sum is: $sum"
}

# Call the function with 5, 10 and 15 as arguments
add 5 10 15
```

If you want return a value from a function, you can `echo` it and then capture the output via command substitution:

```sh
greet() {
    echo "Hello, world!"
}

greeting=$(greet)
echo $greeting
```

> Note that there is also the `return` keyword which allows you to exit a function with a status code.

An important point with `bash` specifically is that variables are global by default, even if they're created in a function body:

```sh
fun() {
    # Create a variable in function body
	x=42
    # This will output 42
    echo "In body: $x"
}

# Call the function
fun

# This will also output 42
echo "Outside body: $x"
```

If you want to create a local variable in `bash`, you need to explicitly use the `local` keyword:

```sh
fun() {
    # Create a variable in function body
	local x=42
    echo "In body: $x"
}

# Call the function
fun

# This will output an empty string
echo "Outside body: $x"
```

## Conditionals

You can use the `if`, `elif` and `else` keywords for conditional behaviour:

```sh
if [ condition1 ]; then
    # commands for condition1
elif [ condition2 ]; then
    # commands for condition2
else
    # commands if neither condition1 nor condition2 is true
fi
```

Make sure that there is a space after `[` and a space before `]`.

The conditions must be expressions that evaluate to true or false.
Here you can use comparison operators like `-eq`, `-ne`, `-lt`, `-le`, `-gt` and `-ge` for numeric comparisons.
For example:

```sh
age=14

if [ $age -lt 17 ]; then
	echo "No drinks for you!"
elif [ $age -eq 17 ]; then
	echo "So close!"
else
	echo "Have fun!"
fi
```

You can also use the operators `=`, `!=` for equality comparisons.
For example:

```sh
number="one"

if [ $number = "one" ]; then
    echo "We are number one!"
elif [ $number = "two" ]; then
    echo "We are number two!"
else
    echo ":("
fi
```

You can also use `-a` (and), `-o` (or) and the `!` (not) operators to combine conditions.
For example:

```sh
user="admin"
logged_in="true"

if [ $user = "admin" -a $logged_in = "true" ]; then
    echo "Welcome, admin!"
fi
```

Bash introduces additional syntax that looks more like conditionals in other programming languages.
You can write `[[ ]]` instead of `[ ]` to allow for operators like `&&` (and), `||` (or) and `==` (same as `=`).
For example, the previous example could be writen like this in Bash:

```sh
user="admin"
logged_in="true"

if [[ $user = "admin" && $logged_in = "true" ]]; then
    echo "Welcome, admin!"
fi
```

## Loops

The shell supports most of the familiar loops.

The general syntax for a `for` loop looks like this:

```sh
for item in list; do
    # commands to execute
done
```

For example:

```sh
for i in 1 2 3; do
    echo $i
done
```

The general syntax for a `while` loop looks like this:

```sh
while [ condition ]; do
    # commands to execute
done
```

For example:

```sh
count=1
while [ $count -le 5 ]; do
    echo $count
    count=$((count + 1))
done
```

## Stdin, Stdout and Stderr

Programs usually send results to **stdout** and errors to **stderr** and take input from **stdin**.
They are internaly referenced as 0 (stdin), 1 (stdout), 2 (stderr).

For example, the output of `echo` is sent to `stdout` by default:

```sh
echo "This is sent to stdout"
```

You can read user input from `stdin` by using the `read` command:

```sh
read -p "Enter a value: " input
echo $input
```

You can use I/O redirection to redirect where stdout goes with `>` or `>>`.
For example, you can redirect stdout to a file like this:

```sh
echo "This will be written to example.txt" > example.txt
```

The difference between `>` and `>>` is that `>` will overwrite the file, while `>>` will append to the file.
Consider this example:

```sh
echo -n "This first line will be overwritten" > example.txt
echo "Second line" > example.txt
```

The resulting file will look like this:

```
Second line
```

Now consider this example:

```sh
echo "This first line will be overwritten" >> example.txt
echo "Second line" >> example.txt
```

The resulting file will look like this:

```
This first line will be overwritten
Second line
```

We can also use `>` to redirect `stdout` to `stderr` via the `1>&2` notation.
For example, here is how we can write something to `stderr`:

```
echo "This will be written to stderr" 1>&2
```

We can also use `>` to redirect stderr to stdout via the `2>&1` notation.

This is usually interesting for the cases where want to redirect both stdout and stderr to a file using `> filename 2>&1`.
Note that the order of the redirects is important here:

First, we redirect `stdout` to `filename`.
Second, we redirect `stderr` to `stdout`.
Since `stdout` is redirected to `filename`, `stderr` will also be redirected to `filename`.

> We can also use the `>` operator to create the worlds simplest text editor by running `cat > output`.

We can use the `<` operator to redirect stdin.
Consider this script that read a line from a user and outputs it back:

```sh
read -p "Enter a line:" line
echo $line
```

If you want to read that line from a file instead, you can create a new file containing the line and then use `<` to redirect `stdin` from the file:

```
sh example.sh < example.txt
```

## Pipe Operator

The pipe operator `|` can be used to pipe stdout of one command to stdin of another command.

## Environment Variables

Environment variables are special variables that are defined outside your script.
They are part of the environment in which the script runs.
These variables contain information related to the system or user environment.

For example `USER` contains the current user and `HOME` the home directory of the current user:

```sh
echo $USER
echo $HOME
```

Use `printenv` to print environment variables.
