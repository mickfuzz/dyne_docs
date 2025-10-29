# Interactive Editing

Many people, especially beginners, use the arrow keys to move the cursor
around the command line. Most people can do that in much less time than
it would take them to remember the more powerful but more complicated
alternatives that are provided. But some of these methods are worth
learning, so we will present them here.

The shell comes with two different sets of *key bindings* (keyboard
shortcuts), inspired by two extremely powerful text editors, Emacs and
vi (probably the two most powerful ones that exist). By exploiting the
keyboard shortcuts that these bindings offer, command line wizards are
able to enter and edit even long command lines in just a fraction of a
second. If you take the time to practice with the key bindings that the
shell offers, even if they may seem impractical at first, you will very
soon be able to do so too.

**Note:** You will only be able to take full advantage of the Emacs and
vi bindings if you know how to type properly (using 10 fingers). If you
don\'t, you should learn it as soon as possible. (There are a lot of
free sites on the web that can teach you.) It is definitely worth
it.There is an application called Klavaro you can use it to learn typing
.\

By default, the bash shell uses the Emacs bindings. If you want to try
out the vi bindings, enter the following command:

    $ set -o vi

You can switch back to the Emacs bindings by entering:

    $ set -o emacs

The Emacs and vi bindings are very different, and both take some time to
get used to. You should try out both bindings to find the ones that
suits you best. This chapter covers the default, Emacs bindings. If you
learn vi, you can switch to those bindings and you will find them pretty
intuitive.\

**Hint:** Do not try to learn all shortcuts at once! The human brain
isn\'t made for that kind of stuff, so you will forget almost all of
them. Rather, we advise you to learn the 4-5 shortcuts that you find
most useful and use them regularly \-- learning by doing. Later, you can
come back to this chapter to pick up more shortcuts. You will soon find
yourself whirling across the command line.

## The Emacs bindings

The Emacs bindings make heavy use of **Ctrl** and **Alt** as modifier
keys. Experienced Emacs users usually remap their **CapsLock** key as
**Ctrl** in order to enter Emacs commands more comfortably (and to avoid
repetitive strain injury!). Once you start using the Emacs bindings on a
regular basis, we advise you to do the same.

**\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\***

This space reserved for instruction on remapping the CapsLock key

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\

### Moving around

The two most basic keystrokes for moving around on the command line in
Emacs mode are **Ctrl + f** and **Ctrl + b**. They move the cursor one
character to the right and to the left, respectively:

  ----------- ------------------------------
   Ctrl + f    Move forward one character
   Ctrl + b    Move backward one character
  ----------- ------------------------------

Of course, you can do the same cursor movements by using the arrow keys
on your keyboard. But as was remarked above, using the Emacs bindings
**Ctrl + f** and **Ctrl + b** may be more efficient, since your hands do
not have to leave the letter block of your keyboard. At the moment, you
may not notice the difference in speed (especially if you\'re not a fast
typist yet), but once you get more experience in using the command line,
you definitely won\'t want to touch the arrow keys again! (IMHO)\

The following table lists some keystrokes which let you navigate the
command line even faster:\

  ----------------------------------- -----------------------------------
   Alt + f                             Move forward one word

   Alt + b                             Move backward one word

   Ctrl + a                            Move to the beginning of the line

   Ctrl + e\                           Move to the end of the line
  ----------------------------------- -----------------------------------

**Hint:** The German word for \"beginning\" is Anfang. Would you ever
forget such a strange word? Let\'s hope not, because it can help you
remember that **Ctrl + a** takes you to the beginning of the command
line.

By taking advantage of the keystrokes summarized in the table above, you
can dramatically speed up your command line editing. If, for example,
you have misspelled the first letter of a terribly long filename, the
keystroke **Alt + b** brings the cursor back to the beginning of the
word \-- making cumbersome characterwise movement of the cursor
unnecessary. The **home** and **end** keys, if present, provide an
alternative to **Ctrl + a** and **Ctrl + e**.\

### Editing text

Two of the most commonly used editing commands are the following:

  ----------------------------------- -----------------------------------
   Ctrl + t\                           Transpose the character before the
                                      cursor and the character
                                      under/following the cursor\

   Alt + t                             Transpose the word before the
                                      cursor and the word under/following
                                      the cursor
  ----------------------------------- -----------------------------------

The two commands take a while to get used to, but both are very useful.
While the main use of **Ctrl + t** is to correct typos, **Alt + t** is
often used to \"drag\" a word forward on the command line. Have a look
at the following command line (the underline marks the position of the
cursor):

    $ echo one two three four

