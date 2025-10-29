# Scripting

If you have a collection of commands you\'d like to run together, you
can combine them in a script and run them all at once. You can also pass
arguments to the script so that it can operate on different files or
other input.\

Like an actor reading a movie script, the computer runs each command in
your shell script, without waiting for you to whisper the next line in
its ear. A script is a handy way to:

-   Save yourself typing on a group of commands you often run together.\
-   Remember complicated commands, so you don\'t have to look up, or
    risk forgetting, the particular syntax each time you use it.
-   Use control structures, like loops and case statements, to allow
    your scripts to do complex jobs. Writing these structures into a
    script can make them more convenient to type and easier to read.

Let\'s say you often have collections of images (say, from a digital
camera) that you would like to make thumbnails of. Instead of opening
hundreds of images in your image editor, you choose to do the job
quickly from the command line. And because you may need to do this same
job in the future, you might write a script. This way, the job of making
thumbnails takes you only two commands:

    $ cd images/digital_camera/vacation_pictures_March_2009
    $ make_thumbnails.sh

The second command, `make_thumbnails.sh`, is the script that does the
job. You have made it previously and it resides in a directory on your
search path. It might look something like this:\

    #!/bin/bash
    # This makes a directory containing thumnails of all the jpegs in the current dir.
    mkdir thumbnails
    cp *.jpg thumbnails
    cd thumbnails
    mogrify -resize 400x300 *.jpg

If the first line begins with #! (pronounced \"shee-bang\"), it tells
the kernel what interpreter is to be used. (Since bash is usually the
default, you might omit this line). After this you should put in some
comments about what the script is for and how to use it . Scripts
without clear and complete usage instructions often do not \"work
right\". For bash, comments start with the hash (#) character and may be
on the ends of executable lines.\

The file includes commands conforming to the syntax of the interpreter.
We\'ve seen three of them before: `mkdir`, `cp`, and `cd`. The last
command, `mogrify`, is a program that can resize images (and do a lot of
other things besides). Read its manual page to learn more about it.\

## Making scripts executable

To write a script like the one we\'ve shown, open your favorite text
editor and type in the commands you would like to run. For bash, you can
put multiple commands on a single line so long as you put a semi-colon
after each command so the shell knows a new command is starting.\

Save the script. One common convention for bash is to use the `.sh`
extension \-- for example, *make_thumbnails.sh*.

There is one more step before you can run the script: it has to be
*executable*. Remember from the section on permissions that
executability is one of the permissions a file can have, so you can make
your script executable by granting the execute (**x**) permission. The
following command allows any user to execute the script:

    $ chmod +x make_thumbnails.sh

Because you\'re probably planning to use the script often, you\'ll find
it worthwhile to check your PATH and add the script to one of the
directories in it (for instance, */home/jdoe/bin* is an easy choice
given the PATH shown here).\

    $ echo $PATH
    /usr/bin:/usr/local/bin:/home/jdoe/bin

For simple testing, if you\'re in the directory that contains the
script, you can run it like this:

    $ ./make_thumbnails.sh

Why do you need the preceding ./ path? Because most users don\'t have
the current directory in their PATH environment variables. You can add
it, but some users consider that a security risk.

Finally, you can also execute a script, even without its execute bit
set, by passing it as an argument to the command interpreter, thusly:

    $ bash make_thumbnails.sh

## More control

To provide the flexibility you want, the bash shell and many other
interpreters let you make choices in a script and run things repeatedly
on a variety of inputs. In that regard, the shell is actually a
programming language, and a nice way to get used to using the powerful
features a programming language provides. We\'ll get you started here
and show you the kinds of control the bash shell provides through
compound statements.\

### if

This statement was already introduced in the section on checking for
errors, but we\'ll review it here. `if` is more or less what you\'d
expect, though its syntax is quite a bit different from its use in most
other languages. It follows this form:

    if [ test-condition ]
    then
      do-something
    else
      do-something-else
    fi

