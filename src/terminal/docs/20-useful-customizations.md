# Useful customizations

You can really make the shell your own, adapting every facet to the way
you work (and even the different ways you work from week to week). In
this section we'll look at quick changes you can make. Scripting, a way
to extend and combine the functions offered by the shell, will be
introduced later.

## Variables

Each command-line shell has the concept of a *variable*.  Variables
consist of two parts: the variable  *name* and the variable *value*.  If
I were to say "x=6", "x" is the name of the variable, and "6" is
the value.  To see the value of a variable, one puts a dollar sign in
front of the variable name.  Here is a very simple example.


    $ x=6
    $ echo $x
    6
    $

Above, the first line *assigns* the value 6 to the variable x and the
second line asks the shell to display the value of x. Note that we put
the dollar sign in front of the variable name when we want to see its
value, but we [never]{.underline} use the dollar sign when assigning the
value.

So anything starting with a dollar sign ($) is interpreted by the shell
as a variable. One variable sneaked into an earlier section on exit
status: you saw that **$?** contains the exit status of the previous
command.

Now, what kind of useful things can we do with variables? A common use
is to save typing. Say that the files for the project you're working on
all week are located in a directory called
*/home/jsmith/projects/foo/confoobulator*.
*/home/jsmith/projects/foo/confoobulator* is a lot to type, but you can
save typing by assigning the value to a variable.

    $ p=/home/jsmith/projects/foo/confoobulator

Now you can change to my project directory by typing

    $ cd $p

You can remove the value of a variable by setting it to an empty string:

    $ VAR=""

or by issuing the `unset` builtin command:

    $ unset VAR

## Ordinary Variables and Environment Variables

Most shells (including the GNU bash shell) recognize two kinds of
variables: *ordinary variables and environment variables*.  An ordinary
variable is available to your shell, but not to any programs that your
shell runs.  On the other hand, an environment variable is available to
both your shell and all of the commands it runs.  One can turn an
ordinary variable into an environment variable by using the `export`
command.  If I were to type

    $ export p

The (ordinary) variable **p** becomes an environment variable, and can
be used by any command that my shell runs.

## Shell Variables

The shell provides a lot of its own variables. For instance, the output
of the `whoami` command (which was shown near the beginning of the book)
is the same as $USER. Your home directory is stored in $HOME. You can
see any variable's value by echoing it:

    $ echo $HOME

The first dollar sign shown in that example is just a prompt; it has
nothing to do with variables.

You can see the shell's built-in variables (actually a subset known as
*environment* variables) through:

    $ env
    SHELL=/bin/bash
    USER=jsmith
    PATH=/usr/local/bin:/usr/bin:/bin:/usr/games
    PWD=/home/jsmith
    HOME=/home/smith
    _=/usr/bin/env
    ...

Your output will look different, but many of the variable names will be
the same. You will find some of these useful in later work.

-   SHELL is the path to your login shell.
-   USER is your username.  When you logged into your GNU/Linux system,
    this is the username you typed in.
-   PATH is a list of directories, separated by colons.  When you run a
    command (like `cat` or `ls`), your shell looks in these directories
    to find the executable program.  We'll talk more about PATH in just
    a moment.
-   PWD is your current working directory (that is, the folder you are
    in).
-   HOME is your home directory.  You start out in this directory when
    you first log in.
-   `_` is the last executed command. In this case, /usr/bin/env.

## Controlling Variable Expansion

If you jam a variable up against other characters, the shell won't
recognize it. For instance, the following won't work:

    $ curr=myfile
    $ rm $curr1.jpeg
    rm: .jpeg: No such file or directory

The error message could easily be perplexing. Here's what has happened:
the shell saw a variable named ****$curr1****. When it couldn't find
any such variable, it substituted an empty string. So you ended up
trying to execute:

    $ rm .jpeg

If you want to remove *myfile1.jpeg*, use curly braces around the
variable so the shell knows where the variable name ends:

    $ rm ${curr}1.jpeg

## The Search Path

We've looked at several examples of running commands.  If I type "ls
-l" on the command line, then my shell runs the `ls` command, which
makes a list of files.  The `ls` command is actually a program sitting
on your computer's hard drive.  You can ask your shell where a command
lives by using the `which` command.   If I type

    $ which ls

then my shell responds with "/bin/ls", which tells me the `ls` command
is a program that lives in the */bin* directory of my hard drive.   We
can even use the `ls` command to look at itself

    $ ls -l /bin/ls
    -rwxr-xr-x 1 root root 92672 2007-01-30 15:48 /bin/ls

My shell found the `ls` command by using the PATH environment variable.

    PATH=/usr/local/bin:/usr/bin:/bin:/usr/games

