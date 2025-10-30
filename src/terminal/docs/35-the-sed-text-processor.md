# The Sed Text Processor

Sed (stream editor) is a utility that does transformations on a
line-by-line basis. The commands you give it are run on each line of
input in turn. It is useful both for processing files and in a pipe to
process output from other programs, such as here:

    $ wc -c * | sort -n | sed ...

## Basic Syntax and Substitution

A common use of Sed is to change words within a file. You may have used
"Find and Replace" in GUI based editors. Sed can do this much more
powerfully and faster:

     $ sed "s/foo/bar/g" inputfile > outputfile

Let's break down this simple command. First we tell the shell to run
`sed`. The processing we want to do is enclosed in double quotation
marks; we'll come back to that in a moment. We then tell Sed the name
of the *inputfile* and use standard shell redirection (>) to the name
of our *outputfile*. You can specify multiple input files if you want;
Sed processes them in order and creates a single stream of output from
them.

The expression looks complex but is very simple once you learn to take
it apart. The initial "s" means "substitute". This is followed by
the text you want to find and the replacement text, with slashes (/) as
separators. Thus, here we want to find "foo" in the inputfile and put
"bar" in its places. Only the output file is affected; Sed never
changes its input files.

Finally, the trailing "g" stands for "global", meaning to do this
for the whole line. If you leave off the "g" and "foo" appears twice
on the same line, only the first "foo" is changed to "bar".

    $ cat testfile
    this has foo then bar then foo then bar
    this has bar then foo then bar then foo
    $ sed "s/foo/bar/g" testfile > testchangedfile
    $ cat testchangedfile
    this has bar then bar then bar then bar
    this has bar then bar then bar then bar

Now let's try that again without the `/g` on the command and see what
happens.

    $ cat testfile
    this has foo then bar then foo then bar
    this has bar then foo then bar then foo
    $ sed "s/foo/bar/" testfile > testchangedfile
    $ cat testchangedfile
    this has bar then bar then foo then bar
    this has bar then bar then bar then foo

Notice that without the "g", Sed performed the substitution only the
first time it finds a match on each line.

This is all well and good, but what if you wanted to change the second
occurrence of the word foo in our testfile? To specify a particular
occurrence to change, just specify the number after the substitute
commands.

    $ sed "s/foo/bar/2" inputfile > outputfile

You can also combine this with the `g` flag (in some versions of Sed) to
leave the first occurrence alone and change from the 2nd occurrence to
the end of the line.

    $ sed "s/foo/bar/2g" inputfile > outputfile

## Sed Expressions Explained

Sed understands regular expressions, to which a chapter is devoted in
this book. Here are some of the special characters you can use to match
what you wish to substitute.

    $ matches the end of a line
    ^ matches the start of a line
    * matches zero or more occurrences of the previous character
    [ ] any characters within the brackets will be matched

For example, you could change any instance of the words "cat",
"can", and "car" to "dog" by using the following:

     $ sed "s/ca[tnr]/dog/g" inputfile > outputfile

In the next example, the first [0-9] ensures that at least one digit
must be present to be matched. The second [0-9] may be missing or may
be present any number of times, because it is followed by the *
metacharacter. Finally, the digits are removed because there is nothing
between the second and third slashes where you can put your replacement
text.

     $ sed "s/[0-9][0-9]*//g" inputfile > outputfile

Inside an expression, if the first character is a caret (^), Sed
matches only if the text is at the start of the line.

    $ echo dogs cats and dogs | sed "s/^dogs/doggy/"
    doggy cats and dogs

A dollar sign at the end of a pattern expression tells Sed to match the
text only if it is at the end of the line.

    $ echo dogs cats and cats | sed "s/cats$/kitty/"
    dogs cats and kitty

A line changes only if the matching string is where you require it to
be; if the same text occurs elsewhere in the sentence it is not be
modified.

## Deletion