You read that right: the block must be terminated with the keyword `fi`.
 (It\'s one of the things that makes using `if` fun.) The `else` portion
is optional. Make sure to leave spaces around the opening and closing
brackets; otherwise `if` reports a syntax error.\

For example, if you need to check to see if you can read a file, you
could write a chunk like this:

    if [ -r /home/joe/secretdata.txt ]
    then
        echo "You can read the file"
    else
        echo "You can't read that file!"
    fi

`if` accepts a wide variety of tests. You can put any set of commands as
the *test-condition*, but most `if` statements use the tests provided by
the square bracket syntax. These are actually just a synonym for a
command named `test`. So the first line of the preceding example could
just as well have been written as follows.

    if test -r /home/joe/secretdata.txt

You can find out more about tests such as `-r` in the manual page for
`test`. All the test operators can be used with square brackets, as we
have.

Some useful `test` operators are:\
\

  ---- --------------------------------
  -r   File is readable
  -x   File is executable
  -e   File exists
  -d   File exists and is a directory
  ---- --------------------------------

There are many, many more of them, and you can even test for multiple
conditions at once. See the the manual page for `test`.

### while (and until)

`while` is a loop control structure. It keeps cycling through until its
test condition is no longer true. It takes the following form:

    while test-condition
    do
      step1
      step2
      ...
    done

You can also create loops that run until they are interrupted by the
user. For example, this is one way (though not necessarily the best one)
to look at who is logged into your system once every 30 seconds:\

    while true
    do
        who
        sleep 30
    done

This is inelegant because the user has to press **Ctrl + c** or kill it
in some other way. You can write a loop that ends when it encounters a
condition by using the `break` command. For instance the following
script uses the `read` command (quite useful in interactive scripts) to
read a line of input from the user. We store the input in a variable
named *userinput* and check it in the next line. The script uses another
compound command we\'ve already seen, `if`, within the `while` block,
which allows us to decide whether to finish the `while` block. The
`break` command ends the `while` block and continues with the rest of
the script (not shown here). Notice that we use two tests through `-o`,
which means \"or\". The user can enter **Q** in either lowercase or
uppercase to quit.\

    while true
    do
      echo "Enter input to process (enter Q to quit)"
      read userinput

      if [ $userinput == "q" -o $userinput == "Q" ]
      then
        break
      fi

      process input...

    done

`until` works exactly the same way, except that the loop runs until the
test condition becomes true.\

### case

`case` is a way for a script to respond to a set of test conditions. It
works similarly to case statements in other programming languages,
though it has its own peculiar syntax, which is best illustrated through
an example.

    user=`whoami` # puts the username of the user executing the script
                  # into the $user variable.
    case $user in
        joe)
            echo "Hello Joe. I know you'd like to know what time it is, so I'll show you below."
            date
            ;;
        amy)
            echo "Good day, Amy. Here's your todo list."
            cat /home/amy/amy-todo.txt
            ;;
        sam|tex)
            echo "Hi fella. Don't forget to watch the system load. The current system load is:"
            uptime
            ;;
        *)
            echo "Welcome, whoever else you are. Get to work now, please."
            ;;
    esac

Each case must be followed by the ) character, then a newline, then the
list of steps to take, then a double semicolon (;;). The \"\*)\"
condition is a catchall, similar to the `default` keyword in some
languages\' `case` structures. If no other cases match, the shell
executes this list of statements. Finally, the keyword `esac` ends the
`case` statement. In the example shown, note the case that matches
either the string \"sam\" or \"tex\".

### for

`for` is a useful way of iterating through items in a list. It can be
any list of strings, but it\'s particularly useful for iterating through
a file list. The following example iterates through all of the files in
the directory *myfiles* and creates a backup file for each one. (It
would choke on any directories, but let\'s keep the example simple and
not test for whether the file is a directory.) ⁞\

    for filename in myfiles/*
    do
        cp $filename $filename.bak
    done

As with any command that sets a variable, the first line of the `for`
block sets the variable called *filename* without a dollar sign.

There\'s another variety of `for`, which is similar to the `for`
construct used in other languages, but which is used less in shell
scripting than it\'s used in other languages, partially because the
syntax for incrementing and decrementing variables in the shell is not
entirely straightforward.

### parallel

`parallel` is a useful way of iterating through items in a list while
maximizing the use of your computer by running jobs in parallel. The
following example iterates through all of the files in the directory
*myfiles* and creates a backup file for each one replacing the extension
with `.bak`. (It would choke on any directories, but let\'s keep the
example simple and not test for whether the file is a directory.)\

    ls myfiles/* | parallel cp {} {.}.bak

`parallel` can often be used instead of `for` loops and `while read`
loops, make these run faster by running them in parallel and make the
code easier to read.

`parallel` can be used for a lot of more advanced features. You can
watch an intro video here:
[http://www.youtube.com/watch?v=OpaiGYxkSuQ](https://web.archive.org/web/20160417194719/http://www.youtube.com/watch?v=OpaiGYxkSuQ){target="_top"}

\
:::