The value of PATH is a list of directories, separated with colons. When
I typed `ls`, my shell looked for the command in */usr/local/bin/ls*,
then */usr/bin/ls*, and finally */bin/ls*.  */bin/ls* is where the
command lives, so my shell was able to run that.  If there wasn't a
*/bin/ls*, then my shell would have tried */usr/games/ls*, and then
given up.

## Configuration Files

You may have seen a lot of nice customizations in the book--or even
better, thought up a few customizations of your own--and may be ready
to save some of them so you can reuse them in every terminal session.
Anything you define in the shell is lost when you close the terminal
window. So this is a good time to look at configuration files, which
save useful customizations between sessions.

Your home directory contains several hidden files that contain settings
for the shell and other programs. In addition, there are entire hidden
directories where programs store information, such as the colors you
chose to put on your desktop.

How are these directories hidden? Through a simple convention: any file
that begins with a dot (.) is considered hidden. Your file manager in
your desktop won't show you the files unless you choose a special
option to display hidden files. Similarly, the shell doesn't display
them by default in an `ls` command. To display them in the shell, add
the `-a` (for "all") option:

    $ ls -a
    .
    ..
    .bash_history
    .bash_logout
    .bashrc
    .irssi
    .profile
    foo
    examplefile

In the previous listing (which will look different on your system) the
*.bashrc* and *.profile* files are what we're particularly interested
in. These are where you can put your customizations. It doesn't matter
much which one you choose. The *.bashrc* file is particular to a type of
shell (there are many types) called Bash, whereas *.profile* is read by
other shells in case you decide to use something besides Bash.

Bash configuration works in a very simple manner: Bash just executes the
commands when it starts up, exactly as if you typed them in before you
did anything else. So anything you see in this section that you
like--an alias, a function, a change to an environment variable,
etc.--you can put in a configuration file. Entire scripts can be
included.

Your startup files likely have commands in them already. Some are
installed along with the operating system, while others are added by
system administrators at workplaces. To change these customizations or
add your own, check out the section on text editors in this book. Pick
one editor and learn a dozen or so of its basic commands so you can do
the minimal editing needed to put in your customizations.

## Functions

You can combine a number of commands and give it a name; then you can
use this name like any other command. Consider writing a function
whenever you find yourself executing the same commands repeatedly. You
can also write flexible functions that change their behavior based on
arguments, just as other commands do.

As a simple example, suppose you want to save information in a file each
day:

    echo ENTRY -------------- >>~/save/log
    date >>~/save/log
    du -c >>~/save/log
    ls -R >>~/save/log
    echo >>~/save/log

To save your commands as a function, issue a command named `function`
followed by the name you want to assign it, and the commands in curly
braces. Note that we've used hash marks (#) to add some comments so we
will remember what the function is for later. The shell ignores the hash
mark and any text that follows on that line.

    function savelog {
    # Add information about this directory a log file, ~/save/log
      echo ENTRY -------------- >>~/save/log
      date >>~/save/log
      du -c >>~/save/log  # Size of subdirectories
      ls -R >>~/save/log  # Complete file listing
      echo >>~/save/log
    }

Now you can issue the command `savelog` and execute the embedded
commands. You can put the function definition in a startup file so you
never have to type the definition in again.

The previous example was quite contrived because you very rarely issue
the exact same commands in sequence. However, you often have a
complicated command that you run on different files, or other objects.

For instance, here is a command that shows you the differences between
the current version of a file and the most recently edited version, if
you edit with Emacs. Emacs saves an old version of your file by creating
another file with the same name but an added tilde (~). In this
example, we view the differences between *txtfile* and the back-up
*txtfile~* version:

    $ diff txtfile~ txtfile | less

This is just complicated enough (and common enough) to be worth saving
as a function. But you want to pass the filename as an argument so you
can use the function on any file you edit. So specify the argument as
$1, a special variable that the function understands:

    function d~ {
    # Compare the Emacs back-up version with the current version.
      diff -u $1~ $1 | less
     }

 Now you can run your new `d~` command on any file that has a backup:

    $ d~ txtfile

As you might guess, a function can take up to nine arguments, which you
can refer to as $1, $2, up to and including $9. If you want more than
nine arguments, you can save an argument and remove it from the list:

    function manyargs {
      $arg=$1
      shift
      ...
    }

The first thing this function does is save the first argument in its own
**$arg** variable. The `shift` command removes the $1 argument and
shifts all the other arguments over, so that the second argument is now
$1. In the section on scripting, you'll see how to use loops to
process arguments or other items one by one.

