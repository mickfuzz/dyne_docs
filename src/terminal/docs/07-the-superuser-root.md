# The Superuser (Root)

Some parts of the computer system are thought to require special
protection (because they do). If somebody can change the basic `cat` or
`less` command, for instance, they could cause you to corrupt your own
files. So certain commands can be run and certain files can be accessed
only by a user logged in with special privileges called *superuser* or
*root* privileges.

In the days when computer systems cost hundreds of thousands of dollars
and were shared by hundreds of people, *root* was assigned to an actual
person (or a small group) who constituted a kind of priesthood. Nowadays
every owner of a PC can execute superuser commands (this is not always
true on mobile devices, though). There is still a user account on each
GNU/Linux system called *root*. This allows the system to make this user
the owner of sensitive system files.

The *root* user, incidentally, has nothing to do with the root directory
(the / directory) in the filesystem.

Superuser commands are powerful and must be used carefully, but their
use is quite common. For instance, whenever a desktop user installs
software, he or she must become superuser for a few minutes.

## The sudo Command

On many modern systems, whenever you want to enter a superuser command,
you just precede it with `sudo`:

    $ sudo rm -r /junk_directory

You are then prompted for your password, so nobody walking up casually
to your system could execute a dangerous command. The system keeps your
password around for a while, so you can enter further superuser commands
without the bother of re-entering the password.

Systems also provide a `su` command that logs you in as superuser and
gives you a new shell prompt. Not all systems allow users to use it,
though, because you can get carried away, start doing everyday work as
superuser --and suddenly realize you've trashed your system through a
typo. It is much safer to do your home system administration using
`sudo`.

If other people share your system and you want to give someone superuser
privileges, for this you need to know a little more about System
Administration.
