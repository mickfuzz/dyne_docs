#  Kedit, KWrite, & Kate

Although Kedit is part of the KDE software suite, which includes the KDE
desktop environment, it does not require KDE to run. It is just as happy
under Gnome. KDE has several built-in tools to help you edit text files
(including scripts). The simplest of these is \"KEdit\", a basic text
editor. You can start it from the KDE menu or from the command line, if
you prefer. For example, you could run:

    $ kedit /etc/profile &

You should see something like this.

![kedit1_4_1](images/IntroCommandLine-Kedit-kedit1_4_1-en.png "kedit1_4_1"){width="594"
height="408"}

Using it is simple. You can move around the file with the arrow keys,
Page Up (**PgUp**) and Page Down (**PgDn**) keys, or the mouse. Opening
a new file is done from the **File-\>Open** menu, and you can
spell-check your file with the **Tools-\>Spelling\...** menu option.

At the bottom of the window is some useful information (if you don\'t
see it, the **Settings-\>Show Statusbar** menu option brings it up). The
line and column display shows the current position of the cursor. The
\"INS\" means that you are in *insert mode*, and that if there is text
to the right of the cursor, it is pushed over as you type. The opposite
of this is \"OVR\", which stands for *overtype mode*, where text to the
right is replaced by the newly typed text. You can switch between these
with the **Insert** key on the keyboard.

If you make any changes to the current file, then \"\[modified\]\"
appears in the title bar to remind you that you need to save changes
before exiting.

![How KEdit indicates a file has been
changed](images/IntroCommandLine-Kedit-kedit2-en.png){width="300"
height="30"}

## KWrite

While KEdit is useful, it is quite limited. KDE offers other options
that are worth investigating. KWrite is very similar-looking, but offers
useful additional features.

![Screenshot_profile\_\_\_KWrite.png](images/IntroCommandLine-Kedit-Screenshot_profile___KWrite-en.png){width="598"
height="650"}

The most obvious advantage is *syntax highlighting*. For shell scripts,
programs, and many other types of files, KWrite colors the text to make
it easier to figure out what is going on. Here, comments are shown in
gray, parameters are in green, built-in bash commands are shown in dark
purple, and other shell commands in light purple. Generally KWrite tries
to pick highlighting based on what it guesses the type of file to be. If
it guesses wrongly, or doesn\'t do highlighting at all, you can manually
choose an option under **Tools-\>Highlighting**.

You might have also noticed the little minus symbols and lines in the
left margin. This is part of what is known as *code folding*. KWrite
tries to match up \"if\" statements with the corresponding \"fi\",
\"for\" with \"done\", and so on. Clicking on the minus symbol collapses
the block, which can be useful if you are reading through a script and
are interested in viewing what comes before and after a block, but not
what\'s inside it. There are many more helpful features that KWrite
offers\--take a look around the menu and see what things do!

## Kate

Last up in this overview is Kate. It is essentially the same editor as
KWrite. However, it offers additional tools that make working on a
project, as opposed to a single file, easier.

![Screenshot_profile\_\_\_Kate.png](images/IntroCommandLine-Kedit-Screenshot_profile___Kate-en.png "kate1_1"){width="604"
height="526"}

On the left side of the window are tabs that let you view the documents
open in the current Kate session (KEdit and KWrite open multiple files
in separate windows, while Kate can open them all in one), or navigate
through your computer\'s filesystem to open a file. Kate also does
syntax highlighting like KWrite, but also adds a \"Terminal\" tab at the
bottom. Clicking on this tab opens and closes a mini-terminal where you
can enter commands. In this case, if we wanted to see what \"id -u\" in
the script does, and can simply type it in to the terminal to try it
out.

For KDE users, KEdit, KWrite, and Kate offer three nice choices for
editing text files. Chances are that all three came pre-installed on any
system with KDE. Have fun trying them out!
:::


