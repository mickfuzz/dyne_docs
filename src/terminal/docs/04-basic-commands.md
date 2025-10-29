# Basic commands

By now you have some basic knowledge about directories and files and you
can interact with the command line interface.  We can learn some of the
commands you'll be using many times each day.

### ls

The first thing you likely need to know before you can start creating
and making changes to files is *what's already there?*  With a
graphical interface you'd do this by opening a folder and inspecting
its contents. From the command line you use the program `ls` instead to
list a folder's contents.

    $ ls
    Desktop  Documents  Music  Photos

By default, `ls` will use a very compact output format. Many terminals
show the files and subdirectories in different colors that represent
different file types.  Regular files don't have special coloring
applied to their names.  Some file types, like JPEG or PNG images, or
tar and ZIP files, are usually colored differently, and the same is true
for programs that you can run and for directories.  Try `ls` for
yourself and compare the icons and emblems your graphical file manager
uses with the colors that ls applies on the command line.  If the output
isn't colored, you can call `ls` with the option `--color`:

    $ ls --color

### man, info & apropos

You can learn about options and arguments using another program called
`man` (`man` is short for manual) like this:

    $ man ls

Here, `man` is being asked to bring up the manual page for `ls`. You can
use the arrow keys to scroll up and down in the screen that appears and
you can close it using the **q** key (for quit).

An alternative to obtain a comprehensive user documentation for a given
program is to invoke `info` instead of `man`:

    $ info ls

This is particularly effective to learn how to use complex GNU
programs.  You can also browse the `info` documentation inside the
editor Emacs, which greatly improves its readability.  But you should be
ready to take your first step into the larger world of Emacs.  You may
do so by invoking:

    $ emacs -f info-standalone

that should display the Info main menu inside Emacs (if this does not
work, try invoking `emacs` without arguments and then type **Alt + x
info**, i.e. by depressing the **Alt** key, then pressing the **x** key,
then releasing both keys and finally typing **info** followed by the
**Return** or **Enter** key).  If you type then **m ls**, the
interactive Info documentation for `ls` will be loaded inside Emacs.  In
the standalone mode, the **q** key will quit the documentation, as usual
with `man` and `info`.

Ok, now you know how to learn about using programs yourself.  If you
don't know what something is or how to use it, the first place to look
is its `man`ual and `info`rmation pages.  If you don't know the name of
what you want to do, the `apropos` command can help.  Let's say you
want to rename files but you don't know what command does that.  Try
`apropos` with some word that is related to what you want, like this:

    $ apropos rename
    ...
    mv (1)               - move (rename) files
    prename (1)          - renames multiple files
    rename (2)           - change the name or location of a file
    ...

Here, `apropos` searches the manual pages that `man` knows about and
prints commands it thinks are related to renaming.  On your computer
this command might (and probably will) display more information but
it's very likely to include the entries shown.

Note how the program names include a number besides them.  That number
is called their *section*, and most programs that you can use from the
command line will be in section 1.  You can pass `apropos` an option to
display results from section 1 manuals only, like this:

    $ apropos -s 1 rename
    ...
    mv (1)               - move (rename) files
    prename (1)          - renames multiple files
    ...

At this stage, the section number isn't terribly important.  Just know
that section 1 manual pages are the ones that apply to programs you use
on the command line.  To see a list of the other sections, look up the
manual page for man using `man man`.

### mv

Looking at the results from `apropos`, that `mv` program looks
interesting.  You can use it like this:

    $ mv oldname newname

