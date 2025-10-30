# Piping hot commands

Pipes let programs work together by connecting the output from one to be
the input for another. The term "output" has a precise meaning here:
it is what the program writes to the standard output, via C program
statements such as printf or the equivalent, and normally it appears on
the terminal screen. And "input" is the standard input, usually coming
from the keyboard. Pipes are built using a vertical bar ("|") as the
pipe symbol.

Say you help your eccentric Aunt Hortense manage her private book
collection. You have a file named *books* containing a list of her
holdings, one per line, in the format "author:title", something like
this:

    $ cat books
    Carroll, Lewis:Through the Looking-Glass
    Shakespeare, William:Hamlet
    Bartlett, John:Familiar Quotations
    Mill, John Stuart:On Nature
    London, Jack:John Barleycorn
    Bunyan, John:Pilgrim's Progress, The
    Defoe, Daniel:Robinson Crusoe
    Mill, John Stuart:System of Logic, A
    Milton, John:Paradise Lost
    Johnson, Samuel:Lives of the Poets
    Shakespeare, William:Julius Caesar
    Mill, John Stuart:On Liberty
    Bunyan, John:Saved by Grace

This is somewhat untidy, as they are in no particular order. But we can
use the `sort` command to straighten that out:

    $ sort books
    Bartlett, John:Familiar Quotations
    Bunyan, John:Pilgrim's Progress, The
    Bunyan, John:Saved by Grace
    Carroll, Lewis:Through the Looking-Glass
    Defoe, Daniel:Robinson Crusoe
    Johnson, Samuel:Lives of the Poets
    London, Jack:John Barleycorn
    Mill, John Stuart:On Liberty
    Mill, John Stuart:On Nature
    Mill, John Stuart:System of Logic, A
    Milton, John:Paradise Lost
    Shakespeare, William:Hamlet
    Shakespeare, William:Julius Caesar

Ah, now you have a list nicely sorted by author. How about getting a
list just of authors, without titles? You can do that with the `cut`
command:

    $ cut -d: -f1 books
    Carroll, Lewis
    Shakespeare, William
    Bartlett, John
    Mill, John Stuart
    London, Jack
    Bunyan, John
    Defoe, Daniel
    Mill, John Stuart
    Milton, John
    Johnson, Samuel
    Shakespeare, William
    Mill, John Stuart
    Bunyan, John

A little explanation here. The `-d` option chose a colon as the
delimiter (separator). This tells `cut` to break up each line wherever a
delimiter appears, and each separate part of the line is called a field.
In our format, the author's name appears as the first field, so we have
put a 1 with the `-f` option to tell `cut` that we want to see just that
field.

But you'll notice the list is unsorted again. Pipelines to the rescue!

    $ sort books | cut -d: -f1
    Bartlett, John
    Bunyan, John
    Bunyan, John
    Carroll, Lewis
    Defoe, Daniel
    Johnson, Samuel
    London, Jack
    Mill, John Stuart
    Mill, John Stuart
    Mill, John Stuart
    Milton, John
    Shakespeare, William
    Shakespeare, William

Voila! You've taken the alphabetized list, which is the output of the
`sort` command, and fed it as input to the `cut` command. Don't give
the `cut` command a filename to use, because you want it to operate on
the text that's piped out of the `sort` command.

Pipes are just that simple--text flows down the pipe from one command
to the next.

How about if you wanted a sorted list of titles instead? Since the title
is the second field, let's try using `-f2` with the `cut` command
instead of `-f1`:

    $ sort books | cut -d: -f2
    Familiar Quotations
    Pilgrim's Progress, The
    Saved by Grace
    Through the Looking-Glass
    Robinson Crusoe
    Lives of the Poets
    John Barleycorn
    On Liberty
    On Nature
    System of Logic, A
    Paradise Lost
    Hamlet
    Julius Caesar

Oops. What happened? When looking at a pipeline, you need to go
left-to-right. In this case, we sorted the file first before extracting
the titles. So it dutifully sorted the lines starting with the author at
the beginning of each line. To get the *titles* in the proper order, you
need to do the sort *after* extracting them:

    $ cut -d: -f2 books | sort
    Familiar Quotations
    Hamlet
    John Barleycorn
    Julius Caesar
    Lives of the Poets
    On Liberty
    On Nature
    Paradise Lost
    Pilgrim's Progress, The
    Robinson Crusoe
    Saved by Grace
    System of Logic, A
    Through the Looking-Glass

Much better. Now this is all very nice, but you may be thinking you
could have done these things with a spreadsheet. For simpler tasks, this
is probably true. But suppose that Aunt Hortense is in the habit of
asking odd questions about her collection. For example, she wants to
know how many books she has from each author named John. A spreadsheet
or other graphical program may have difficulty handling a request that
wasn't anticipated by the program's authors. But the shell offers us
many small, simple commands that can be combined in unforeseen ways to
accomplish a complex task.

To find a particular string in a line of text, use the `grep` command.
Now remember that when you combine commands, they need to go in the
proper order. You can't run `grep` against the file first, because it
will match the title "John Barleycorn" in addition to authors named
John. So add it to the end of the pipeline:

    $ cut -d: -f1 books | sort | grep "John"
    Bartlett, John
    Bunyan, John
    Bunyan, John
    Johnson, Samuel
    Mill, John Stuart
    Mill, John Stuart
    Mill, John Stuart
    Milton, John

This gets us close, but you don't want to get "Samuel Johnson" on the
list and make Aunt Hortense angry. Often when working with `grep` you
will need to refine the matching text to get exactly what you need.
`grep` happens to offer a `-w` option that will let it match "John"
only when "John" is a complete word, not when it's part of
"Johnson". But we'll solve this particular dilemma by adding a comma
and space on the front of the string to match, so it will match only
when John is a first name:

    $ cut -d: -f1 books | sort | grep ", John"
    Bartlett, John
    Bunyan, John
    Bunyan, John
    Mill, John Stuart
    Mill, John Stuart
    Mill, John Stuart
    Milton, John

Ah, that's better. Now you just need to total up the number of books
for each author. A little command called `uniq` will work nicely. It
removes duplicate lines (duplicates must be on consecutive lines, so be
sure your text is sorted first), and when used with the `-c` option also
provides a count:

    $ cut -d: -f1 books | sort | grep ", John" | uniq -c
          1 Bartlett, John
          2 Bunyan, John
          3 Mill, John Stuart
          1 Milton, John

And there you are! A nicely sorted list of Johns and the number of books
from each. For our example set this is a simple job, one you could even
do with pencil and paper. But this very same pipeline can be used to
process far more data -- it won't blink even if Aunt Hortense has
hundreds of thousands of books stored in the barn.

System administrators often use pipelines like these to deal with log
files generated by web and mail servers. Such files can grow to tens or
hundreds of megabytes in size, and a command pipeline can be a quick way
to generate summary statistics without trying to read through the entire
log.

A nice thing about building pipelines is that you can do it one command
at a time, seeing exactly what effect each one has on the output. This
can help you discover when you might need to tweak options or rearrange
the order of commands. For instance, to put the authors in ranking
order, you can just add a `sort -nr` to the previous pipeline:

    $ cut -d: -f1 books | sort | grep ", John" | uniq -c | sort -nr
          3 Mill, John Stuart
          2 Bunyan, John
          1 Milton, John
          1 Bartlett, John

Experiment!
