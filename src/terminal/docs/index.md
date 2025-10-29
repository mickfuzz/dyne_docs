# Getting Started

Modern computing is highly interactive, and using the command line is
just another form of interaction.  Most people use the computer through
its desktop or graphical interface, interacting at a rapid pace.  They
click on an object, drag and drop it, double-click another to open it,
alter it, etc.

Although interactions happen so fast you don't think about it, each
click or keystroke is a command to the computer, which it reacts to.
Using the command line is the same thing, but more deliberate.  You type
a command and press the **Return** or **Enter** key.  For instance, in
my terminal I type:

    date

And the computer replies with:

    Thu Mar 12 17:15:09 EDT 2009

That's pretty computerish.  In later chapters we'll explain how to
request the date and time in a more congenial format. We'll also
explain how working in different countries and with different languages
changes the output.  The idea is that you've just had an interaction.

## The Command Line Can Do Much Better

The *date* command, as seen so far, compares poorly with the alternative
of glancing at a calendar or clock.  The main problem is not the
unappetizing appearance of the output, mentioned already, but the
inability to do anything of value with the output.  For instance, if
I'm looking at the date in order to insert it into a document I'm
writing or update an event on my online calendar, I have to do some
retyping.  The command line can do much better.

After you learn basic commands and some nifty ways to save yourself
time, you'll find out more in this book about feeding the output of
commands into other commands, automating activities, and saving commands
for later use.

## What Do We Mean By a Command?

At the beginning of this chapter we used the word "command" very
generally to refer to any way of telling the computer what to do.  But
in the context of this book, a command has a very specific meaning.
It's a file on your computer that can be executed, or in some cases an
action that is built into the shell program. Except for the *built-in
commands*, the computer runs each command by finding the file that bears
its name and executing that file. We'll give you more details as they
become useful.

## Ways to Enter Commands

To follow along on this book, you need to open a command-line
interpreter (called a *shell* or *terminal* in GNU/Linux) on your
computer.  Pre-graphical computer screens presented people with this
interpreter as soon as they logged in.  Nowadays almost everybody except
professional system administrators uses a graphical interface, although
the pre-graphical one is still easier and quicker to use for many
purposes.   So we'll show you how to pull up a shell.

## Finding a Terminal

You can get a terminal interface from the desktop, but it may be easier
to leave the desktop and use the original text-only terminal. To do
that, use the `<ctrl><alt><F1>` key combination. You get a nearly
blank screen with an invitation to log in. Give it your username and
password. You can go to other terminals with `<alt><F2>` and so on,
and set up sessions with different (or the same) users for whatever
tasks you want to do. At any time, switch from one to another by using
the `<alt><F#>` keystroke for the one you want. One of these, probably
F7 or F8, will get you back to the desktop. In the text terminals you
can use the mouse (assuming your system has gpm running) to select a
word, line or range of lines.  You can then paste that somewhere else in
that terminal or any other terminal.

GNU/Linux distributions come with different graphical user interfaces
(*GUI* ) offering different aesthetics and semantic metaphors.  Those running
on top of the operating system are known as *desktop environments*.
GNOME, KDE and Xfce are among the most widely used ones.  Virtually
every desktop environment provides a program that mimics the old
text-only terminals that computers used to offer as interfaces.  On your
desktop, try looking through the menus of applications for a program
called Terminal.  Often it's on a menu named something such as
"Accessories", which is not really fair because once you read this
book you'll be spending a lot of time in the terminal every day.

In GNOME you choose `Applications -> Accessories -> Terminal`.

![Screenshot_1.png](images/IntroCommandLine-GettingStarted-Screenshot_1-en.png "ubuntu_open")

In KDE you choose `K Menu -> System -> Terminal`; in Xfce you choose
`Xfce Menu -> System -> Terminal`.
Wherever it's located, you can almost certainly find a terminal
program.

When you run the terminal program, it just shows a blank window;
there's not much in the way of help.  You're expected to know what to
do--and we'll show you.

The following figure shows the Terminal window opened on the desktop in
GNOME.

 ![Screenshot_2.png](images/CommandLineIntro-GettingStarted-Screenshot_2-en.png)

## Running an Individual Command

Many graphical interfaces also provide a small dialog box called
something like "Run command".  It presents a small text area where you
can type in a command and press the **Return** or **Enter** key.

![Screenshot_Run_Application.png](images/IntroCommandLine-GettingStarted-Screenshot_Run_Application-en.png "run"){height="187"
width="520"}

To invoke this dialog box, try typing the **Alt** + **F2** key
combination, or searching through the menus of applications.  You can
use this box as a shortcut to quickly start up a terminal program, as
long as you know the name of a terminal program installed on your
computer.  If you are working on an unfamiliar computer and don't even
know the name of the default terminal program, try typing `xterm` to
start up a no-frills terminal program (no fancy menus allowing choice of
color themes or fonts).  If you desperately need these fancy menus,

-   in GNOME the default terminal program should be `gnome-terminal`;
-   in KDE it should be `konsole`;
-   in Xfce you'd try with `Terminal` or with version specific terminal
    names: for example in Xfce 4 you should find `xfce4-terminal`.

## How We Show Commands and Output in This Book

There's a common convention in books about the command-line. When you
start up a terminal, you see a little message indicating that the
terminal is ready to accept your command. This message is called a
*prompt*, and it may be as simple as:

    $

After you type your command and press the **Return** or **Enter** key,
the terminal displays the command's output (if there is any) followed
by another prompt. So my earlier interaction would be shown in the book
like this:

    $ date
    Thu Mar 12 17:15:09 EDT 2009
    $

You have to know how to interpret examples like the preceding one. All
you type here is *date*. Then press the **Return** key. The word *date*
in the example is printed in bold to indicate that it's something you
type. The rest is output on the terminal.
:::
