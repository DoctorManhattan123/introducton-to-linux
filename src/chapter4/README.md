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

A process can be in a **state**.

The three most important state are **running**, **sleeping** and **stopped**.

A process is running if it's either currently being executed by the CPU or is on a "run queue" (i.e. will be executed as soon as the CPU is available).
Basically, processes that are in the running state are actively doing some work or are about to do some work.

A process is sleeping if it's waiting for something to happen (like an event or it's waiting for some time to pass).
During this time, a process is not executing any code.

The most common type of sleep is the **interruptible sleep** can be interrupted by signals (we will talk about them below).
However, there is also the **uninterruptible sleep**, which is a special sleep mode in which a process can't be interrupted by a signal.

A process can also be in the stopped state, which is rather self explanatory.
Usually, a process is stopped because it has received a signal.
Note that a process tha was stopped can be restarted where it left off.

Here is a Python script `proc.py` that cycles through the states:

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

Execute the script by running `python proc.py`.
The script will print a PID - now run `watch proc -f $PID`.

While the process is calculating the number `k`, you will see an `R+` in the status column - `R` is short for "running".
This makes sense, because the process is actively using the CPU for number crunching.

When the `time.sleep` is called, the status column will show an `S+` in the status column - `S` is short for "sleeping".
This also makes sense, because the process is waiting for the timer to complete and is not actively executing any code.

Finally, once the process receives the SIGTSTP signal, the status column will show a `T` - `T` is short for "stopped".

> There are more states a process can be in (like "zombie"), but these are out of scope for this book.

## Signals

**Signals** are basically notifications that can be used to tell a process to do something special.

You can send a signal to a process either by invoking the `kill` command or by using certain keyboard shortcuts on the controlling terminal.

To showcase the effect of different signals, we will use the following `proc.py` example:

```python
import os
import time

print(os.getpid())

while True:
    print("Running...")
    time.sleep(1)
```

The **SIGINT** (signal interrupt) can be sent by the controlling terminal to indicate that a user wants to interrupt the process.

Start the process by running `python proc.py` and press `^C` in the terminal.
This will send a SIGINT to the process.
In the case of Python, a `KeyboardInterrupt` will be raised and the process will be killed.

However, a SIGINT can also be explicitly handled by the process, resulting in different behaviour.
Consider this example:

```python
import os
import signal
import time

print(os.getpid())

def handler(signum, frame):
    print("I received the SIGINT, but I will just keep running...")

signal.signal(signal.SIGINT, handler)

while True:
    print("Running...")
    time.sleep(1)
```

If you try to press `^C` now, the process will just print the message and keep running.

> If you press `^C` and a process doesn't die, that means that it has registered a signal handler.
> So keep in mind that `^C` doesn't actually necessarily kill a process in all cases.

You can also sending a SIGINT by executing `kill -SIGINT $PID`.

The **SIGTERM** (signal terminate) "politely" tells a process to die for good.
This signal can be caught and ignored by the process (hence the "polite").

You can send a SIGTERM by executing `kill -SIGTERM $PID`.

Just like the SIGINT, a SIGTERM can be caught:

```python
import os
import signal
import time

print(os.getpid())

def handler(signum, frame):
    print("I received the SIGTERM, but I will just keep running...")

signal.signal(signal.SIGTERM, handler)

while True:
    print("Running...")
    time.sleep(1)
```

The difference between SIGINT and SIGTERM is a bit philosophical.
A SIGINT is usually an signal from a user requesting immediate interruption, while SIGTERM is usually a termination request by a system service (or script).

If you are a regular user and you need to interrupt a process, you should send a SIGINT by pressing `^C`.
If you write a system script that needs to terminate certain processes, you should send a SIGTERM.

Note that the fact that a process can catch a SIGINT or SIGTERM is not a bug, but a feature.
For example a process might choose to perform some cleanup upon receiving a SIGINT (or SIGTERM), which is often a good idea.

The **SIGKILL** (signal kill) is the "impolite" version of a SIGTERM.
This signal can't be caught by the process.
This means that process will be terminated immediately (by the operating system).

You should send a SIGKILL as a last resort if the process ignores your more polite requests to die.
You can send a SIGKILL using e.g. `kill -SIGKILL $PID`.

The **SIGHUP** (signal hangup) is sent to a process when the controlling terminal is closed.
If you start a process and close the terminal that will send a SIGHUP.
You can also send a SIGHUP using e.g. `kill -SIGHUP $PID`.

Note that a SIGHUP can be caught:

```python
import os
import signal
import time

print(os.getpid())

def handler(sig, frame):
    with open("hup", "w") as f:
        f.write("I caught the SIGHUP")

signal.signal(signal.SIGHUP, handler)

while True:
    print("Running...")
    time.sleep(1)
```

