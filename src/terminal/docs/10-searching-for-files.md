# Searching for Files

When you first get a computer, you tend to place files in just a couple
folders or directories. But as your list of files grows, you have to
create some subdirectories and spread the files around in order to keep
your sanity. Eventually, you forget where files are. "Where did I store
those photos I took in Normandy?"

You could run `ls -R`, as in the following section, and start running
your finger down the screen, but why? Computers are supposed to be about
automation. Let the computer figure out where the file is.

If you know your file is named "somefile", telling the computer what
to do is pretty easy.

    $ find . -name somefile -print
    ./files/somefile

The `find` command takes more arguments than the other commands we've
seen so far, but if you use it for a while you'll find it becomes
natural.  Its first argument (the '.') tells find where to start
looking: the directory at the top of everything you're searching
through. In this case, we're telling `find` to start looking in
whatever directory we're in right now.

The `-name` argument tells it to look for a file named *somefile*.
Finally, the `-print` option tells the command to print out on our
screen the location of any file that matches the name it was given.

## Wildcards with Find

What if you don't remember the name of the file you're looking for?
You might only remember that it starts with "some".  Luckily, `find`
can handle that too.

    $ find . -name 'some*' -print
    ./dir1/subdir2/files/somefile_other
    ./some_other_file
    ./files/somefile

This time it found a few more files than you were after but it still
found the one you wanted.  As you can see, the `find` command can
process wildcards in much the same way the shell can.  Here you asked it
to look for anything that starts with the letters "some".

The "*", "?", and "[ ]" wildcards can all be used just as they
would be in the shell.  However, since `find` is using the wildcards you
have to make sure they remain unaltered by the shell.  To do this you
can surround the name you're searching for, and the wildcards it
contains, in single quotes.

## Trimming The Search Path

With just a name and a location, find will begin searching through every
directory below its starting point, looking for matches.  Depending on
how many subdirectories you have where you're searching, `find` can
take a lot of time to look in places you know don't contain the file.

It is possible, however, to control how far `find` sinks in the
directory tree.

    $ find . -maxdepth 1 -name 'some*' -print
    ./some_other_file

By using the `-maxdepth` argument we can tell `find` to go no lower than
the number of directories we specify.  A maxdepth of 1 says: don't
leave the starting directory. A maxdepth of 3 would allow `find` to
descend 3 directories from where it started, and so on.  It's important
to note that `-maxdepth` should immediately follow the start location,
or find will complain.

## Using Criteria

The `find` command can search for files based on any criteria the
filesystem know about files. For instance, you can search for files
based on:

-   When they were last modified or accessed (somebody read them)
-   How big they are
-   Who owns them, or what group they are in
-   What permissions (read, write, execute) they have

```{=html}
<!-- -->
```
-   What type of file (directory, regular file) they are

and other criteria described in the manual page. Here we'll just show a
couple popular options.

The `-mtime` option shows the latest modification time. Suppose you just
can't remember anything about a file's name, but know that you created
or modified it within the past three days. You can find all the files in
your home directory that were created or modified within the past three
days through:

    $ find ~ -mtime -3 -print

Notice the minus sign before the 3, for "less than." If you know you
created the file yesterday (between 24 and 48 hours ago), you can search
for an exact day:

    $ find ~ -mtime 1 -print

To find files that are more than 30 days old (caution: there will be a
lot of these), use a plus sign:

    $ find ~ -mtime +30 -print

Perhaps you want to remove old files that are large, before backing up a
directory. Combine `-mtime` with `-size` to find these files. The file
has to match all the criteria you specify in order to be printed.

    $ find directory_to_backup  -mtime +30  -size +500k  -print

We've specified +500k as our `-size` option. The plus sign means
"greater than" and "500k" means "500 kilobytes in size".

## Using Find To Run a Command on Multiple Files

The find command can do much more powerful things than print filenames.
You can combine it with any other command you want, so that you can
remove files, move them around, look for text in them, and so on. On
those occasions, the `find` command with its `-exec` option is just what
you'll need.

Because the next example is long, it is divided onto two lines, with a
backslash at the end of the first so the shell keeps reading and keeps
the two lines as one command. The first line is the same as the command
to find old, large files in the previous section.

    $ find directory_to_backup  -mtime +30  -size +500k -print
                  -exec rm {} ;

The `-exec` option is followed by an `rm` command, but there are two odd
items after it:

-   `{}` is a special convention in the `-exec` option that means "the
    current file that was found"
-   `;` is necessary to tell find what the end of the command is. A
    command can have any number of arguments. Think of `-exec` and `;`
    as surrounding the command you want to execute.

So we find each file, print the name through `-print` (which we don't
have to do, but we're curious to see what's being removed), and then
remove it in the `-exec` option.

Clearly, a tiny mistake in a find command could lead to major losses of
data when used with `-exec`. Test your commands on throw-away files
first!

Using `cp` you can see how the bracket pairs can be specified multiple
times, allowing the file's name to be easily duplicated.

    $ find . -name 'file*' -exec cp {} {}.backup ;

Experiment and practice!
