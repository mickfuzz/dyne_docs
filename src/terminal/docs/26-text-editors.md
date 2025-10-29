# Text Editors

Besides running simple commands like `ls` and `grep`, you can use the
command line to start large, complex programs. Before graphical
interfaces were common, programs were designed to use plain text and
take up the screen. Now these programs run within the same window that
you use for the command line.

In this book we\'ll focus on text editors, because you need them to save
your commands for later use and to write scripts. They\'re handy for
lots of other things too; for instance, you may want to edit HTML files
on a web server using a text editor.

## Word Processing vs Text Editing

Almost everybody who uses a computer is familiar with a word processor.
The free software world provides several powerful ones, including
OpenOffice.org and KWrite. The text editors we show in this book
manipulate text, like word processors, but there\'s a fundamental
difference.

-   Word processors store a lot more than a stream of text characters
    you see on the screen. They want to provide \"rich text,\" with
    italic and bold, numbering and bullets, colors\--you name it. This
    is obviously valuable for many purposes. A plain text resume does
    not impress many employers.\
-   On the other hand, word processors are showing their limitations
    these days: some word processors have proprietary formats that make
    it hard to use documents in other programs. In fact, sometimes you
    cannot open a document with another version of the same word
    processor, or the document has display problems.
-   Many people find online collaboration tools (such as the wiki
    software we used to write this manual) and content management
    systems (such as many weblog sites use) easier for modern document
    production than a word processor. But word processors are also
    evolving to do a better job supporting collaboration; probably all
    these tools will merge over time and evolve into something better
    than any of them offered before.

Word processors, wikis, content management systems, and text editors all
have their place. The tasks in this book require a text editor. If you
want to use a word processor to edit these files, you can do so, but
make sure to choose a plain text form when you save the file.\

## Why do you need a text editor?

GNU/Linux is a very file-centric operating system: everything is (or
looks like) a file. All basic configuration is done via carefully
crafted text files, in the right place with the right contents. You can
find many graphical tools to configure your GNU/Linux box, but most of
them just tweak text files on your behalf.

Those text files have an exact syntax that you must follow. A simple
misplaced character could jeopardize your system, so using a word
processor for this matter is not only a bad idea but could corrupt your
files with extra formatting information. Configuration files don\'t need
italic or bold, they only need the right information.

With source code it\'s the same thing. Compilers (programs that turn
code into other programs) are very strict with syntax. Some of them even
care about where in the line a specific command is. Word processors mess
up the position of text in lines far too much for compiler to like them.
What you need is a clean view of what\'s in the source code or the
configuration file to know that what you\'re writing is exactly what
your system will get.\

Some editors go even further: they became Integrated Development
Environments (IDEs), that not only understand what you\'re typing (be it
an Apache configuration or Java code) but can predict what you want to
type, suggest modifications, or show your mistakes. They can color
specific keywords, automatically place things in the right place, and so
on.

But the most important is that all those colors and highlighting are
done *only* within the display. Those fancy changes are **not**
propagated to the text files, which are meant to be plain text. This is
one particular useful feature that word processing programs can\'t do
and is most essential to text editing.

## Why are most text editors command-line programs?

In the beginning\... was the command line (Neil Stephenson). Twenty
years ago there weren\'t many graphical interfaces around and Unix was
already a grown-up operating system running on a whole lot of very
important computers. All configuration was already stored in text files
because of the KISS principle (keep it short and simple). Unix made the
most of KISS and plain text by helping programs work together on text
files. Pipes (using the \| character) are one powerful method of working
together that you\'ve seen in this book.\

Nowadays, computers have thousands of times more power than those early
ones, but keeping configuration in text files still gives a big
advantage when the only connection you have to your server is through a
56-Kbit modem line and it\'s in a different country. Having to open a
graphical interface might not be possible and if that\'s the only way
you have to fix a problem, you\'re in big trouble.

Making graphical programs that deal with configuration was a big plus,
as the average user can now change things without reading tons of
documentation and isn\'t likely to break the system by inserting one
wrong character to a point where it\'s irrecoverable, but providing text
files and the command-line editor is fundamental to any operating
system.

Although most text editors came from the command-line world, most also
have a graphical interface today. Menus and buttons do help a lot when
using Gvim or Emacs. GEdit and Kate (which are purely graphical) are
short and simple, still providing the same basic functionality and the
same important features for text editing.

## Setting a default text editor

Because the terminal and command line are so tied in with the text
editors, many commands open up a text editor for you. We saw one example
`sudoedit`, in the \"Useful Customizations\" section. You can set the
default editor though by setting either the EDITOR or the VISUAL
environment variable. For instance:\

    $ export EDITOR=emacs

Put this in a startup file such as *./bashrc*, and commands will use
your chosen editor when they present a file for editing.\
:::


