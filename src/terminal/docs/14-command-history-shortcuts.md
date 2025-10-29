# Command History Shortcuts

The shell lets you bring back old commands and re-enter them, making
changes if you want. This is one of the easiest and most efficient ways
to cut down on typing, because repeated sequences of commands are very
common. For instance, in the following sequence we\'re going through
various directories, listing what\'s there, deleting files we don\'t
want, and saving certain files under different names:

    cd Pictures/
    ls -l status.log.*
    rm status.log.[3-5]
    mv status.log.1 status.log.bak

    cd ../Documents/
    ls -l status.log*
    rm status.log.[2-4]
    mv status.log.1 status.log.bak

    cd ../Videos/
    ls -l status.log*
    rm status.log.[2-5]
    mv status.log.1 status.log.bak

Eventually, if you had to do this kind of clean-up regularly, you would
write a script to automate it and perhaps use a cron job to run it at
regular intervals. But for now, we\'ll just see how to drastically
reduce the amount of typing you need while entering the commands
manually.

An earlier chapter showed you how to use arrow keys to move around in
your command history as if you were editing a file. This chapter shows a
more complicated and older method of manipulating the command history.
Sometimes you\'ll find the methods in this chapter easier, so it\'s
worth practicing them. For instance, suppose you know you entered the
`mv` command you want (or another one very similar to what you want) an
hour ago. Pressing the back arrow repeatedly is a lot more trouble than
recalling the command using the technique in this section.

## Recalling a command by a string

The *bang operator*, named after the ! character (an exclamation point,
or more colloquially \"bang\"), allows you to repeat recent commands in
your history.\

*!string* executes the most recent command that starts with *string*.
Thus, to execute the exact same mv command you did before, enter:

    !mv

What if you don\'t want the exact same command? What if you want to edit
it slightly before executing it? Or just want to look at what the bang
operator retrieves to make sure that\'s the command you want? You can
retrieve it without executing it by adding `:p` (for \"print\"):\

    !mv:p

We\'ll show you how to edit commands soon.\

Perhaps you issued a lot of `mv` commands, but you know there\'s a
unique string in the middle of the command you want. Surround the string
with ? characters, as follows:

    !?log?

Entering two bangs in a row repeats the last run command. A very useful
command history idiom is re-running the last command with superuser
privilege:

    sudo !!

as we all happen to type commands without the right permissions from
time to time.

While running your last command may seem to have limited use, this
method can be modified to select only portions of your last command, as
we will see later.\

## Recalling a command by number

The shell numbers each command as it is executed, in order. If you like
recalling commands by number, you should alter your prompt to include
the number (a later chapter shows you how). You can also look at a list
of commands with their numbers by executing the `history` command:

    $ history
    ...
      502  cd Pictures/
      503  ls -l status.log*
      504  rm status.log.[3-5]
      505  mv status.log.1 status.log.bak
      506  cd ../Documents/
      507  history
    $

Here we\'ve shown only the last few lines of output. If you want to
re-execute the most recent `rm` command (command number 504), you can do
so by entering:

    !504

But  the numbers are probably more useful when you think backwards. For
instance, if you remember that you entered the `rm` command followed by
three more commands, you can re-execute the `rm` command through:

    !-4

That tells the shell, \"start where I am now, count back four commands,
and execute the command at that point\".\

### Repeating arguments

You\'ll often find yourself reusing portions of a previous command,
either because you made a typo, or because you are running a sequence of
commands for a certain task. We accomplish this using the bang operator
with modifiers.

The three most useful modifiers are: \*, !\^, and !\$, which are
shortcuts for all, first, and last arguments respectively. Let\'s look
at these in order.

*\"commandName \*\"* executes the *commandName* with any arguments you
used on your last command. This maybe useful if you make a spelling
mistake. For example, if you typed *emasc* instead of *emacs*:

``` SCREEN
emasc /home/fred/mywork.java /tmp/testme.java
```

That obviously fails. What you can do now is type:

``` SCREEN
emacs !*
```

This executes `emacs` with the arguments that you last typed on the
command-line. It is equivalent to typing:

``` SCREEN
emacs /home/fred/mywork.java /tmp/testme.java
```

*\"commandName !\^\"* repeats the first argument.

``` SCREEN
emacs /home/fred/mywork.java /tmp/testme.java
svn commit !^    # equivalent to: svn commit /home/fred/mywork.java
```

*\"commandName !\$\"* repeats the last argument.

``` SCREEN
mv /home/fred/downloads/sample_screen_config /home/fred/.screenrc
emacs !$     # equivalent to: emacs /home/fred/.screenrc
```

You can use these in conjunction as well. Say you typed:

``` SCREEN
mv mywork.java mywork.java.backup
```

when you really meant to make a copy. You can rectify that by running:

``` SCREEN
cp mywork.java.backup mywork.java
```

But since you are reusing the arguments in reverse, a useful shortcut
would be:

``` SCREEN
cp !$ !^
```

For finer-grained control over arguments, you can use the double bang
with the `:N` modifier to select the Nth argument.  This is most useful
when you are running a command with `sudo`, since your original command
becomes the first argument.  The example below demonstrates how to do
it.

``` SCREEN
sudo cp /etc/apache2/sites-available/siteconfig /home/fred/siteconfig.bak
echo !^ !!:2  # equivalent to echo cp /etc/apache2/sites-available/siteconfig
```

A range is also possible with `!!:M-N`.\

### Editing arguments

Often you\'ll want to re-execute the previous command, but change one
string within it. For instance, suppose you run a command on *file1*:

    $ wc file1
         443    1578    9800 file1

Now you want to remove *file2*, which has a name very close to *file1*.
You can use the last parameter of the previous command through \"!\$\",
but alter it as follows:

    $ rm !$:s/1/2/
    rm file2

That looks a little complicated, so let\'s take apart the argument:

    !$   :   s/1/2/

The \"!\$\" is followed by a colon and then a \"s\" command, standing
for \"substitute\". Following that is the string you want to replace (1)
and the string you want to put in its place (2) surrounded by slashes.
The shell prints the command the way it interprets your input, then
executes it.

Because this kind of substitution is so common, you\'ll be glad to hear
there\'s a much simpler way to rerun a command with a minor change. You
can change only one string in the command through this syntax:\

    $ wc file1
         443    1578    9800 file1
    $ ^1^2
    wc file2

We used a caret (\^), the string we wanted to replace, another caret,
and the string we want to put in its place.\

## Searching through the Command History

Use the **Ctrl + R** key combination to perform a \"reverse-i-search\".
For example, if you wanted to use the command you used the last time you
used `snort`, start by typing **Ctrl + R**. In the terminal window
you\'ll see:

    (reverse-i-search)`':

As you type each letter (s, n, etc.) the shell displays the most recent
command that has that string somewhere. When you finish typing
\"snort\", you can use **Ctrl + R** repeatedly to search back through
all commands containing \"snort.\" When you find the command you\'re
looking for, you can press the right or left arrow keys to place the
command on an actual command line so you can edit it, or just press
**Enter** to execute the command.

## Sharing Bash History

The Bash shell saves your history so that you can recall commands from
earlier sessions. But the history is saved only when you close the
terminal. If you happen to be working in two terminals simultaneously,
this means you can\'t share commands.

To fix this\--if you want the terminal to save each command immediately
after its execution\--add the following lines to your *\~/.bashrc* file:

    shopt -s histappend
    PROMPT_COMMAND='history -a'

Learning these shortcuts can save you a tremendous amount of time so
please experiment!
:::