If you want to pass all the arguments to a command, use $*. For
instance, the following `orth` function runs the spell utility on
whatever string you pass:

    function orth () {
      echo $* | spell
    }

Functions can contain compound statements, such as if/then blocks. To
show how flexible and powerful the combination of functions and compound
statements can be, we'll include here an if/then statement that was
shown earlier in the section "Handling command failure".

    function helpme() {
      if man $1
        then echo "you now know more about $1"
        else apropos $1
      fi
    }

So the following:

    $ helpme draw

 will now be equivalent to:

      if man draw
        then echo "you now know more about draw"
        else apropos draw
      fi

As long as you can guess what errors or other conditions will occur, you
can handle them automatically in a function.

## Sourcing in files

If this chapter has gotten you excited about the possibilities of
writing up your customizations and saving them in files, good. But you
will eventually have lots of different functions that fall into various
categories, and you'll find it confusing to keep them all in one file.
At this point, you can start storing commands, variable settings, and
functions in various files that meet different needs, and read them into
your *.bashrc* file or any other script. Just use a dot to read a file
and have its contents executed by the shell:

    .  scriptfile

It's important to put a space after the dot, before the filename.

## Setting prompts

Whenever bash or any other shell is waiting for the user to type a
command, it displays a prompt, which can be as simple or complex as you
like. A minimal prompt would be

    $

The default prompt looks something like

     user@host:~$

where user is the login name, host is the name of the computer, ~ is
the working directory, short for the user's home, typically in the form
*/home/user*, and $ means that the current user is not root.

