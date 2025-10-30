# The Parts of a Command

The first word you type on a line is the command you wish to run.  In
the "Getting Started" section we saw a call to the `date` command,
which returned the current date and time.

## Arguments

Another command we could use is `echo`, which displays the specified
information back to the user.  This isn't very useful if we don't
actually specify information to display.  Fortunately, we can add more
information to a command to modify its behavior; this information
consists of *arguments* .  Luckily, the `echo` command doesn't argue
back; it just repeats what we ask it:

    $ echo foo
    foo

In this case, the argument was **foo**, but there is no need to limit
the number of arguments to one. Every word of the text entered,
excluding the first word, will be considered an additional argument
passed to the command. If we wanted `echo` to respond with multiple
words, such as **`foo bar`**, we could give it multiple arguments:

    $ echo foo bar
    foo bar

Arguments are normally separated by "white space" (blanks and tabs --
things that show up white on paper).  It doesn't matter how many spaces
you type, so long as there is at least one. For instance, if you type:

    $ echo foo              bar
    foo bar

with a lot of spaces between the two arguments, the "extra" spaces are
ignored, and the output shows the two arguments separated by a single
space.  To tell the command line that the spaces are part of a single
argument, you have to delimit in some way that argument.  You can do it
by *quoting* the entire content of the argument inside double-quote
(`"`) characters:

    $ echo "foo              bar"
    foo              bar

As we'll see later, there is more than a way to quote text, and those
ways may (or may not) differ in the result, depending on the content of
the quoted text.

## Options

Revisiting the `date` command, suppose you actually wanted the UTC
date/time information displayed.  For this, `date` provides the
**`--utc`** option.  Notice the two initial hyphens.  These indicate
arguments that a command checks when it starts and that control its
behavior.  The `date` command checks specially for the **`--utc`**
option and says, "OK, I know you're asking for UTC time".  This is
different from arguments we invented, as when we issued `echo` with the
arguments **`foo bar`**.

Other than the dashes preceding the word, **`--utc`** is entered just
like an argument:

    $ date --utc
    Tue Mar 24 18:12:44 UTC 2009

Usually, you can shorten these options to a shorter value such as
**`date -u`** (the shorter version often has only one hyphen).  Short
options are quicker to type (use them when you are typing at the shell),
whereas long options are easier to read (use them in scripts).

Now let's say we wanted to look at yesterday's date instead of
today's.  For this we would want to specify the **`--date`** argument
(or shortly **`-d`**), which takes an argument of its own. The argument
for an option is simply the word following that option. In this case,
the command would be `date --date yesterday`.

Since options are just arguments, you can combine options together to
create more sophisticated behaviour.  For instance, to combine the
previous two options and get  yesterday's date in UTC you would type:

    $ date --date yesterday -u
    Mon Mar 23 18:16:58 UTC 2009

As you see, there are options that expect to be followed by an argument
(`-d`, `--date`) and others that don't take any one (`-u`, `--utc`).
Passing a little bit more complex argument to the `--date` option allows
you to obtain some interesting information, for example whether this
year is a leap year (in which the last day of February is 29).  You need
to known what day immediately precedes the 1st of March:

    $ date --date "1march yesterday" -u
    Sat Feb 28 00:00:00 UTC 2009

The question you posed to `date` is: if today were the 1st of March of
the current year, what date would it be yesterday?  So no, 2009 is not a
leap year.  It may be useful to get the weekday of a given date, say the
2009 New Year's Eve:

    $ date -d 31dec +%A
    Thursday

which is the same as:

    $ date --date 31december2009 +%A
    Thursday

In this case we passed to `date` the option `-d` (`--date`) followed by
the New Year's Eve date, and then a special argument (that is specific
to the `date` command). ⁞ Commands may once in a while have strange
esoteric arguments...  The `date` command can accept a *format*
argument starting with a plus (`+`).  The format `%A` asks to print the
weekday name of the given date (while `%a` would have asked to print the
abbreviated weekday: try it!).  For now don't worry about these
hermetic details: we'll see how to obtain help from the command line in
learning command details.  Let's only nibble a more savory morsel that
combines the `echo` and `date` commands:

    $ echo "This New Year's Eve falls on a $( date -d 31dec +%A )"
    This New Year's Eve falls on a Thursday

## Repeating and editing commands

Use the **Up-arrow** key to retrieve a command you issued before.  You
can move up and down using arrow keys to get earlier and later
commands.  The **Left-arrow** and **Right-arrow** keys let you move
around inside a single command.  Combined with the **Backspace** key,
these let you change parts of the command and turn it into a new one.
Each time you press the **Enter** key, you submit the modified command
to the terminal and it runs exactly as if you had typed it from scratch.
