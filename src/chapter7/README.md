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
