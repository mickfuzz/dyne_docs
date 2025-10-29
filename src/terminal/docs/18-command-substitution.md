# Command Substitution

In the shell, you can execute one command inside another. Here\'s a
simple example:

    grep `date +%b` apache_error_log

The **back-quote** key (also called the backtick) is usually located at
the same place as the tilde, above the **Tab** key (dependent on your
keyboard layout).

The command within the backticks \`\` is executed first. The output is
then plugged into the larger command. So first the shell executes:

    date +%b

This is the `date` command with an argument beginning with a + sign to
indicate a format that you want for the output. The `%b` format is a
rather odd convention asking the `date` command to print just a
three-letter abbreviation of the current month. For instance, if we
execute this command in March, it prints:

    Mar

So the three-letter abbreviation of the current month is now inserted
into the surrounding `grep` command. In the month of March, the command
is equivalent to:

    grep Mar apache_error_log

It just so happens that the *apache_error_log* file stores log messages
with dates, and the date contains the three-letter abbreviation of the
month:

    [Mon Mar 09 14:44:23 2009] [notice] Apache/2.0.59 (Ubuntu) PHP/5.2.6 DAV/2 configured

So what is the effect of our command? It displays all the log messages
from *apache_error_log* that were logged during the current month. (Of
course, if there are multiple years in a single log file, you could get
messages from March of previous years\--but this example is meant to be
simple.) By embedding the `date` command in the `grep` command, we have
created a command we can store and execute any time without having to
specify the right month. For instance, we could store this in the
*.bashrc* start-up file:

    alias monthlog="grep `date +%b` apache_error_log"

Now we have our very own command, `monthlog`, to display current Apache
log messages.\

In Bash, you can do the same thing with a syntax many people find
simpler:

    grep $(date +%b) apache_error_log

Instead of backticks, insert a dollar sign and put the command between
parentheses.\

Command substitution is like a pipe (the \| character). But command
substitution is more flexible than a pipe because you can put one
command anywhere you want inside another. There is one other subtle
difference: a pipe allows both commands to execute at the same time. If
an embedded command takes a long time, the outer command doesn\'t
execute at all until the embedded one is done.

If the embedded command could produce output that is more than one word
(such as "Mar 09") you can pass it as a single argument by enclosing
the command in double quotes. The `grep` command in this section, for
instance, requires the string to be passed as a single argument.
:::