If you press **Alt + t** in this situation, the word before the cursor
(\"one\") is exchanged with the word after the cursor (\"two\"). Try it
out! The result should look like this:

    $ echo two one three four

You will notice two things. First, the order of the words \"one\" and
\"two\" has been reversed. Second, the cursor has moved forward along
with the word \"one\". Now, the cool thing about the cursor\'s moving
along is that you just need to press **Alt + t** once more in order to
transpose \"one\" with the following word, \"three\":

    $ echo two three one four

So, by pressing **Alt + t** repeatedly, you can \"drag\" forward the
word before the cursor until it has reached the end of the command line.
(Of course, you can do the same with a single character by using
**Ctrl + t**.)

At first, the elaborate functionality of the two transpose commands may
seem a bit confusing. Just play around with them for a while, and you
will soon get the hang of it.\

### Deleting/killing and reinserting text

Here are some handy commands for deleting/killing text:

  ----------------------------------- -----------------------------------
   Ctrl + d\                           Delete the character under the
                                      cursor\

   Alt + d\                            Kill all text from the cursor to
                                      the end of the current word

   Alt + Backspace\                    Kill all text from the cursor to
                                      the beginning of the current word
  ----------------------------------- -----------------------------------

Note that **Alt + d** and **Alt + Backspace** do not delete text, but
kill it. Killing is different from deleting in that deleted text is
gone, but killed text may be brought back to life (the term is
\"yanked\") later on by using the following command:

  ----------------------------------- -----------------------------------
   Ctrl + y\                           Reinsert (yank) text that was
                                      previously killed\

  ----------------------------------- -----------------------------------

Let\'s see how this works by way of an example:

    $ echo one two

Again, the cursor position is indicated by an underline. If you press
**Alt + Backspace** in this situation, the word \"two\" as well as the
whitespace after it will be killed, leaving the command line like this:

    $ echo one

If you now press **Ctrl + y**, the killed text is \"yanked\" back into
the command line. You can do this several times. If you press **Ctrl +
y** three times, for example, you end up with the following line:\

    $ echo one two two two

As you can see, killing text is much like the \"cut\" function of most
modern text editors. Note that text which is not killed, but deleted (by
pressing **Ctrl + d**) cannot be reinserted into the command line. The
only way to get it back is to use the undo function, which will be
introduced below.\

Probably the most useful commands for killing text are the following
ones:\

  ----------------------------------- -----------------------------------
   Ctrl + k\                           Kill all text from the cursor to
                                      the end of the line\

   Ctrl + u\                           Kill all text from the cursor to
                                      the beginning of the line
                                      (unix-discard-line)\
  ----------------------------------- -----------------------------------

As usual, the best way to learn these commands is to experiment with
them. You will find that killing and, where necessary, reinserting big
stretches of text can save you a lot of time.\

### Undoing changes

You can undo the last change that you made by using the following
command:

  ----------------------------------- -----------------------------------
   Ctrl + \_\                          Undo last change\

  ----------------------------------- -----------------------------------

An alternative way of doing the same thing is to press **Ctrl + xu**.
(Press **x** and **u** in turn while holding down **Ctrl**.)

### Navigating the shell\'s history

The shell saves the last commands that you enter in its history. This
allows you to get back to previously entered commands, which can save
you a lot of typing. Here are the most important commands for navigating
the shell\'s command history:

  ----------------------------------- -----------------------------------
   Ctrl + p\                           Go to the previous command in the
                                      history\

   Ctrl + n\                           Go to the next command in the
                                      history

   Ctrl + \>\                          Go to the end of the history\

   Ctrl + r\                           Search the history for previously
                                      entered commands
                                      (reverse-search-history)

   Ctrl + g\                           Cancel the current history search\
  ----------------------------------- -----------------------------------

Let\'s see how these commands work by way of a simple example. Open a
shell and enter the following commands:

    $ echo two
    two
    $ echo three
    three
    $ echo four
    four

After you have entered these commands, you are left with an empty
command line waiting for your input. Now, press **Ctrl + p**. You will
notice that the previously entered command appears on your command line:
`echo four`. If you press **Ctrl + p** once more, you move \"up\" in the
history even further, so that `echo three` appears on the command line.
Now, press **Ctrl + n**, and you will see that you have come back to
`echo four`: **Ctrl + n** works exactly like **Ctrl + p**, but the other
way round. The **up** and **down** arrow keys are alternatives to
these.\

After having pressed **Ctrl + p** and maybe also **Ctrl + n** a few
times, you may want to get back to the command line that you were
entering before you started navigating the history. You can do this by
pressing **Ctrl + \>**.\

As you can see, the shell\'s history is nothing else but a big list of
all recently entered commands. You can move up and down the list by
pressing **Ctrl + p** and **Ctrl + n**, respectively. And you can press
the **Enter** key at any time in order to execute the currently selected
command line.

Since the shell\'s command history is just a big list, it is also
searchable. This is most commonly done by using the command **Ctrl +
R**. Again, let\'s assume that you have entered the commands `echo two`,
`echo three` and `echo four`. Try pressing **Ctrl + R** now. You will
notice that a new prompt appears which says something like
\"reverse-i-search\". If you now enter the letter \"t\", you immediately
jump back in history to the last command line containing \"t\", which is
of course **echo three**. From there, you can use **Ctrl + p** and
**Ctrl + n** to navigate the history as explained above. Or you can
modify the search by entering a second letter, let\'s say \"w\". You
then jump to the command **echo two**, because it is the nearest command
in history containing the letter sequence \"tw\". Or you can just cancel
the search by pressing **Ctrl + g**.

If you feel a bit lost in using the shell\'s history functions, don\'t
worry! If you keep on practicing, you will quickly get into the routine
of flipping back and forth in the shell\'s history, avoiding the
cumbersome retyping of long command lines.

### Interactive editing: an example

The following example is intended to show you how the interactive
editing capabilities of the shell can drastically speed up your work.
Let\'s suppose you have entered the following command line:

    $ echoo ne two three
    bash: echoo: command not found

Bash has thrown an error, because the command *echoo* doesn\'t exist.
What you really meant, of course, was *echo one two three*. You will
perhaps be surprised to hear that it takes just five keystrokes to
correct the mistake:

1.  Press **Ctrl + p** to get the previous history item back on screen,
    namely the wrongly entered command line.
2.  Press **Ctrl + a** to move the cursor to the beginning of the line.
3.  Press **Alt + f** to move the cursor forward by one word. The cursor
    is now located between the wrongly entered words \"echoo\" and
    \"ne\".
4.  Press **Ctrl + t**. You will see that the \"o\" preceding the cursor
    and the whitespace under the cursor have been transposed: \"echoo
    ne\" has become \"echo one\".
5.  Finally, press **Enter** to execute the corrected command line.
:::


