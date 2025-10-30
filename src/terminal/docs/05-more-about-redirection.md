# More about redirection

How do pipes work? They use three communication channels provided to
every executing command.

**stdin** (standard input) by default is what we type on the keyboard.
We can use "<" with a filename to make a program take input from a
file.

**stdout** (standard output) by default is printed on your computer
screen. We can use ">" with a filename to send that to a file,
overwriting whatever is there, or we can use ">>" to append standard
output to the end of the file.

**stderr** (standard error)  is an alternative kind of output. Programs
use it to send error messages. This can be useful because you might want
to see error messages on the terminal even if you redirect output to a
file. Here's an example:

    $ ls *.bak > listfile
    ls: *.bak: No such file or directory

Here, we wanted a list of all files ending in *.bak*. But no such files
exist in this directory. If `ls` sent its error message to standard
output (which in this case has been directed to a file), we wouldn't
know that there is a problem without looking at the content of
*listfile*. But because `ls` sent its message to standard error, we see
it. The error message starts with the name of the program (`ls`)
followed by a colon and the actual message.

A pipe simply redirects the standard output of the first program to the
standard input of the second:

    $ ls *.bak | more

Sometimes, we want to direct the output of a command to a file, but we
also want to see the output as the program runs. The `tee` command does
just that:

    $ ls -lR / | tee allMyFiles

provides a complete, detailed list of your file system, saved to
*allMyFiles*. This takes some time to run; `tee` saves you from staring
at a lifeless screen, wondering whether any thing's happening.

Each program can open a lot of files, and each has a number called a
*file descriptor* that is meaningful only within that program. The first
three numbers are always reserved for the file descriptors we just
described.

  --------- ----
   stdin     0
   stdout    1
   stderr    2
  --------- ----

## Redirecting stderr

When we redirect stdin as we did above, error messages still go to the
screen.  For example

    $ ls /nosuchplace > /dev/null
    ls: /nosuchplace: No such file or directory
    $

To redirect stderr we have to use the more general form of redirection,
which uses the file numbers mentioned in the previous section, and looks
like this.

    $ ls /nosuchplace 2>/tmp/errors
    $

This sends the error message sent to file number 2 (*stderr*) into the
file */tmp/errors*.

Now we can introduce a more complex redirection, which redirects
standard output and standard error to the same file:

    $ ls *.bak > listfile 2>&1

The & in that command has nothing to do with putting a command in the
background. The & here must directly follow the > character, and it
sends file number 2 onto file number 1.

Or in the case of a pipe, put this before the pipe:

    $ ls *.bak 2>&1 | more

## Adding more descriptors

Sometimes it is convenient to keep other files open and add to them in
dribs and drabs. You can do this with redirection and `exec`.

    $ exec 3>/tmp/thirdfile
    $ exec 4>/tmp/fourthfile
    $ echo drib >&3
    $ echo drab >&4
    $ echo another drib >&3
    $ echo another drab >&4
    $ exec 3>&-
    $ exec 4>&-

The first two lines open connections to two more file descriptors, 3 and
4.  We can then echo text onto them, redirect programs into them, etc.
using `>&3` or `>&4`.  Finally, we close them with the `3>&-` and `4>&-`
syntax.