If you run `python proc.py` and then close the controlling terminal, you will see that a new file `hup` has been created in the same directory as `proc.py`.

You can "detach" a process from its terminal by telling it to ignore SIGHUP signals using the `nohup` command.
Consider the following example:

```python[1]+ Stopped                 python easy.py
import time
import os

print(os.getpid(), flush=True)

while True:
    print("Running...", flush=True)
    time.sleep(1)
```

Now run:

```
nohup python example.py
```

If you close the controlling terminal, you will see that the process will still be running.
You can confirm that by inspecting the `nohup.out` file, getting the process ID and executing `ps -f -p $PID`.

The `nohup` command is useful for scenarios where you need to start a process in a terminal, but keep it running even if the terminal is closed.

The **SIGTSTP** (signal terminal stop) "politely" stops a process.
This signal can be caught and ignored by the process (just like SIGTERM).
If you start a process and you press `^Z` in the terminal that sends a SIGTSTP.
You can also use e.g. `kill -SIGTSTP $PID`.

The **SIGSTOP** (signal stop) is again the "impolite" version of SIGTSTP.
This signal can't be caught by the process (just like SIGKILL).
You can send a SIGSTOP using e.g. `kill -SIGSTOP $PID`.

You can resume a stopped process using **SIGCONT**.
You can send a SIGCONT using `kill -CONT $PID`.

Consider the `proc.py` example script.
If you run `python proc.py` and then press `^Z`, you will see the following output in the terminal:

```
[1]+  Stopped                 python easy.py
```

Executing `ps -f $PID` will reveal that the process is still very much alive and kicking, but it is in the stopped (`T`) state.

You can resume the process by executing `kill -SIGCONT $PID`.
It will then continue its executing and keep printing "Running...".

> Yes, the `kill` command has a _very unfortunate and confusing_ name.
> It should have been named `sendsignal` or something similar.

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

This means that the user can interact with the process (e.g. input some data or send a SIGINT).
Foreground processes also take control of the terminal, i.e. you can't use the terminal for anything else while the process is running.

You can create a **background process** by adding a `&` to the end of the command.
For example:

```sh
python example.py &
```

Notice that you can now interact with the terminal normally, but you will still see the output.
You can no longer interact with the process however, for example pressing `^C` will no longer send a SIGINT.

You can execute the `jobs` command to see the status of background processes that are currently running.

If you do so, you will (among other things) see the following entry:

```
[1]   Running                 python example.py &
```

To bring a background process to the foreground, simply run `fg %NUM`, where `NUM` is the number of the job.
In the above example the job number was `1`, so we would run:

```sh
fg %1
```

You can use foreground processes if you need to interact with the process or your process produces a lot of output and you need to heavily monitor it.

You can use background processes if you need to run tasks that take a long time, don't need to be monitored by you or you want to run multiple tasks at the same time.

If you want to make sure that the controlling terminal no longer has any impact on the process, you can combine the `nohup` command with background processes:

```sh
nohup python proc.py &
```

You can now do anything you want in your terminal and even close it - the process will just keep running along.

Doing this is the simplest way to start e.g. a long running web server - just `nohup` it and sends the process to the background.
You can then safely close the terminal - the web server will keep running.
However, for production setups web server usually utilize proper process supervisors like `systemd`.

## The `systemd` Tool

The `systemd` tool allows you to manage services.

Consider the following example:

```python
import time

with open("/tmp/example.txt", "w") as f:
    while True:
        f.write("Running...")
        f.flush()
        time.sleep(3)
```

To manage this script using `systemd` we need to create a service unit file that will define how to start, stop and manage this script.
Create the following file at `/etc/systemd/system/procpy.service`:

```ini
[Unit]
Description=procpy
After=network.target

[Service]
ExecStart=python /path/to/proc.py
Restart=always
User=exampleuser
Group=examplegroup

[Install]
WantedBy=multi-user.target
```

Start the service:

```sh
sudo systemctl start procpy.service
```

You can check the status of the service by running:

```sh
systemctl status procpy.service
```

Finally, you can stop the service:

```sh
sudo systemctl stop procpy.service
```

To automatically start a service at boot, you can use the `enable` command:

```sh
sudo systemctl enable procpy.service
```

To disable the service from starting automatically, you can use the `disable` command:

```sh
sudo systemctl disable procpy.service
```

If your system is failing to start, you can look at the journalctl logs:

```sh
journalctl -u procpy.service
```

Finally, if you need to change the service file, you will have to run this command before restarting the service:

```sh
sudo systemctl daemon-reload
```
