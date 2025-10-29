# All That Typing...

So, all this typing has got to stop being fun at some point.
Fortunately, the command line offers a number of ways to make your work
more efficient.

## Auto Completion

Every keyboard has a **Tab** key, and its a very useful thing to have in
the terminal. You might have used this keystroke before to indent words
in a word processor. You can still do this in GNU/Linux word processors,
but when you use **Tab** in the GNU/Linux terminal it becomes such a
time saver that when you master it you will be using it all the time.

Essentially, the **Tab** is an auto-complete command. If, for example, I
want to move the file 'dsjkdshdsdsjhds_ddsjw22.txt' somewhere with the
`mv` command I can either type out every letter of the stupid filename,
or I can type `mv` (for 'move') followed by the first few letters of
the filename and press **Tab**. The rest of the filename will be
automagically filled in. If the filename is not filled in it means that
there are several files (or directories) that start with those first few
letters I typed. To remedy this I could type a few more letters of the
filename and press **Tab** again, or to help me out I could press
**Tab** twice and it will give me a list of files that start with those
letters.

You can also use **Tab** to auto-complete command names.

**Tab** is your friend, use it a lot.

## Copy and Paste

Just because you are working on the command line doesn't mean you
can't use some of the conveniences you are used to in the GUI. While
cut and paste may work a little differently here from its behavior in
other operating systems, you'll soon find it very intuitive.

**Copying text** is as simple as highlighting the text you wish to copy
by holding down the left mouse button and highlighting the text as you
are probably already used to doing. Or, left-click 2 times to select a
word or 3 times to select a line.

**Pasting text** The highlighted text that you just copied is held in
the clipboard until you paste it where your cursor is by clicking the
middle (wheel) mouse button.

**Note :** if your mouse only has two buttons, pressing *both together*
will be recognized as a "middle button" press.

Anyway, it works like that in a non-graphical terminal. You may find
that it is not quite like this on the desktop. So it may be a good idea
to log in a text session. Use `<ctrl><alt><F1>` to get
out of the desktop.

Try it! Select the paragraph below with the left mouse button, open a
new virtual terminal, and paste the text with the middle mouse button.

     echo "This is pasted text."

After you see the text in the terminal, press the **Enter** key and the
`echo` command will repeat the text between the quotes on the command
line.

**Note :** If you are copying text from a web page, sometimes the
punctuation isn't handled properly. You might actually copy some unseen
formatting along with the text, which will break the syntax of the
command you are copying.

## History

It is also possible to use the up and down arrows on the keyboard to
navigate back and forwards through the history of the commands you have
typed. When you navigate to an earlier command this way, it is then just
necessary to press the **Return** or **Enter** key and the command will
be re-executed. You can edit it first to make it do something
different.
:::