The "d" command deletes an entire line that contains a matching
pattern. Unlike the "s" (substitute) command, the "d" goes after the
pattern.

    $ cat testfile
    line with a cat
    line with a dog
    line with another cat
    $ sed "/cat/d" testfile > newtestfile
    $ cat newtestfile
    line with a dog

The regular expression ^$ means "match a line that has nothing
between the beginning and the end", in other words, a blank line. So
you can remove all blank lines using the "d" command with that regular
expression:

    $ sed "/^$/d" inputfile > outputfile

## Controlling Printing

Suppose you want to print certain lines and suppress the rest. That is,
instead of specifying which lines to delete using "d", you want
specify which lines to keep.

This can be done with two features:

Specify the `-n` option, which means "do not print lines by default".

End the pattern with "p" to print the line matched by the pattern.

We'll show this with a file that contains names:

    $ cat testfile
    Mr. Jones
    Mrs. Jones
    Mrs. Lee                                                                        Mr. Lee

We've decided to standardize on "Ms" for women, so we want to change
"Mrs." to "Ms". The pattern is:

     s/Mrs./Ms/

and to print only the lines we changed, enter:

    $ sed -n "s/Mrs./Ms/p" testfile

## Multiple Patterns

Sed can be passed more than one operation at a time. We can do this by
specifying each pattern after an `-e` option.

    $ echo Gnus eat grass | sed -e "s/Gnus/Penguins/" -e "s/grass/fish/"
    Penguins eat fish.

## Controlling Edits With Patterns

We can also be more specific about which lines a pattern gets applied
to.  By supplying a pattern before the operation, you restrict the
operation to lines that have that pattern.

    $ cat testfile
    one: number
    two: number
    three: number
    four: number
    one: number
    three: number
    two: number
    $ sed "/one/ s/number/1/" testfile > testchangedfile
    $ cat testchangedfile
    one 1
    two: number
    three: number
    four: number
    one: 1
    three: number
    two: number

The `sed` command in that example had two patterns. The first pattern,
"one", simply controls which lines Sed changes. The second pattern
replaces "number" with "1" on those lines.

This works with multiple patterns as well.

    $ cat testfile
    one: number
    two: number
    three: number
    four: number
    one: number
    three: number
    two: number
    $ sed -e "/one/ s/number/1/" -e "/two/ s/number/2/" \
          -e "/three/ s/number/3/" -e "/four/ s/number/4/" \
          < testfile > testchangedfile
    $ cat testchangedfile
    one: 1
    two: 2
    three: 3
    four: 4
    one: 1
    three: 3
    two: 2

## Controlling Edits With Line Numbers

Instead of specifying patterns that can operate on any line, we can
specify an exact line or range of lines to edit.

    $ cat testfile
    even number
    odd number
    odd number
    even number
    $ sed "2,3 s/number/1/" < testfile > testchangedfile
    $ cat testchangedfile
    even number
    odd 1
    odd 1
    even number

The comma acts as the range separator, telling Sed to work only on lines
two through three.

    $ cat testfile
    even number
    odd number
    odd number
    $ sed -e "2,3 s/number/1/" -e "1 s/number/2/" < testfile > testchangedfile
    $ cat testchangedfile
    even 2
    odd 1
    odd 1

Sometimes you might not know exactly how long a file is, but you want to
go from a specified line to the end of the file. You could use `wc` or
the like and count the total lines, but you can also use a dollar sign
($) to represent the last line:

    $ sed "25,$ s/number/1/" < testfile > testchangedfile

The $ in an address range is Sed's way of specifying, "all the way to
the end of the file".

## Scripting SED commands

By using the `-f` argument to the `sed` command, you can feed Sed a list
of commands to run. For example, if you put the following patterns in a
file called *sedcommands*:

    s/foo/bar/g
    s/dog/cat/g
    s/tree/house/g
    s/little/big/g

You can use this on a single file by entering the following:

    $ sed -f sedcommands < inputfile > outputfile

Note that each command in the file must be on a separate line.

There is much more to Sed than can be written in this chapter. In fact,
whole books have been written about Sed, and there are many excellent
tutorials about Sed online.
