# Redirection

Output redirection is one of the very powerful, and easily
misunderstood, parts of the shell. To decrease misunderstandings, we'll
keep to the basics.

The `>` operator (an "operator" is a symbol like `+`,`-`,`<`,`>` that
represents a specific action) is for redirecting output.  In a very
simple example, if you want to list the files in a directory, you type
`ls`.  That output goes to your screen.  If you want that list instead
to go to a file, however, you'd do something like this:

    $ ls > my-file-list

The file *my-file-list* now contains a list of all of the files,
directories, links and other things in the current working directory
with names that do not start with '.'. (Note that the shell creates,
if necessary, the files used for redirection before executing the
command, so the file *my-file-list* wil also be included in your list.)

The > operator is a "clobbering" operator -- if you are outputting
to an existing file, it will overwrite the old contents. Sometimes,
especially if you are keeping a logfile, what you want is the >>
operator.  It works the same way as the > operator, except that it
appends to the end of an existing file.  (If the file doesn't yet
exist, it creates it.)

There are other places you can redirect output to, like device special
files such as terminals, or */dev/null*, which is an infinitely big
empty bucket (or more accurately it just ignores all input).  If you
have a program that you know will produce voluminous output you don't
care about, you could do this:

    $ bigprogram > /dev/null

The program will execute normally, but you won't see its normal
output.  (You would, however, see any of its error output; more detail
below under File Descriptors).

The < operator is for redirecting input.  Most programs that would
expect input from your terminal are happy to accept it from another
source instead, such as an existing file.

For example, if you wish to email the contents of *myfile.txt* to joe
you could do this:

    $ mail joe < myfile.txt

The redirection operators are particularly relevant for jobs running in
the background.  When working with a graphical interface, you are
already familiar with the concept of switching windows by, for example,
minimizing the current window that is being engaged in playing musics
and restoring another window to resume browsing the Internet.  Such a
situation also happens when you are working with a command-line
interface.  However, instead of minimizing the command to play your
music files, you run it in background by appending an ampersand (&) at
the end of the command like this:

    $ ogg123 *.ogg &

Alas, any output it produces like announcing a new track along with its
title and description goes to your terminal as usual cluttering your
text-based Internet browser's screen.  So, you may want to avoid it.
Many programs have a silent mode to suppress the normal output.  Usually
this mode can be enabled using the options `-q` or `--quiet`, but this
is not a general convention and you should read the `man` or `info`
documentation before relying on these options.  If you want to be sure
avoiding any output irrespective of gentle program options, you can
easily do so by redirecting the output to a file.  For instance, the
following command places the output of `ogg123` into */dev/null* because
you do not care about any track announcement as long as the musics keep
playing and any error messages into *music_err* so that you can easily
find out why the musics suddenly stop playing.  This way, no output can
confuse you by appearing at the terminal and you can always have a look
at the result at a later time by doing something like `cat music_err`:

    $ ogg123 *.ogg >/dev/null 2>music_err &

A program running in the background cannot accept input from the
terminal.  So if you mistakenly put such a program in the background
(and don't redirect input from a file through the < operator), it will
get stuck waiting when it has to accept input.
