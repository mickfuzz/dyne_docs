# Parameter Substitution

As we saw in the chapter on variables, you can put braces around a
variable name to set it off from its surroundings:

    $ curr=myfile
    $ rm ${curr}.jpeg

There are also some nifty tricks you can perform inside the braces, such
as changing parts of the string. Suppose you have a file named
*mypicture.jpeg* instead of *myfile.jpeg*. You could alter the \$curr
variable when you insert it into a command:

    $ rm ${curr/file/picture}.jpeg

## Playing Safe With Variables That Don't Exist

Sometimes you might be using variables that have been removed (which you
can do with the `unset` command) or were never initialized in the first
place.  Since by default the shell uses an empty string for a
nonexistent or undefined variable, as in the case of the `rm` command we
showed earlier, it\'s useful to be able to substitute a default value
for a variable.

    $ cat "${VARIABLE_FILE_NAME:-/home/user/file}"

The \'**:-**\' operator asks the shell to check whether the variable is
set. to see if it exists and is set to some string.  If it was never
defined, or has no value, the shell substitutes the text after
\'**:-**\'.

    $ cat "${VARIABLE_FILE_NAME:=/home/user/file}"

The \'**:=**\' operator will do much the same, but instead of just
substituting */home/user/file* in the current `cat` command, if
VARIABLE_FILE_NAME doesn\'t exist, the shell will also set the variable
to the alternative text.

## Cutting Corners With Variable Expansion

Variable expansion is by no means limited to filenames. It is also a
handy way to pass complex, frequently used options to commands.

    $ export ALT_LS='--color=always -b -h --filetype'
    $ ls $ALT_LS

Since the alternative options are stored and expanded in variable form,
you can use whatever defaults you like for ninety-percent of your work
but quickly use an alternative form in special cases.\

Parameter expansion is an excellent way to abstractly deal with multiple
files or tediously long series of options.  Once you understand it,
it\'s bound to expand your abilities on the command line.\
:::


