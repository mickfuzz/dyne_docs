# GNU Screen

GNU screen helps you get most out of your desktop's real estate, in
cases where you need to work on more than one terminal simultaneously.
Using GNU screen, you can have as many processes as you need, such as
editors, web browsers and shells, all within a single terminal window.
Every desktop system allows you to open multiple terminals in different
windows, and most terminal programs let you run multiple sessions at
once using tabs, but GNU screen is often easier to manage and less
confusing when you have many sessions. Additionally, GNU screen offers a
copy-paste mechanism to transfer pieces of text easily within the
multiplicity of sessions handled by it. It can be used in an ordinary
text terminal as well as on the desktop, and for most purposes it is an
alternative to using multiple text terminal sessions and moving between
them by the **Alt + F#** key combinations.

Start by typing `screen` in your command prompt.

    $ screen

You get a welcoming message and some versioning information, or your
system may be configured so that it goes directly to a shell prompt.

![screen](images/IntroCommandLine-Screen-screen-en.png "screen"){width="507"
height="366"}

If you press **Enter** you get a shell prompt, just like before you
invoked screen. You are now running within a single session of screen.
In order to create a second session with its own shell, press **ctrl +
a** followed by **c** or **ctrl + c**.  All screen commands begin with
**ctrl + a** but if you need to use that for some other purpose you can
change that key binding. See the manual (type "man screen" at your
bash prompt) for this and many details we will not present here.

If you want to run just one command in a new screen and then close the
screen, specify the command as an argument:

    $ screen irssi

## Switching Sessions

To switch to the previous shell, enter **ctrl + a** followed by the
**p** key. To go forward again, enter **ctrl + a** followed by the **n**
key. You can see a list of all the sessions you have created using
**ctrl + a** followed by the **"** key. This presents a scroll-down
menu listing all open sessions (very helpful when you have created many
sessions!). If you want to save even the half second it takes to scroll
down the menu, while being able to see a list of available sessions and
instantly jump to another session, enter **ctrl + a** **w**. This keeps
you in your current window, but adds, at the bottom of the screen, a
list of sessions with a different number for each. Then enter **ctrl + a
*session_number*** to jump to the session of your choice.

Suppose you created a few sessions under GNU screen. In one session you
have a Vim editor open, a couple more sessions you use for logging in to
different remote servers, another session you use to run FTP, and so
forth. By default, **ctrl + a** **"** shows the program you used to
start each session. Normally you started it with the shell, so **ctrl +
a** **"** shows "bash" for each session (or whatever your shell is).
This isn't very helpful if you want to quickly find the session that's
running Vim or FTP.

It turns out that customizing the display is easy. While you are in a
session--say, editing in vim--press **ctrl + a** A and you get a line
at the bottom of your window with the name of your session, which you
can edit to your liking.

## Copy & Paste

You can use the mouse (on systems running gpm) to select text in one
session and paste it in any session or even another terminal. The screen
program has its own method for copying text, like this:

1.  Press **ctrl + a** **[** to go into *screen copying* mode.
2.  Navigate through the text anywhere in your window, using arrow keys
    or editor commands or whatever works in that session. At one end of
    the region of interest, press the **spacebar**.
3.  Navigate to the end of the region of interest (text is highlighted
    as you proceed with your selection).
4.  Press the **spacebar** again to copy the text to the clipboard.
5.  Switch to the session where you want to copy the text, navigate to
    the right place and press **ctrl + a** **]**. This pastes the
    selected text into the edit buffer or onto the command line or
    whatever, depending on what that session is doing.
6.  Repeat step 5 as desired.

With either the mouse method or the screen method, text is taken from
the video memory, which does not preserve the escape sequences that make
colors, boldspace and so on. That is probably what you want if you are
copying examples from a manual onto a command line, but maybe not what
you want if you are taking exerpts from one file to another.

## Splitting The Screen

Besides multiple full-screen windows, Screen can also allow two or more
programs to share the screen at once.

Use **ctrl + a** **S** (capital S) to divide your screen into two parts.
Your original session is at the top and a new blank session at the
bottom. (Be careful you don't accidentally press **ctrl + s**. This can
lock up your terminal. If you do accidentally hit **ctrl + s**, you can
unlock the terminal with **ctrl + q**.) By default, there is no program
running in this new region, but you can start one by using **ctrl + a
tab** to move to that region and then typing **ctrl + a c**.

Each region acts as an independent session, and you can switch between
sessions just as in fullscreen mode.

You can remove the current region by using **ctrl + a X**. This won't
destroy the session or what's running in it. It just turns the other
session back into a full-sized window.

## Detaching A Session

One of Screen's most powerful features is the ability to halt and
restore sessions.  Say you're doing something really interesting on
your computer, but you have to leave and go to work.  Now say that while
away you want to access what you were working on. If both machines are
accessible through the Internet, you can do that with Screen.  For
example you may start a session on a remote machine using the `telnet`
or the safer `ssh` protocol, and then invoke `screen`.  When you have to
leave, you can safely close the remote Screen session and later restore
it, maybe accessing the remote machine from the command line of a third
machine located in another suburb -- or in another continent.

Type **ctrl + a d** in your screen session. You return to your original
terminal and the screen exits, printing **[detached]**. Now if you
execute `ps`, you find that the screen is still running in the
background. Get a list of all running sessions by passing `screen` the
`-list` option.

    $ screen -list
    There is a screen on:
    12056.pts-0.hostname
    (Detached)
    1 Socket in
    /var/run/screen/S-user_name.
    $

You can reconnect to this running session by entering:

    $ screen -r 12056.pts-0.hostname

Or you can just use:

    $ screen -R

This reconnects to the first session it finds.

Now all you have to do is log into your home machine from any remote
machine and you are able to see whatever you were working on, exactly as
you left it. Use the same procedure to put a download into the
background with `wget`, or `ftp`. A detached session persists even if
you log out from the machine.

## Quitting Screen

If you have only a few programs open, you can exit screen simply by
quitting them all. However, if you have many different application and
windows open, you can exit them all by typing **ctrl + a **. You are
prompted for confirmation, and if you select yes, Screen terminates all
its programs and exits.
