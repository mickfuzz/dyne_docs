# Maintainable Scripts

You are slowly delving into programming by the way of shell scripting.
Now it's the best time to start to learn about how to be a good
programmer.  Since this book is just an introduction to the command
line, we are only going to provide few but nevertheless very important
hints centered around the idea of *maintainability*.

When programmers talk about maintainability they are talking about the
ease with which a program can be modified, whether it's to correct
defects, add new functionality, or improve its performance.
Unmaintainable programs are very easy to spot: they lack structure, so
functionality is spread all over the place. When you push *here* they
break way over *there*, a real nightmare. In general, they are very hard
to read.  Consider for example this:

    #!/bin/sh
    identify `find ~/Photos/Vacation/2008 -name *.jpg` | cut -d ' ' -f 3 | sort | uniq -c

use your favorite editor to save this file as *foo*, then:

    $ chmod +x foo
    $ ./foo
         11 2304x3072
         12 3072x2304

What that small monster does is find files that ends with ".jpg" in a
certain directory, run `identify` on all of them, and report some kind
of information that someone at some time must have thought very useful.
If the programmer would only have added some hints as to what the
programs does...

## Don't use long lines

The first thing you'll note is that our example of an unmaintainable
program is one long line. There's really no need for that.  What if the
program looked like this instead:

    #!/bin/sh
    identify `find ~/Photos/Vacation/2008 -name *.jpg` |
    cut -d ' ' -f 3 |
    sort |
    uniq -c

It becomes a little bit easier to spot where each command begins and
ends.  It's still the same set of piped programs, only their
presentation is different.  You can break long lines at pipes and the
functionality will be the same.

You can also split one command into several lines by using the **\**
character at the end of a line to join it with the next:

    #!/bin/sh
    echo This \
         is \
         really \
         one \
         long \
         command.

## Use descriptive names for your scripts

The second thing you might have noticed is that the script is called
"foo".  It's short and convenient but it doesn't provide a clue as
to what the program does.  What about this:

    $ mv foo list_image_sizes

Now the name helps the user understand what the script does.  Much
better, isn't it?

## Use variables

One bothersome thing about that program is its use of backticks.  Sure,
it works, but it also has drawbacks.  Perhaps the biggest one is the
least evident one, too: remember that backticks substitute the output of
the command they contain in the position where they appear.  Some
systems have a limit of the command line length they allow.  In this
particular case, if the specified directory has lots and lots of
pictures, the command line can become extraordinarily long, producing an
obscure error when you call the program.  There are several methods that
you can use to remedy this, but for the purpose of this explanation,
let's try the following:

    #!/bin/sh
    find ~/Photos/Vacation/2008 -name *.jpg |
    while read image ; do identify $image ; done |
    cut -d ' ' -f 3 |
    sort |
    uniq -c

Now `find` is running the same as before, but its output, the list of
filenames, is piped into a while-loop.  The condition for the loop is
`read image`.  `read` is a function that reads one line at a time,
splits its input into fields and then assigns each field to a variable,
*image* in this case.  Now `identify` works on one image at a time.

Notice how introducing a variable makes the program a bit easier to
read: it literally says that you wish to identify an image.  Also note
how the effect on future programmers wouldn't have been the same if the
variable was called something like *door* or *cdrom*.  Names are
important!

But there's still something bothersome about the program: that
directory name is glowing like a sore thumb.  What if we change the
program like this:

    #!/bin/sh
    START_DIRECTORY=~/Photos/Vacation/2008

    find $START_DIRECTORY -name *.jpg |
    while read image ; do identify $image ; done |
    cut -d ' ' -f 3 |
    sort |
    uniq -c

That's a little bit better: now you can edit your script and change the
directory each time you wish to process a different one.

## Use arguments

That last bit didn't sound quite right, did it?  After all, you don't
edit `ls` each time you wish to list the contents of a different
directory, do you? Let's make our program just as adaptable:

    #!/bin/sh
    START_DIRECTORY=$1

    find $START_DIRECTORY -name *.jpg |
    while read image ; do identify $image ; done |
    cut -d ' ' -f 3 |
    sort |
    uniq -c

