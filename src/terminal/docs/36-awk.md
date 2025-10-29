# Awk

AWK is a programming language designed for processing plain text data.
It is named after its founders, Alfred **A**ho, Peter **W**einberger and
Brian **K**ernighan.  AWK is quite a small language and easy to learn,
making it the ideal tool for quick and easy text processing.  Its prime
use is to extract data from table-like input.\

Since programs written in AWK tend to be rather small, they are mostly
entered directly on the command line.  Of course, saving larger scripts
as text files is also possible.

In the next paragraphs, we present the basics of AWK through three
simple examples.  All of them will be run on the following text file
(containing the five highest scores ever achieved in the video game
Donkey Kong as of March 2009):\

    1050200 Billy Mitchell 2007
    1049100 Steve Wiebe 2007
    895400 Scott Kessler 2008
    879200 Timothy Sczerby 2001
    801700 Stephen Boyer 2007

The file is a table organized into *fields*.  The first field of each
row contains the respective score, the second and third fields contain
the name of the person who has achieved it, and the fourth and last
field of each row contains the year in which the score was set.  You
should copy and paste the text above into a text file and name it
something like *highscores.txt* so that you can try out the following
examples.

## Example 1

Let\'s say we want to print only those scores higher than 1,000,000
points. Also, we want only the first names of the persons who have
achieved the scores.  By using AWK, it\'s easy to extract this
information:

    $ awk '$1 > 1000000 { print $2, $1 }' highscores.txt
    Billy 1050200
    Steve 1049100

Try it out!

The little AWK program that we\'ve just entered on the command line
consists of two parts:

1.  The part preceding the curly braces (*\$1 \> 1000000*) says "Do
    this for all lines where the value of field no. 1 is greater than
    1,000,000."
2.  The part inside the curly braces (*print \$2, \$1*) says "Print
    field no. 2, followed by field no. 1."\

What the combined program says is: "For all lines, if the value of the
first field is greater than 1,000,000, print the second field of the
line followed by the first field of the line."  (Note that AWK programs
entered on the command line are usually enclosed in single quotation
marks in order to prevent the shell from interpreting them.)

As we have seen in the previous example, the structure of an AWK
statement is as follows:

    pattern { action }

The expression *pattern* specifies a condition that has to be met for
*action* to take effect. AWK programs consist of an arbitrary number of
these statements.  (The program we have discussed above contains only a
single statement.)  An AWK program basically does the following:

1.  It reads its input (e.g. a file or a text stream from standard
    input) line by line.
2.  For each line, AWK carries out all statements whose
    condition/pattern is met.

Simple, isn\'t it?

## Example 2

Let\'s look at another example:

    $ awk '$4 == 2007 { print "Rank", NR, "-", $3 }' highscores.txt
    Rank 1 - Mitchell
    Rank 2 - Wiebe
    Rank 5 - Boyer

The program, again consisting of a single statement, may be paraphrased
like this: "For each line, if the value of field no. 4 equals 2007,
print the word \'Rank\', followed by the value of the variable \'NR\',
followed by a dash (\'-\'), followed by field no. 3."

So what this little program does is print the surnames of all high score
holders having set their record in 2007 along with their respective
ranks in the high score table.

How does AWK know which ranks the individual high score holders occupy?
Since the table is sorted, the rank of each high score holder is equal
to the row number of the entry.  And AWK can access the number of each
row by means of the built-in variable NR (**N**umber of **R**ow).  AWK
has quite a lot of useful built-in variables, which you can look up in
its documentation.\

## Example 3

The third and final example is a bit more complex than the other two,
since it contains three AWK statements in total:

    $ awk 'BEGIN {print "Together, the five best Donkey Kong players have achieved:"}\
    {total += $1} END {print total, "points"}' highscores.txt

This will output the following:\

    Together, the five best Donkey Kong players have achieved:
    4675600 points

Let\'s break up this program into its three parts/statements (which we
have entered on a single command line):

### First statement

*pattern*: BEGIN\
*action*: print "Together, the five best Donkey Kong players have
achieved:"

### Second statement

*pattern:* none (= always execute *action*)\
*action*: add the value of field no. 1 to the variable *total*

### Third statement

*pattern:* END\
*action*: print the value of the variable *total*, followed by the
string "points"

OK, now let\'s look at what is new in this short AWK program.

First of all, the patterns BEGIN and END have a special meaning: the
action following BEGIN is executed before any input is read and the
action introduced by END is executed when AWK has finished reading the
input.

In the second statement, we can observe that an AWK statement does not
need a pattern, only *action* is obligatory.  If a statement doesn\'t
contain a pattern, the condition of the statement is always met and AWK
executes the action for every single input line.

Finally, we have used our own variable for the first time, which we have
called *total*. AWK variables do not need to be declared explicitly; you
can introduce new ones by simply using them.  In our example program,
the value of the variable *total*, starting out at 0 (zero), is
increased by the value of field no. 1 for each input line. The +=
operator means "add the math expression on the right to the variable on
the left."

So after all input lines have been read, *total* contains the sum of all
field 1 values, that is, the sum of all high scores.  The END statement
outputs the value of *total* followed by the string "points".\

## Where to go from here?

We have seen that AWK is a fun and easy to use little programming
language that may be applied to a wide range of data extraction tasks.
This short introduction to AWK can of course be little more than an
appetizer.  If you want to learn more, we recommend you have a look at
GAWK, the GNU implementation of AWK.  It is one of the most feature-rich
implementations of the language, and comes with a comprehensive and easy
to read manual (see
[http://www.gnu.org/software/gawk/manual/](https://web.archive.org/web/20160417194719/http://www.gnu.org/software/gawk/manual/)).\
:::


