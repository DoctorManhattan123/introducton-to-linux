# Processes

## What is a Process?

A **process** is simply an instance of a program that is currently being executed.
Basically, think of a process as a program in action.

Consider the following Python script `hello.py`:

```python
print("Hello, world!")
```

If you run `python hello.py`, you will start a process that will print `"Hello, world!"`.
Afterwards, the process will terminate.

You can list the processes on your system using the `ps` command.
For example, to list all processes you can run:

```sh
ps -ef
```

The `-e` flag selects all processes and the `-f` flag tells `ps` to do "full-format listing".

You can also show an interactive view of processes by executing the `top` command.

## Parents and Children

A process can launch another process.
We say that a **parent process** launches a **child process**.
This is in fact the main mechanism by which new processes are created.

The way this works is that after the kernel has booted, an initial process is created.
This process spawns new processes, which can in turn spawn new processes etc.
Basically the processes form a giant tree, with the initial process as the root of that tree.

Note that it is possible to have multiple initial processes, in which case you will have multiple process trees.

## PIDs

Each process has a **PID** (process ID) which is a number that uniquely identifies a process.
You can see the PIDs in the output of `ps -ef`.

Each process also has a **PPID** (parent process ID) which is simply the PID of its parent.

You can use the `-p` flag to get information about a process with a specific PID.
For example here is how you list information about the process with PID `1`:

```sh
ps -f -p 1
```

Let's consider an example.
Create the following file `parent.py`

```python
import os
import subprocess

print(f"Parent PID={os.getpid()}")
print(f"Parent PPID={os.getppid()}")

subprocess.Popen(["python", "child.py"])

input("Press Enter to exit.\n")
```

Also create the following file `child.py` in the same directory:

```python
import os
import time

print(f"Child PID={os.getpid()}")
print(f"Child PPID={os.getppid()}")

while True:
    print("Child process running")
    time.sleep(3)
```

Run `python parent.py` and observe the following terminal output:

```
Parent PID=148411
Parent PPID=144069
Press Enter to exit.
Child PID=148469
Child PPID=148411
Child process running
Child process running
Child process running
Child process running
...
```

> Of course your PIDs and PPIDs will be different.

The parent process (`python parent.py`) with PID `148411` has created a child process (`python child.py`).
That child process has a completely new PID `148469`, however its PPID is the ID of the parent process, i.e. `148411`.

You can observe this with the `ps` command:

```console
$ ps -f 148411
UID          PID    PPID  C STIME TTY      STAT   TIME CMD
example   148411  144069  0 19:06 pts/2    S+     0:00 python parent.py

$ ps -f 148469
UID          PID    PPID  C STIME TTY      STAT   TIME CMD
example   148469  148411  0 19:06 pts/2    S+     0:00 python child.py
```

## Process States

A process has a state.
The three most important states are:

- running (`R`)
- sleeping (`S`)
- stopped (`T`)

Here is a Python script that cycles through the states:

```python
import time
import os
import signal

print(f"Process ID: {os.getpid()}")

# Running state
print("State: Running")
k = 0
for i in range(10000):
    for j in range(10000):
        k += i * j

# Sleeping state
print("State: Sleeping")
time.sleep(10)

# Stopped state
print("State: Stopped")
os.kill(os.getpid(), signal.SIGTSTP)
```

## Foreground and Background Processes

Let's take the following Python script:

```python
import time
import os

print(f"Process ID: {os.getpid()}")

for i in range(30):
    print(f"Running iteration {i}")
    time.sleep(1)
```

If you simply run the normal command `python example.py`, this will create a **foreground process**.
This is a process that is connected to a terminal.

This menas that the user can interact with the process (e.g. input some data).
Foreground processes also take control of the terminal, i.e. you can't use the terminal for anything else while the process is running.
You can also stop the process with `^C`.

You can create a **background process** by adding a `&` to the end of the command.
For example:

```sh
python example.py &
```

Notice that you can now interact with the terminal normally, however you will still see the output.
You can execute the `jobs` command to see the status of background processes that are currently running.

If you do so, you will (among other things) see the following entry:

```
[3]   Running                 python example.py &
```

Notice that you can't stop a background process with `^C`.

To bring a background process to the foreground, simply run `fg %NUM`, where `NUM` is the number of the job.
In the above example the job number was `3`, so we would run:

```sh
fg %3
```

You can use foreground processes if you need to interact with the process or your process produces a lot of output and you need to heavily monitor it.

You can use background processes if you need to run tasks that take a long time, don't need to be monitored by you or you want to run multiple tasks at the same time.

## Signals

**Signals** are basically notifications that can be used to tell a process to do something special.

The **SIGINT** (signal interrupt) can the controlling terminal to indicate that a user wants to interrupt the process.
If you start a process and you press `^C` in the terminal that sends a SIGINT.

The **SIGTERM** (signal terminate) "politely" tells a process to terminate.
This signal can be caught and ignored by the process (hence the "polite" termination).
Usually you should terminate a process using the SIGTERM signal, because that allows the process to potentially perform cleanup.
You can send a SIGTERM using e.g. `kill -TERM $PID`.

The **SIGKILL** (signal kill) forces a process to terminate.
This signal can't be caught by the process.
You should send a SIGKILL as a last resort if the process ignores your polite request to die after a SIGTERM.
You can send a SIGKILL using e.g. `kill -KILL $PID`.

The **SIGHUP** (signal hangup) is sent to a process when the controlling terminal is closed.
If you start a process and close the terminal that sends a SIGHUP.

You can "detach" a process from its terminal by telling it to ignore SIGHUP signals using the `nohup` command:

```
nohup python3 example.py &
```

The **SIGTSTP** (signal terminal stop) "politely" stops a process.
This signal can be caught and ignored by the process (just like SIGTERM).
If you start a process and you press `^Z` in the terminal that sends a SIGTSTP.

The **SIGSTOP** (signal stop) stops a process.
This signal can't be caught by the process (just like SIGKILL).
You can send a SIGSTOP using e.g. `kill -STOP $PID`.

You can resume a stopped process using **SIGCONT**.
You can send a SIGCONT using `kill -CONT $PID`.

> Yes, the `kill` command has a _very unfortunate and confusing_ name.
> It should have been named `sendsignal` or something similar.
