# Python

Python is a programming language that can be used to perform tasks that
would be difficult or cumbersome on the command line. Python is included
by default with most GNU/Linux distributions. Just like the command
line, you can either use Python by typing commands individually, or you
can create a script file. If you want to type commands individually,
start the Python interpreter by typing `python`.\

    $ python
    >>> 10 + 10
    20

To exit an interactive Python session, type **Ctrl + d**.

To write a multi-line script in Python that you can run from outside of
the Python interactive console, put the commands in a file. You can use
any text editor to create this file \-- Emacs, Vim, Gedit, or whatever
your favorite is; and call it what you like (often the filename ends
with \".py\" to help distinguish it in a directory listing). A script
could look like this:

    a = 1 + 2
    print a

In this example, we create a variable, *a*, which stores the result of
\"1 + 2\". It then uses the *print* command to print out the result,
which should be 3. If we save this file as *first.py*, we can run it
from the command line.

    $ python first.py
    3

The Python program printed out \"3\", just like we expected. We can add
a first line specifying the python interpreter, make it executable and
then just type *./first.py* to run it. If we still have the first.pl
file from the previous chapter, it does exactly the same thing and we
can make a link to one of them in order to choose the method by which we
add 1 and 2.

    $ ln -s first.py 1plus2
    $./1plus2
    3
    $ln -sf first.pl 1plus2
    $./1plus2
    3

Of course, we can use Python to do more useful things. For example, we
can look at all the files in the current directory.

    $ python
    >⁞⁞>> import os
    >>> os.listdir('.')
    ['notes.txt', 'readme.txt', 'first.py']

Here we import the standard library \"os\", which has operating
system-like functions in it.  We call the *listdir* function to return a
list of names of files in the current directory.  We pass the directory
name as a string (enclosed in single quotes); the single dot refers to
the current directory.\

Let\'s try doing something with these files \-- here\'s a way to find
all of the \".py\" files in a directory.

    >>> files = os.listdir('.')
    >>> files
    ['notes.txt', 'readme.txt', 'first.py']
    >>> [file for file in files if '.py' in file]
    ['first.py']

Above we use a powerful construction called a *list comprehension* to
produce a new list by transforming and filtering a list.  Below is a
simpler but wordier way to pick out the all of the files with \".txt\"
in them.

    >>> for file in files:
    ...     if '.txt' in file:
    ...         print file
    ...
    notes.txt
    readme.txt

The indentation is required in Python.  Indentation tells the Python
interpreter what to include in the *for* loop and what to include in the
*if* statement.  Also, a you must press an additional **Enter** at the
last set of three dots to tell the Python interpreter that you\'re done.

We can also use command line code in Python by passing it to the
*os.system* function. For example, if we wanted to delete all of the
\".txt\" files, we could use.

    >>> for file in files:
    ...     if '.txt' in file:
    ...         cmd = 'rm ' + file
    ...         os.system(cmd)
    ...

Above, we construct a shell command *cmd* as a Python string by
concatenating (using the \"+\" operator) the strings \"rm \" and the
filename, then pass it to the *os.system* function.  Now we can check to
see that the files have been deleted.\

    >>> os.system('ls')
    first.py

## More information about Python

The Python web site at
[http://www.python.org](https://web.archive.org/web/20160417194719/http://www.python.org/)
contains an impressive amount of information and documentation about the
Python language.  If you are just getting started with programming, the
book \"How to Think Like a Computer Scientist\" by Jeffrey Elkner, Allen
B. Downey and Chris Meyers at
[http://openbookproject.net/thinkCSpy/index.html](https://web.archive.org/web/20160417194719/http://openbookproject.net/thinkCSpy/index.html)
is a good place to start.
:::