Depending on your system configuration, you may not be warned when
renaming a file will overwrite an existing file whose name happens to be
`newname`.  So, as a safe-guard, always use `-i' option when issuing
`mv` like this:

    $ mv -i oldname newname

Just as the description provided by `apropos` suggests, this program
moves files.  If the last argument happens to be an *existing*
directory, `mv` will move the file to that directory instead of renaming
it. Because of this, you can provide `mv` more than two arguments:

    $ mv one_file another_file a_third_file ~/stuff

If *~/stuff* exists, then `mv` will move the files there.  If it
doesn't exist, it will produce an error message, like this:

    $ mv one_file another_file a_third_file stuff
    mv: target 'stuff' is not a directory

### mkdir

How do you create a directory, anyway?  Use the `mkdir` command:

    $ mkdir ~/stuff

And how do you remove it?  With the `rmdir` command:

    $ rmdir ~/stuff

If you wish to create a subdirectory (say the directory *bar*) inside
another directory (say the directory *foo*) but you are not sure whether
this one exists or not, you can ensure to create the subdirectory *and*
(if needed) its parent directory without raising errors by typing:

    $ mkdir -p ~/foo/bar

This will work even for nested sub-sub-...-directories.

If the directory you wish to remove is not empty, `rmdir` will produce
an error message and will not remove it.  If you want to remove a
directory that contains files, you have to empty it first.  To see how
this is done, we will need to create a directory and put some files in
it first.  These files we can remove safely later.  Let's start by
creating a directory called *practice* in your home and change the
current working directory there:

    $ mkdir ~/practice
    $ cd ~/practice

### cp, rm & rmdir

Now let's copy some files there using the program `cp`.  We are going
to use some files that are very likely to exist on your computer, so the
following commands should work for you:

    $ cp /etc/fstab /etc/hosts /etc/issue /etc/motd .
    $ ls
    fstab  hosts  issue  motd

Don't forget the dot at the end of the line!  Remember it means "this
directory" and being the last argument passed to `cp` after a list of
files, it represents the directory in which to copy them.  If that list
is very long, you'd better learn using *globbing* (expanding file name
patterns containing wildcard characters into sets of existing file
names) or some other tricky ways to avoid wasting your time in typing
file names.  One trick can help when dealing with the copy of an entire
directory content.  Passing to `cp` the option `-R` you can recursively
copy all the files and subdirectories from a given directory to the
destination:

    $ cp -R . ~/foo
    $ ls ~/foo
    bar  fstab  hosts  issue  motd
    $ cp -R . ~/foo/bar
    $ ls -R ~/
    ~/foo:
    bar  fstab  hosts  issue  motd

    ~/foo/bar:
    fstab  hosts  issue  motd

In this case the current directory has no subdirectories so only files
were copied.  As you can see, the option `-R` can be passed even to `ls`
to list recursively the content of a given directory and of its
subdirectories.

Now, if you go back to your home and try to remove the directory called
*practice*, `rmdir` will produce an error message:

    $ cd ..
    $ rmdir practice
    rmdir: failed to remove 'practice': Directory not empty

You can use the program `rm` to remove the files first, like this:

    $ rm practice/fstab practice/hosts practice/issue practice/motd

And now you can try removing the directory again:

    $ rmdir practice

And now it works, without showing any output.

But what happens if your directories have directories inside that also
have files, you could be there for weeks making sure each folder is
empty!  The `rm` command solves this problem through the amazing option
`-R`, which as usual stands for "recursive".  In the following
example, the command fails because *foo* is not a plain file:

    $ rm ~/foo/
    rm: cannot remove `~/foo/`: Is a directory

So maybe you try `rmdir`, but that fails because *foo* has something
else under it:

    $ rmdir ~/foo
    rmdir: ~/foo: Directory not empty

So you use `rm -R`, which succeeds and does not produce a message.

    $ rm -R ~/foo/

So when you have a big directory, you don't have to go and empty every
subdirectory.

But be warned that `-R` is a very powerful argument and you may lose
data you wanted to keep!

### cat & less

You don't need an editor to view the contents of a file.  What you need
is just to display it.  The `cat` program fits the bill here:

    $ cat myspeech.txt
    Friends, Romans, Countrymen! Lend me your ears!

Here, `cat` just opens the file *myspeech.txt* and prints the entire
file to your screen, as fast as it can.   However if the file is really
long, the contents will go by very quickly, and when `cat` is done, all
you will see are the last few lines of the file.  To just view the
contents of a long file (or any text file) you can use the `less`
program:

    $ less myspeech.txt

Just as with using `man`, use the arrow keys to navigate, and press
**q** to quit.
:::


