# Basic Shell Scripting

## Hello, World

So far we only executed simple commands in the terminal.
For more complex scenarios, we might execute a lot of commands after each other, chain commands together or maybe execute commands conditionally.
To achieve this, we could write a **shell script**

Let's create a very simple test script and call it `test`:

```sh
echo "Hello, world!"
```

Execute it:

```sh
bash test
```

This will print `Hello, world!`.

Bash is the name for one of the most common Linux shells.
However, it is not the only shell - there are many others like `ksh`, `csh` and `fish`.
One particularly interesting shell is `sh`, which is an older shell whose features later became standardized in the POSIX standard.
Bash implements all `sh` features and has some additional functionality - however, we will only talk about the `sh` compliant parts of bash.

Because there are many other shells, it is often good practice to add a "shebang" on top of your shell to specify how your script should be executed:

```sh
#!/bin/bash

echo "Hello, world!"
```

Instead of writing `bash test`, you can also mark your script as executable and then run it directly:

```sh
chmod u+x test
./test
```

This will again print `Hello, world!`.

## Variables

You declare a variable using `variable=value` and use the variable by prefixing it with `$`.
For example:

```sh
# You can assign a number to a variable
number=1
echo $number

# You can assign a string to a variable.
# If the string contains no whitespaces, you don't need
# to put it in quotes, although it's usually recommended to do so.
short_username=John
echo $short_username

short_username_2="John"
echo $short_username_2

long_username="John Doe"
echo $long_username

# You can use a variable in a string if you prefix it with $
echo "Hello, $short_username"

# You can also surround the variable with curly braces
# in ambiguous expressions
echo "Hello, ${short_username}1"
```

This will print the following:

```
1
John
John
John Doe
Hello, John
Hello, John1
```

An extremely common error that beginners make is adding spaces in assignments:

```sh
username = "John"
```

This doesn't work and will result in the following error:

```sh
./test: line 1: username: command not found
```

## Command Substitution

TODO

## Arithmetic

The shell supports the usual operators `+`, `-`, `*`, `/`, `%` (modulo) and `**` (exponentiation).

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

This will result in a syntax error:

```sh
echo $((5.2))
```

## Functions

Just like most other programming languages, sh supports functions.
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
    sum=$(( $1 + $2 + $3 ))
    echo "Sum is: $sum"
}

# Call the function with 5, 10 and 15 as arguments
add 5 10 15
```

If you want return a value from a function, you typically echo it and then capture the output via command substitution:

```sh
greet() {
    echo "Hello, world!"
}

greeting=$(greet)
echo $greeting
```

Generally, don't think of shell functions the same way you would think about e.g. Python or TypeScript functions.
Instead, you should think of shell functions as kind of "mini-scripts".

An important point with `sh` is that all variables are globaly by default, even if they're created in a function body:

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

If you want to create a local variable, you need to explicitly use the `local` keyword:

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

You can also use the operators `=`, `!=` for string comparisons.

You can also use `-a` (and), `-o` (or) and the `!` (not) operators.

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

## Stdin, Stdout and Stderr

Programs usually send results to **stdout** and errors to **stderr** and take input from **stdin**.
They are internaly referenced as 0 (stdin), 1 (stdout), 2 (stderr).

## Redirection

You can use I/O redirection to redirect where stdout goes with `>` or `>>`.
To redirect stderr we use `2>`.

We can redirect stdout and stderr to a file using `> filename 2>&1` (i.e. redirect stdout to file and stderr to stdout).
Alternatively you can use `&>` for combined redirection.

Use `<` to redirect stdin.

We can create the worlds simplest text editor with `cat > output`.

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
