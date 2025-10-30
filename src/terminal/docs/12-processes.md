# Processes

Processes are programs in action. Programs in binary/executable form
reside on your disk; when they are executed (run), they are moved into
memory and become a process. Each and every program we run is a
process.

## Interrupting (CTRL-C)

The kernel delivers signals to processes for many reasons. The SIGINT
signal is raised when the user presses the <ctrl><C> key
combination. It is delivered to the process that is in the foreground in
the currently active terminal. If that process has not set up a response
to SIGINT (we say it catches SIGINT), then it will be terminated
immediately with possible loss of data (but open files do get closed).
Some programs catch SIGINT and use it for a specific purpose. For
example, the nano editor responds to it by displaying the current
position in the file being edited.

## ps and kill

We can use the `ps` and `top` commands to view processes running on our
machine.

The `ps` command, when you run it without any arguments, displays
processes run by the current user.

    $ ps
     PID TTY          TIME CMD
    3922 tty2     00:00:00 su
    3923 tty2     00:00:00 sh
    3941 pts/0    00:00:00 cat
    3942 pts/0    00:00:00 ps

Here we find there are 4 processes that we are running from our
terminal. The 4 columns have the following interpretation:

    Process ID   Terminal   CPU Time   Program/Command

Each process has an identifier by which the operating system tracks it.
This is an integer number that is given to each new process, and is
called the PID (for "process ID"). The gap between the PID 3923 for sh
and the PID 3941 for cat merely shows that somebody started processes on
the machine in between the times these two processes started.

The second column in the output of `ps` specifies the terminal to which
the process is attached or the terminal that controls the process. You
can use the `tty` command to find out which terminal you are presently
in.

    $ tty
    /dev/tty2

Now, you may well expect that your machine has a lot more processes than
the ones you see by running a simple `ps` without arguments. In fact, it
shows only the processes you started from the terminal in which you
issue the command. On a graphical desktop, that command doesn't show
the programs you start from menus or by clicking on icons. The system
also runs a lot of its own processes in the background. To see
everything, add the `-e` option:

    $ ps -e
     PID TTY          TIME CMD
       1 ?        00:00:01 init
       2 ?        00:00:00 kthreadd
       3 ?        00:00:00 migration/0
       4 ?        00:00:00 ksoftirqd/0
       5 ?        00:00:00 watchdog/0
       6 ?        00:00:00 migration/1
       7 ?        00:00:00 ksoftirqd/1
       8 ?        00:00:00 watchdog/1
       9 ?        00:00:00 events/0
      10 ?        00:00:00 events/1
      11 ?        00:00:00 khelper
      44 ?        00:00:00 kblockd/0
      45 ?        00:00:00 kblockd/1
    .........................................
    .........................................
    3534 tty1     00:00:00 getty
    3535 tty2     00:00:00 login
    3536 tty3     00:00:00 getty
    3537 tty4     00:00:00 getty
    3538 tty5     00:00:00 getty
    3539 tty6     00:00:00 getty

In case you want to terminate a process that you started, you can do so
from a terminal using the `kill` command.

    $ kill 3941

Here we provide the PID as the argument. Remember that the kill argument
is non-interactive (it doesn't ask for confirmation before starting)
and non-verbose (it doesn't tell you what it is doing) by default and
hence must be used carefully. You can kill only your own processes. Also
if the program has truly crashed it may not respond to the instruction
so use the `-9` option in that case. That is the signal number for
SIGKILL, and if you are really careful you will type "kill -s SIGKILL
(pid)" since it is conceivable that your system has the numbers
assigned differently.

## Processes and jobs (background)

If you want to run something in the background and return control to
your terminal, just put an ampersand ("&") after the command name.

    $ firefox &
    [1] 3694
    $

The shell prints a brief message and gives you another dollar sign
prompt. Firefox is now running (and should pop up a window of its own,
because it's a graphical program). You can continue to execute other
commands in your terminal.

What are the two numbers printed after you put the program in the
background? The number in square brackets is a special number assigned
to each program you run in the background; it's called a *job number*.
In this case, the job number is 1 because we don't have any other
programs currently running in the background in this shell.

The second number, which is 3694 in this case, is the process number we
saw in an earlier section.

To bring your job back in the foreground just type `fg`. The job takes
over your terminal as commands usually do, until it finishes.

If you have multiple jobs in the background, you can pass the job to the
`fg` command either as a process number:

    $ fg 3694

or as a job number:

    $ fg %1

To distinguish the job number from a process number, you must put a
percent sign ("%") before the job number.

If you want to run a process in the background that's now running in
the foreground, type **Ctrl + Z**, which suspends the job, then issue
the command `bg`.

To find out what jobs you've put in the background (and their status),
enter `jobs`:

    $ jobs
    [1]-  Running                 firefox &
    [2]+  Exit 2                  sort > big_file_sorted 2> big_file_err

Firefox is still running, but the second job exited with an error status
of 2.
