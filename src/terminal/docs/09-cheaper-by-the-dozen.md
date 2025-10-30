# Cheaper by the dozen

After getting used to the command line, you will start looking for ways
to do more in less time.  One of the easiest ways to achieve that is to
work on multiple files at the same time, so that instead of:

    $ rm this
    $ rm that
    $ rm here
    $ rm there

you just remove all those files with one command. Many commands,
including `rm`, let you simply specify all the files you want to delete
as arguments in one go:

    $ rm this that here there

Still, there has to be a better way!

## Globbing

File globbing is the shell's way of dealing with multiple files with
the fewest characters possible.  The shell treats certain characters as
codes that you can use to specify groups of things you want the commands
to affect. These characters are commonly called "wildcards" because
they're like a card in a game that the players have designated to
represent anything you want.

### The "*" Wildcard

Imagine a directory of files:

    $ ls
    here  that  there  this

that you want to delete.  A tedious job can be turned simple by using
the * or asterisk wildcard.

    $ rm *

When used by itself, the asterisk wildcard refers to all the items in
the directory except for those with names starting with ".". We say
that the shell *expands* the wildcard. Knowing what's in the directory,
the shell substitutes those filenames for the asterisk and effectively
executes the following command:

    $ rm here that there this

You can combine * with other characters, however, to make it selective.

    $ rm t*
    $ ls
    here

What happened here? The shell looked at "t" first and then expanded
the asterisk to cover all the files that begin with "t".  If you had
requested "h*" instead, the shell would have removed any file that
started with an "h". Let's make the directory like it was and then
remove the "h" files:

    $ touch that there this
    $ rm h*
    $ ls
    that  there  this

The asterisk wildcard can be placed anywhere within a word. Let's
switch to an `ls` command because it's easier to see what's happening
with wildcards:

    $ ls th*re
    there

By switching from `rm` to `ls` we see an important aspect of wildcard:
you can use them with any command, because the shell interprets them
before it even invokes the command. In fact, you can't issue a command
*without* taking into account the behavior of wildcards, because
they're a feature of the shell.  (Luckily, you're not likely to ever
have to deal with a filename that contains a real asterisk.)

Multiple asterisks can also be used together. For instance, in this way
you can find filenames where the middle of a series is the same, but
they start and end differently. Let's try it on the original four
files:

    $ ls *i*
    this

People often use the asterisk to remove files that are all of one type.
For instance, if you've been working with a lot of photos and want to
clean up files ending in *.jpg* when you're finished, you can remove
all the ones in the current directly as follows:

    $ rm *.jpg

Suppose you have some files ending in  *.jpg* and some ending in
*.jpeg*. The asterisk still makes clean-up easy:

    $ ls *.jp*g

And suppose the JPEG files are scattered among several subdirectories.
You have directories named photos1, photos2, photos3, and so forth, each
containing JPEGs you want to remove. A wildcard can help you list all
the contents of those subdirectories:

    $ ls photos*
    photos1:
    centraal_station.jpg    nieuwe_kerk.jpg

    photos2:
    ica.jpeg                sanders_theater.jpeg

    photos3:
    bayeux_cathedral.jpeg   rouen_cathedral.jpeg    travel.odt

