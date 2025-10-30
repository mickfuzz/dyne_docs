# Other scripting languages

The shell is a wonderful friend. If you have read the rest of the book
up to this point, you may well be dizzy with the possibilities it
presents. But the shell is still tremendously limited compared to many
languages. We'll give you just a taste of other tools and languages you
can explore.

Two classic tools, AWK and Sed, are commonly invoked from the shell.
Each operates on input one line at a time. You can think of them as
assembly lines on which workers load a file line by line. Each line is
processed in order. These are classic examples of *filters*, an idea
closely associated with GNU/Linux. Filters are strung together in pipes
with the | character, with the output of each command becoming the
input for the next.

The next section introduces regular expressions. They're a language all
their own, but one where you can do a lot by learning a few simple
features. They turn up in very similar forms all over: in text editors
such as vi, in commands such as `grep`, in AWK and Sed, and in all the
languages that follow.

Scripting languages were invented to make programming easy and allow
people to create applications quickly. Unlike AWK and Sed, they usually
run by themselves, not as part of pipes or other contexts where they
just produce a line of output for each line of input. In contrast to the
shell, these languages offer such advantages as:

-   Versatile integer and floating-point arithmetic
-   Objects, which help you keep data together with the functions that
    manipulate it
-   Complex data structures that can store related data of many types

This book has short sections on the three most popular scripting
languages in the free software world today: Perl, Python, and Ruby. You
will encounter many tools and products that provide customization
through those languages.