The *$1* variable is the first argument that you pass to your script
($0 is the name of the script you're running).  Now you can call your
script like this:

    $ ./list_image_sizes ~/Photos/Vacation/2008

Or you can examine the 2007 pictures, if you wish:

    $ ./list_image_sizes ~/Photos/Vacation/2007

## Know where you begin

Consider what happens if you run the script like this:

    $ ./list_image_sizes

Maybe that's what you want, but maybe it isn't.  What happens is that
*$1* is empty, so *$START_DIRECTORY* is empty as well and in turn the
first argument to find is also empty.  That means that find will search
your current working directory.  You might wish to make that behavior
explicit:

    #!/bin/sh
    if test -n "$1" ; then
        START_DIRECTORY=$1
    else
        START_DIRECTORY=.
    fi

    find $START_DIRECTORY -name *.jpg |
    while read image ; do identify $image ; done |
    cut -d ' ' -f 3 |
    sort |
    uniq -c

The program behaves exactly as before, with the only difference that in
six months, when you come back and look at the program, you won't have
to wonder why it's producing results even when you don't pass it a
directory as argument.

## Look before you leap

Speaking of which, what happens if you do pass an argument to the
script, but that argument isn't a directory or better yet, it doesn't
even exist?  Try it.

Not pretty, ah?

What if we do this:

    #!/bin/sh
    if test -n "$1" ; then
        START_DIRECTORY=$1
    else
        START_DIRECTORY=.
    fi

    if ! test -d $START_DIRECTORY ; then
        exit
    fi

    find $START_DIRECTORY -name *.jpg |
    while read image ; do identify $image ; done |
    cut -d ' ' -f 3 |
    sort |
    uniq -c

That's better.  Now the script won't even attempt to run if the
argument it receives isn't a directory.  It isn't very polite, though:
*it silently exits* with no hint of what went wrong.

## Complain if you must

That's easily fixed:

    #!/bin/sh
    if test -n "$1" ; then
        START_DIRECTORY=$1
    else
        START_DIRECTORY=.
    fi

    if ! test -d $START_DIRECTORY ; then
        echo "$START_DIRECTORY" is not a directory or it does not exist.  Stop.
        exit
    fi

    find $START_DIRECTORY -name *.jpg |
    while read image ; do identify $image ; done |
    cut -d ' ' -f 3 |
    sort |
    uniq -c

## Mind your exit

The program now produces an error message if you don't pass it an
existing directory as argument and it exits without further action.  It
would be nice if you let other programs that might eventually call your
script know that there was an error condition.  That is, it would be
nice if your program exits with an error code.  Something like this:

    #!/bin/sh
    if test -n "$1" ; then
        START_DIRECTORY=$1
    else
        START_DIRECTORY=.
    fi

    if ! test -d $START_DIRECTORY ; then
        echo "$START_DIRECTORY" is not a directory or it does not exist.  Stop.
        exit 1
    fi

    find $START_DIRECTORY -name *.jpg |
    while read image ; do identify $image ; done |
    cut -d ' ' -f 3 |
    sort |
    uniq -c

Now, if there's an error, your script's exit code is 1. If the program
exits normally, the exit code is 0.

## Use comments

Anything following a **#** symbol on a line will be ignored, allowing
you to add notes about how your script works.  For example:

    #!/bin/sh
    # This script reports the sizes of all the JPEG files found under the current
    # directory (or the directory passed as an argument) and the number of photos
    # of each size.

    if test -n "$1" ; then
        START_DIRECTORY=$1
    else
        START_DIRECTORY=.
    fi

    if ! test -d $START_DIRECTORY ; then
        echo "$START_DIRECTORY" is not a directory or it does not exist.  Stop.
        exit 1
    fi

    find $START_DIRECTORY -name *.jpg |
    while read image ; do identify $image ; done |
    cut -d ' ' -f 3 |
    sort |
    uniq -c

Comments are good, but don't fall prey to writing too many comments.
Try to construct your program so that the code itself is clear.  The
reason behind this is simple: next year, when *someone else* changes
your script, that other person could well change the commands and forget
about the comments, making the later misleading.  Consider this:

    # count up to three
    for n in `seq 1 4` ; do echo $n ; done

Which one is it?  Three or four?  Evidently the program is counting up
to four, but the comment says it's up to three.  You could adopt the
position that the program is right and the comment is wrong.  But what
if the person who wrote this meant to count to three and that's the
reason why the comment is there?  Let's try it like this:

    # There are three little pigs
    for n in `seq 1 3` ; do echo $n ; done

The comment documents the *reason* why the program is counting up to
three: it is not describing what the program *does*, it's describing
what the program *should do*.  Let's consider a different approach:

    TOTAL_PIGS=3
    for pig in `seq 1 $TOTAL_PIGS` ; do echo $pig ; done

Same result, slightly different program.  If you reformat your program,
you can do without the comments (as a side note, the fancy word for the
kinds of change we have been making is *refactoring*, but that goes
outside the scope for this book).

## Avoid magic numbers

In our current example, there's a *magic number*, a number that makes
the program work, but no one knows why it has to be *that* number.
It's magic!

    ...
    cut -d ' ' -f 3 |
    ...

You have two choices: write a comment and document why it has to be
"3" instead of "2" or "4" or introduce a variable that explains
why by way of its name.  Let's try the latter:

    #!/bin/sh
    # This script reports the sizes of all the JPEG files found under the current
    # directory (or the directory passed as an argument) and the number of photos
    # of each size.

    if test -n "$1" ; then
        START_DIRECTORY=$1
    else
        START_DIRECTORY=.
    fi

    if ! test -d $START_DIRECTORY ; then
        echo "$START_DIRECTORY" is not a directory or it does not exist.  Stop.
        exit 1
    fi

    IMAGE_SIZE_FIELD=3

    find $START_DIRECTORY -name *.jpg |
    while read image ; do identify $image ; done |
    cut -d ' ' -f $IMAGE_SIZE_FIELD |
    sort |
    uniq -c

It does improve things a little; at least now we know where the 3 comes
from.  If ImageMagick ever changes the output format, we can update the
script accordingly.

## Did it work?

Last but not least, check the exit status of the commands you run.  As
it stands right now, in our example there's not much that can fail.  So
let's try one last example:

    #!/bin/sh
    # Copy all the HTML and image files present in the source directory to the
    # specified destination directory.

    SRC=$1
    DST=$2

    if test -z "$SRC" -o -z "$DST" ; then
        cat<<EOT
    Usage:

        $0 source_directory destination_directory
    EOT
        exit 1
    fi

    if ! test -d "$SRC" ; then
        echo "$SRC" is not a directory or it does not exist.  Stop.
        exit 1
    fi

    if test -e "$DST" ; then
        echo "$DST" already exists.  Stop.
        exit 1
    fi

    if ! mkdir -p "$DST" ; then
        echo Can't create destination directory "$DST".  Stop.
        exit 1
    fi

    # Obtain the absolute path for $DST
    cd "$DST"
    DST=`pwd`
    cd -

    cd "$SRC"

    find ! -type d ( -name *.html -o -name *.jpg -o -name *.png ) |
    while read filename ; do
        dir=`dirname "$filename"`
        mkdir -p "$DST/$dir" && cp -a "$filename" "$DST/$filename"
        if test $? -ne 0 ; then
            echo Can't copy "$filename" to "$DST/$filename"
            echo Abort.
            exit 1
        fi
    done

Note that this example makes use of many things you learned in this
book. It does not try to be definitive; you can practice improving it!

The thing you should note now is how the program pays attention to the
error conditions that the different programs might produce.  For
example, instead of just calling `mkdir` to check if a program worked,
it does this:

    if ! mkdir -p "$DST" ; then
        echo Can't create destination directory "$DST".  Stop.
        exit 1
    fi

It calls `mkdir` as the condition for `if`.  If `mkdir` encounters an
error, it will exit with a non-zero status and the `if` clause will
interpret that as a false condition. The "!" is a negation operator
that inverts false to true (or vice versa. So the line as a whole
basically says "Run the `mkdir` command, turn an error into a true
value with the "!" operator, and take action if it's true that
there's an error." In short, if `mkdir` encounters an error, the flow
will enter the body of the `if`.  This might happen, for example, if the
user running the script doesn't have permissions to create the
requested directory.

Note also the usage of "&&" to verify error conditions:

    mkdir -p "$DST/$dir" && cp -a "$filename" "$DST/$filename"

If `mkdir` fails, `cp` won't be called.  Furthermore, if either `mkdir`
or `cp` fails, the exit status will be non-zero.  That condition is
checked in the next line:

    if test $? -ne 0 ; then

Since this might indicate something going awfully wrong (e.g., is the
disk full?), we had better give up and stop the program.

## Wrapping up

Writing scripts is an art. You can become a better artist by looking at
what others have done before you and doing a lot yourself. In other
words: *read a lot of scripts and write a lot of scripts yourself*.

Happy hacking!