To change the prompt, give a new value to the environment variable PS1.
To make the change permanent, put the assignment in your *.profile*
file, which bash reads whenever it starts up. The default value is
\\u@\\h:\\w\\\$, specifying username, host, working directory, and
decorator characters. The following table describes the fields that can
appear in a prompt, and various other useful characters. The prompt can
ring the terminal "bell", now more usually a beep; it can contain
multiple lines using \\r for Carriage Return; and it can contain
embedded terminal control sequences, typically starting with the Escape
character. We will not attempt to explain all of these options here. See
*Bash Reference Manual*, by Brian Fox and Chet Ramey, for full details.
[http://www.gnu.org/software/bash/manual/](https://web.archive.org/web/20160417194719/http://www.gnu.org/software/bash/manual/)\

  ------ ------------------------------------------------------------------------------------ ------- -----------------------------------------------------------------------------------------------------------------------
   \\a   an ASCII bell character (07)                                                           \\d   the date in "Weekday Month Date" format (e.g., "Tue May 26")
   \\\]  end a sequence of non-printing characters                                              \\e   an ASCII escape character (033)
   \\h   the hostname up to the first '.'                                                     \\H   the hostname
   \\j   the number of jobs currently managed by the shell                                      \\l   the basename of the shell's terminal device name
   \\n   newline                                                                                \\r   carriage return
   \\s   the name of the shell, the basename of \$0 (the portion following the final slash)     \\t   the current time in 24-hour HH:MM:SS format
   \\T   the current time in 12-hour HH:MM:SS format                                            \\@   the current time in 12-hour am/pm format
   \\A   the current time in 24-hour HH:MM format                                               \\u   the username of the current user
   \\v   the version of bash (e.g., 2.00)                                                       \\V   the release of bash, version + patchelvel (e.g., 2.00.0)
   \\w   the current working directory                                                          \\W   the basename of the current working directory
   \\!   the history number of this command                                                     \\#   the command number of this command
   \\\$  if the effective UID is 0 (root), a #, otherwise a \$                                 \\nnn  the character corresponding to the octal number nnn
   \\\\  a backslash                                                                           \\\[   begin a sequence of non-printing characters, which could be used to embed a terminal control sequence into the prompt
  ------ ------------------------------------------------------------------------------------ ------- -----------------------------------------------------------------------------------------------------------------------

Example:

    $ PS1="\a\d, \e[31m\t\r\n\e[0m\u@\h:\w $"

would result in a sound from the computer, and the visible prompt

    Mon Mar 23, 13:47:43
    user@host:~ $

with the time printed in red. This uses \\d for the date, \\e\[31m to
turn on red color, \\t for the time, \\e\[0m to turn off red, \\r\\n for
Carriage Return and New Line, and the rest as in the default.\

To make things more interesting, you can run a program within the prompt
by enclosing it in \[\\\$( )\]. This example counts the number of files
in the current directory, by counting the lines (`wc -l`) piped in from
a directory listing (`ls`).

    $ PS1="\u@\h [\$(ls | wc -l)]:\$ "
    user@host [3]:$

## Superuser Privileges

Besides the configuration files in each user's directory, the system
has a lot of configuration files that control system-wide behavior.
Sometimes you'll find it necessary to edit one by hand, using a text
editor. In this section we'll show how to grant someone superuser
privileges, a system-wide issue controlled by a file named
*/etc/sudoers*.

It is best not to edit this file in an ordinary text editor. The
`sudoedit` command provides a much safer way to edit configuration
files.

    $ sudoedit /etc/sudoers

This makes a temporary copy of the file and opens the copy in an editor.
You can override the default editor by setting the VISUAL or EDITOR
environment variable to "vi", "emacs", or whatever you like.

Permission lines in */etc/sudoers* identify the user, followed by the
hosts the user can use `sudo` on, which groups the user can act as a
member of, and which commands the user can execute using `sudo`.\

An operator in a corporate or school system might have permissions that
look like this.

    operator       ALL = DUMPS, KILL, SHUTDOWN, HALT, REBOOT, PRINTING,\
                           sudoedit /etc/printcap, /usr/oper/bin/

(The `\` character continues the permissions on the next line.) This
gives permission to run a specific set of commands, and to edit two
specific configuration files, but no others. To give someone permission
to run any superuser command using `sudo`, set the username's
permission line to:

    username ALL = (ALL) ALL

This also lets you edit any configuration file on your computer.

## Localization

Different countries use different conventions for all sorts of things:
character sets, currencies, the formats of dates and times, and even
paper size and shape. Computers can be instructed which language to use,
and which version of the language to use for a particular country. This
combination of customized information is called the *locale*.\

All of the locale settings are reported by the `locale` command. For
example,

    $ locale
    LANG=en_US.UTF-8
    LC_CTYPE="en_US.UTF-8
    LC_NUMERIC="en_US.UTF-8
    LC_TIME="en_US.UTF-8"
    LC_COLLATE="en_US.UTF-8"
    LC_MONETARY="en_US.UTF-8"
    LC_MESSAGES="en_US.UTF-8"
    LC_PAPER="en_US.UTF-8"
    LC_NAME="en_US.UTF-8"
    LC_ADDRESS="en_US.UTF-8"
    LC_TELEPHONE="en_US.UTF-8"
    LC_MEASUREMENT="en_US.UTF-8"
    LC_IDENTIFICATION="en_US.UTF-8"
    LC_ALL=

The LANG setting **en_US.UTF-8** specifies English as the language, US
as the country,  and Unicode UTF-8 as the encoding. Money in the US is
in dollars, \$. Paper is letter, 8.5" × 11", as opposed to A4 for most
of the rest of the world.

You usually specify a language and country when you install your
operating system, and everything including the shell picks those values
up. Originally, it was supposed that language, country, and character
encoding would go together, but in our increasingly global society, it
can happen that a Hungarian temporarily in the US on UN business would
choose UTF-8, French language, metric (SI) measurements, Euros, Swiss
address and telephone formats (for the home office in Geneva), and US
letter paper.

You can change any of these settings in your shell by assigning an
appropriate string to the relevant environment variable. The accepted
values for locale settings are provided with options to the locale
command.

    $ locale -m # available charmaps: character set and encoding identifiers
    ANSI_X3.110-1983
    ANSI_X3.4-1968
    ARMSCII-8
    ASMO_449
    BIG5
    BIG5-HKSCS
    ...         # 226 choices in Ubuntu 8.10

    $ locale -a # available locales for English and UTF-8 in various countries
    C
    en_AU.utf8
    en_BW.utf8
    en_CA.utf8
    en_DK.utf8
    en_GB.utf8
    en_HK.utf8
    en_IE.utf8
    en_IN
    en_NG
    en_NZ.utf8
    en_PH.utf8
    en_SG.utf8
    en_US.utf8
    en_ZA.utf8
    en_ZW.utf8
    POSIX

You will get different locale specifications depending on the languages
and encodings selected on your system at installation time or modified
later.

To set your preferences, check for the correct format using these
commands, and set the locale environment values in your *.profile*
accordingly.

Another essential element of localization is your preferred keyboard
layout, set with the `loadkeys` command for the command line, and
`setxkbmap` for the X Window System (used on virtually all free
desktops).

    $ loadkeys de-latin1 # German

or

    $ setxkbmap dvorak # Dvorak keyboard for English

The `setfont` command lets you change to a font for a specific writing
system.

     $ setfont iso01.f16

This sets a bitmap font covering ISO 8859-1, suitable for many Western
European languages.

If you need to type documents in more than one writing system, you
probably need to move to X. But there are extended versions of Emacs and
vim that can create plain text files in multiple writing systems, either
in their own format or in Unicode.
