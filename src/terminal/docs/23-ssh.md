# SSH

The command line is such a useful tool that it won\'t be long before you
need to have access to the command line on a computer that is not
sitting in front of you.  In the old days, before security was a
concern, people used `telnet` to get a command line on a remote
computer.  For most purposes, `telnet` is no longer a good idea, because
data is transmitted in a raw, unencrypted format.  The standard secure
way to gain access to a command line on a remote computer is via `ssh`
(secure shell).  The simplest invocation of the command is

    $ ssh othermachine.domain.org

This command assumes that your username on the remote machine is the
same as your username on the local machine at which you type the
command.  The remote machine prompts you for your password.  If your
username on the remote machine is different than your username on the
local machine, use the `-l` (lower-case \"L\") option to indicate your
username on the remote machine.\

    $ ssh -l remoteusername othermachine.domain.org

Alternatively, you can use email-style notation to indicate a different
username.\

    $ ssh remoteusername@othermachine.domain.org

So far, all these commands display a command line on the remote machine
from which you can then execute whatever commands that machine provides
to you.  Sometimes you may want to execute a single command on a remote
machine, returning afterward to the command line on your local machine.
This can be achieved by placing the command to be executed by the remote
machine in single quotes.\

    $ ssh remoteusername@othermachine.domain.org 'mkdir /home/myname/newdir'

`Sometimes` what you need is to execute time consuming commands on a
remote machine, but you aren\'t sure to have sufficient time during your
current `ssh` session.  If you close the remote connection before a
command execution has been completed, that command will be aborted.  To
avoid losing your work, you may start via `ssh` a remote `screen`
session and then detach it and reconnect to it whenever you want.  To
detach a remote `screen` session, simply close the `ssh` connection: a
detached `screen` session will remain running on the remote machine.

`ssh` offers many other options, which are described on the manual page.
You can also set up your favorite systems to allow you to log in or run
commands without specifying your password each time. The setup is
complicated but can save you a lot of typing; try doing some Web
searches for \"ssh-keygen\", \"ssh-add\", and \"authorized_keys\".

## scp: file copying

The SSH protocol extends beyond the basic `ssh` command.  A particularly
useful command based on the SSH protocol is `scp`, the secure copy
command.  The following example copies a file from the current directory
on your local machine to the directory */home/me/stuff* on a remote
machine.

    $ scp myprog.py me@othermachine.domain.org:/home/me/stuff

Be warned that the command will overwrite any file that\'s already
present with the name */home/me/stuff/myprog.py*. (Or you\'ll get an
error message if there\'s a file of that name and you don\'t have the
privilege to overwrite it.) If */home/me* is your home directory, the
target directory can be abbreviated.\

    $ scp myprog.py me@othermachine.domain.org:stuff

You can just as easily copy in the other direction: from the remote
machine to your local one.

    $ scp me@othermachine.domain.org:docs/interview.txt yesterday-interview.txt

The file on the remote machine is *interview.txt* in the *docs*
subdirectory of your home directory. The file will be copied to
*yesterday-interview.txt* in the home directory of your local system

`scp` can be used to copy a file from one remote machine to another.

    $ scp user1@host1:file1 user2@host2:otherdir

To recursively copy all of the files and subdirectories in a directory,
use the `-r` option.

    $ scp -r user1@host1:dir1 user2@host2:dir2

See the `scp` man page for more options.

## rsync: automated bulk transfers and backups

`rsync` is a very useful command that keeps a remote directory in sync
with a local directory. We mention it here because it\'s a useful
command-line way to do networking, like `ssh`, and because the SSH
protocol is recommended as the underlying transmission for `rsync`.

The following is a simple and useful example. It copies files from your
local */home/myname/docs* directory to a directory named *backup/* in
your home directory on the system *quantum.example.edu*. `rsync`
actually minimizes the amount of copying necessary through various
sophisticated checks.\

    $ rsync -e ssh -a /home/myname/docs me@quantum.example.edu:backup/

The `-e` option to `ssh` uses the SSH protocol underneath for
transmission, as recommended. The `-a` option (which stands for
\"archive\") copies everything within the specified directory. If you
want to delete the files on the local system as they\'re copied, include
a `--delete` option. See the `rsync` manual page for more details about
`rsync`.

## Making life easier when you use SSH often

If you use SSH to connect to a lot of different servers, you will often
make mistakes by mistyping usernames or even host names (imagine trying
to remember 20 different username/host combinations). Thankfully, SSH
offers a simple method to manage session information through a
configuration file.

The configuration file is hidden in your home directory under the
directory *.ssh* (the full path would be something like
*/home/jsmith/.ssh/config* \--if this file does not exist you can create
it). Use your favorite editor to open this file and specify hosts like
this:

    Host dev
    HostName example.com
    User fc

You can set up multiple hosts like this in your configuration file, and
after you have saved it, connect to the host you called \"dev\" by
running the following command.

    $ ssh dev

Remember, the more often you use these commands the more time you save.
:::


