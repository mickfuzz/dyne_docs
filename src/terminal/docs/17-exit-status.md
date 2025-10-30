# Exit status

When you type commands, you can usually tell whether they worked or not.
Commands that are unable to do what you asked usually print an error
message. This is sufficient if you are typing in each command by hand
and looking at the output, but sometimes (for example, if you are
writing a script) you want to have your commands react differently when
a command fails.

To facilitate this, when a command finishes it returns an *exit status*.
The exit status is not normally displayed; instead it is placed in a
variable (a named memory slot) named "**$?**". The exit status is a
number between 0 and 255 (inclusive); zero means success, and any other
value means a failure.

One way to see the exit status of a command is to use the `echo` command
to display it:

    $ echo "this works fine"
    this works fine
    $ echo $?
    0
    $ hhhhhh
    bash: hhhhhh: command not found
    $ echo $?
    127

Now we'll look at various ways to handle errors.

## if/then

Handling an error is an example of something you do conditionally: *if*
something happens, *then* you want to take action.   The shell provides
a compound command--a command that runs other commands--called `if`.
The most basic form is:

    if
      <command>
    then
      <commands-if-successful>
    fi

We will start with a basic example, then improve it to make it more
useful.  After we type `if` and press the **Enter** key, the shell knows
we're in the middle of a compound command, so it displays a different
prompt (**>**) to remind us of that.

``` SCREEN
$ if
> man ls
> then
> echo "You now know more about ls"
> fi
The manual page for ls scrolls by
You now know more about ls
```

Running this command brings up the manual page for `ls`.  Upon quitting
with the **q** key, the `man` command exits successfully and the `echo`
command runs.

### Handling command failure

Adding an `else` clause allows us to specify what to run on failure:

    if
      <command>
    then
      <commands-if-successful>
    else
      <commands-if-failed>
    fi

Let's run `apropos` if the `man` command fails.

``` SCREEN
$ if
> man draw
> then
> echo "You now know more about draw"
> else
> apropos draw
> fi
...
list of results for apropos draw
...
```

This time the `man` command failed because there is no `draw` command,
activating the else clause.

## && and ||

The if-then construct is very useful, but rather verbose for chaining
together dependent commands.  The "*&&*" *(and)* and "*||*" *(or)*
operators provide a more compact format.

    command1 && command2 [&& command3]...

The && operator links two commands together.  The second command will
run only if the first has an exit status of zero, that is, if the first
command was successful.  Multiple instances of the && operator can be
used on the same line.

    $ mkdir mylogs && cd mylogs && touch mail.log && chmod 0660 mail.log

Here is an example of multiple commands, each of which assume the prior
one has run successfully.  If we were to use the if-then construct to do
this, we would have ended up with an unwieldy mass of ifs and thens.

Note that the && operator *short circuits*, that is, if one command
fails, no subsequent command is run.  We take advantage of this property
to prevent unwanted effects (like creating *mail.log* in the wrong
directory in the above example).

If *&&* is the equivalent of `then`, the *|| operator* is the
equivalent of `else`.  It provides us a mechanism to specify what
command to run if the first fails.

    command1 || command2 || command3 || ...

Each command in the chain will be run only if the previous command did
not succeed (that is, had a nonzero exit status).

    $ cd Desktop || mkdir Desktop || echo "Desktop directory not found and could not be created"

In this example we try to enter the *Desktop* directory, failing which
we create it, failing which we inform the user with an error message.

With this knowledge we can write an efficient and compact `helpme`
function.  Our previous examples have shown the two operators used in
isolation, but they can be mixed as well.

    $ function helpme() {
      man $1 && echo "you now know more about $1" || apropos $1
    }

As you probably suspect, the "you now know..." echo is not exactly
the most useful command.  (It might not even be accurate, perhaps the
`man` page introduced so many options and confused the poor user).  We
heartily confess we threw it in just to match the original if-then
syntax.  Now that we know about the *|| operator*, we can simplify the
function to:

    $ function helpme() {
      man $1 || apropos $1
    }

## What does an exit status mean?

Up to now, we considered only the difference between zero and non-zero
exit statuses.  We know that zero means success, and non-zero means a
failure.  But what kind of failure?  Some exit values may be used for
user-specified exit parameters, so that their meaning may vary for one
command to another.  However, some widely accepted meanings for
particular values do exist.

For example, if you invoke an inexistent command (e.g. by wrongly typing
an existing one or by omitting its correct path), you should expect to
receive the standard notification of "command not found" which is
generally associated with the exit status 127.  We encountered this
value at the very beginning of this chapter:

    $ hhhhhh
    bash: hhhhhh: command not found
    $ echo $?
    127

Another exit status to which attention should be drawn is "permission
denied", usually the code 126.  When you encounter this value, it may
be worth enhancing the level of attention.  The command you are trying
to execute requires permissions you do not have.  There are some
frequent cases.

First, you attempted to execute as a normal user a command which
requires root privileges.  In this case, if you know what are you doing,
you should log in as root and then invoke the command again.  However,
if you are plenty of doubts about that command, it would probably be a
good idea to spend some time documenting yourself about.  If that
command requires root privileges, it is likely to be potentially harmful
if not [[ used correctly]{#search}]{#main}.

Second, you may be trying to run a software which has been installed
with the wrong privileges.  For instance, you may be collaborating with
other people in developing an application which is hosted in the home
path of another user and that user may have missed to allow you to run
the executable.  The `chown` and `chmod` commands can help.

There is even the possibility you are trying to obtain information about
your system without modifying the system status, so you may think this
should be possible without the need to be root.  It is often possible,
but not always.  For example, you may check the status of the `ssh` and
`at` services: the first one is normally readable while the second one
may not:

    $ /etc/init.d/sshd status
    sshd (pid 1234) is running...

    $ /etc/init.d/atd status
    bash: /etc/init.d/atd: Permission denied
    $ echo $?
    126

It may happen that an invoked command takes a long time to complete.  If
that command is needed to generate or modify information which is
required by your subsequent commands, then you should check that the
time-demanding command you've started has not been unexpectedly
terminated its execution.  This is different from checking that the
command correctly terminated (exit status 0).  That command may
encounter an error condition and decide to terminate with a non-zero
exit code.  However it is very different the case in which the command
cannot choose its exit status because it cannot terminate at all.
Whether an infinite loop inside the command requires an extern
intervention or a premature extern intervention overrides the correctly
running command,  an INT signal (which is a user interrupt) may be sent
to the command by hitting **ctrl + c** or killing the command via:

    $ kill -int pid-of-the-command

Such kind of termination may alter the expected output of the
interrupted command and break some subsequent manipulation of that
output.  When an INT signal terminates a command, a 130 exit status is
returned.  For instance, consider the `yes` command which requires a
**ctrl + c** to terminate:

    $ yes
    y
    y
    y
    ...
    # press ctrl+c
    $ echo $?
    130

That covers the concept of exit status and using it to control the flow
of your command and scripts. We hope you leave this chapter with an exit
status of zero!