And you can specify a directory along with the filenames you remove:

    $ rm photos*/*.jp*g
    $ ls photos*
    photos1:

    photos2:

    photos3:
    travel.odt

Only the *travel.odt* file remains (because it doesn't match
".jp*g") as a record of all the trips you've taken.

There is, however, one limit to the asterisk wildcard.  By default, it
will not match any hidden files (those with filenames that start with a
dot, you need to `ls -a` to see these).

    $ ls -a
    .
    ..
    .hidden
    this
    that
    here
    there
    $ rm *
    $ ls -a
    .
    ..
    .hidden

If you want those hidden files deleted by a wildcard it is necessary to
append a dot to the front of the wildcard. Note that normal files (those
that are not hidden/do not start with a dot) will not be deleted when
you do this:

    $ ls -a
    .
    ..
    .hidden
    here
    $ rm .*
    $ ls -a
    .
    ..
    here

Finally, it's important to note that the asterisk can also match
nothing when appropriate, as seen in the following example:

    $ ls task*
    task  taskA  taskB  taskXY

 This is because the asterisk matches zero or more occurrences.  So, as
in this example, "task*" matches any filename that starts with
"task" even if it only consists of just "task".

**CAUTION:** When you use just an asterisk ("*") with **`rm`**, and
basically any other command, it is always a good idea to put an option
terminator ("--") before the wildcard like this:

    $ rm -- *

Take this case for example:

    $ ls

    -r    directory1    directory2    file1.txt
    $ rm *

Normally, **rm** will not remove sub-directories and their contents,
however, in this case everything in the directory will be removed even
the sub-directories. This is because the asterisk("*") is expanded to
"-r directory1 directory2 file1.txt". Although "-r" is a filename,
**`rm`** will confuse it as an option and think it has been told to
delete the directories and their contents as well. Using an the option
terminator ("--") will prevent **`rm`** from treating anything
following the terminator as an option. Therefore, generally it is a good
idea to always use an option terminator after typing a command and its
options, if there is any, to prevent the command from treating a
filename as an option.

### The "?" Wildcard

The "?" or question mark wildcard is very similar to the asterisk
wildcard.  The crucial difference is that the question mark wildcard
takes the place of only one character.

    $ ls task*
    task  taskA  taskB  taskXY
    $ ls task?
    taskA  taskB
    $ ls task??
    taskXY

As we've already seen, the asterisk matches all the files beginning
with "task". A single question mark matches files that have a single
character after "task". The double question mark requires exactly two
characters in that position.

### The "[ ]" Wildcards

The square brackets wildcards can get even more specific, denoting a
ranges of characters.The following `ls` command includes a `-1` (the
digit "one") option, which means "list one entry on each line." This
makes it easier to see how the files in this example differ.

    $ ls -1
    file_1
    file_2
    file_3
    file_a
    file_b
    file_c

By using the square brackets, you can remove specific files without
typing every name completely.

    $ rm file_[13ac]
    $ ls -1
    file_2
    file_b

Furthermore, within the square brackets, the order of the characters
doesn't matter.

Combining square brackets with a hyphen, you can also do ranges of
files. Let's start with a directory containing lots of files ending in
numbers:

    $ ls
    file_1
    file_2
    file_3
    ...
    file_9

At first it might be tempting to use the asterisk wildcard here.
However, what if we need to remove only files 2-6? We could list each
suffix in the brackets, but you would still have to type five numbers.
Fortunately, there is a much easier way.

    $ rm file_[2-6]

Now the only files left are files 1 and 7-9.  By using the dash between
a set of numerals in the square brackets, you make the shell expand the
pattern by creating a name with every number between the starting value
to the left of the dash and the end value to the right.

Ranges aren't just for numbers.  They can also use letters.

    $ ls -1
    file_a
    file_b
    file_c
    file_d
    $ rm file_[a-c]
    $ ls -1
    file_d

Both letters and ranges can be combined into the same instance of square
brackets.

    $ ls -1
    file_a
    file_b
    file_c
    file_1
    file_2
    $ rm file_[a-c12]
    $ ls

 Character groups can be inverted by prefixing them with the **^**
(caret) character:

    $ ls -1
    file_a
    file_b
    file_c
    file_d
    file_1
    file_2
    $ ls -1 file_[^c-z2-9]
    file_a
    file_b
    file_1

**CAUTION:** Ranges can, at times, be tricky things. For one, their
order can be affected by the current locale settings (in some locales
[A-C] could mean the same as [ABCabc], while in others it could be
equivalent to [ABC]). A good rule of thumb is to always know which
files you are working with. You can do this by simply substituting
**echo** or **ls** for whatever command you intend to run, such as:

    $ ls -1 file[A-b]
    fileA
    fileB
    filea
    fileb

This allows you to ensure the pattern matches the files you want to work
with.

## Brace Expansion

We've seen the ability to get a range of characters or letters that
fall in a single digit range (0-9 in our examples) but what about when
you need to match a range of files that uses double or even triple
digits?

    $ ls -1
    file_1
    file_2
    file_3
    ...
    file_78
    $ rm file_[1-20]
    $ ls -1
    file_3
    ...
    file_78

Since the brackets glob can only interpret single character ranges it
interprets, "1-20" not as a range but as the characters: "1", "-",
"2", and "0".   Causing only "file_1" and "file_2" to be deleted
because they are the only ones that match.  If you want to access ranges
larger than 0-9, you have to using Bash's brace expansion,
"{start..end}".

    $ rm file_{1..20}

In a brace range the double dot is the delimiter instead of a dash.

Braces can also be used when you need to get a series of files that have
a common pattern but subtle differences.  Such as with:

    $ ls
    file.txt
    file.pdf
    file.pl
    file.odf

If you just wanted to delete the *pdf*, *odf*, and *txt* files you could
specify a comma separated list of strings in a brace pair:

    $ rm file.{txt,pdf,odf}
    $ ls
    file.pl

## Globbing When No File Matches

Suppose you specify a wildcard and the shell can't find any matching
filename:

    $ ls -1
    file_a
    file_b
    file_c
    file_d
    $ rm file?
    rm: cannot remove `file?': No such file or directory

When there is no file to match a pattern, the shell passes the wildcard
to the program unexpanded.  That's why you get an error message from
the `rm` program, not from the shell.

## Disabling A Wildcard

Okay, we know the shell will pass a wildcard as an option to a program
when it can't find a file, but what do we do when we want to send a
character that also happens to be a wildcard to our program? Here's a
common example: we want to search a file for every occurrence of an
asterisk.

    $ ls
    2file
    *file
    *?****[a-b]

Now we happen to want *file, but we get:

    *file   2file

Why? Because the asterisk is a wildcard, the shell expanded it before
sending it to `ls`.  So after expansion, the command would look like:

    $ ls *file 2file

If we want `ls` to find an asterisk something different is in order.

The "\", or backslash, tells the shell to treat the following
character as a normal character and do no expansion.

    $ ls \*file
    *file

Because the asterisk is the next character after the backslash, the
shell sends the asterisk to `ls` unmodified.  In other words: the
backslash *escapes* the asterisk.

The backslash modifier works well when we have only one wildcard
character than we want to pass to a program, but what if we wanted to
pass a string like ***?****[a-b]** with lots of characters that
would normally be interpreted as wildcards?  If we used backslashes to
escape them, we'd have to mark every single character.  A short string
would end up turning into: `\*\*\*\?\*\*\*\*\[a-b\]*\*`.
Instead of doubling our amount of typing, we can use a pair of single
quotes.

    $ ls '*?****[a-b]'
    *?****[a-b]

Any string encased in single quotes will not be modified by the shell,
even when it's filled with wildcards.  However, you cannot type a
single quote within the single quotes like this:

    $ ls '*?***'*[a-b]'

So, if you happen to have a file whose name has some single quotes in it
like this:

    *?*'**'*[a-b]

There is no other way but to escape the single quotes individually like
this:

    $ ls '*?*'\''**'\''*[a-b]'

Simply to say, replace any occurence of a single quote in the filename
with '\'.
