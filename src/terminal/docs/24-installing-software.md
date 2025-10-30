# Installing Software

Installing software on GNU/Linux is a broad subject because each version
of GNU/Linux has its own way of doing things. Most are variations on
apt-get (Advanced Packaging Tool), used by Debian, Ubuntu, gNewSense,
and related distributions) or yum (Yellowdog Update Manager), used by
Fedora, BLAG, and related distributions.  The basic syntax is

    $ sudo apt-get install packagename
    $ sudo yum install packagename

Several apt-get and yum functions have the same name and act in the same
way, but by no means all. When you want to go beyond the simple cases
described here, be sure to check the documentation for whichever you are
using.

These examples use `sudo` to remind you that installing software and
editing configuration files require superuser privileges. You can either
use `sudo` with each command, or switch to being superuser with the `su`
command. (Remember to exit your superuser session before resuming normal
user work.)

There are numerous options to each command. To uninstall a package, use
this command.

    $ sudo apt-get remove packagename
    $ sudo yum remove packagename

To read repository index files, and update the local package database.

    $ sudo apt-get update
    $ sudo yum update

To install all available newer versions of packages.

    $ sudo apt-get upgrade

To fix broken dependencies, if any.

    $ sudo apt-get --fix-broken

The `yum` command does not have this option. There are other ways to
deal with broken RPM package dependencies, but they require more help
than we can give you here.

Users can configure multiple package repositories to download from by
editing */etc/apt/sources.list* as superuser. Be careful. Back up the
current file before making any changes.

All types of GNU/Linux allow the user to install software using the
source code. For software in Debian-style packages, you can use

     $ apt-get source packagename

The `yum` utility does not handle source installs.

Compiling from source is especially important for software that is not
available in packages, typically because it is too new. You probably
don't want to tackle this process unless you know a little bit about
how to use GNU/Linux commands and a little about the GNU/Linux file
system, but whenever you decide to try out something brand new and
possibly unfinished, this is the most common method. If you don't know
about commands and file systems, you can easily get lost doing a source
code installation. It is better to read up on them first, get
comfortable with them, and then return here.

Installing from source works on any GNU/Linux system that has the
compiler and related tools and libraries, so it's a good process to
know, and it more or less follows this route once you have a source
package:

1.  Unpack the archive and `cd` to its base directory.
2.  Run the configure script **./configure**
3.  Compile the software **make**
4.  Install the software **make install**

To carry out the second and third steps, you must have compiler tools on
your system. Some GNU/Linux systems come with these tools automatically,
but others do not. Any system you are likely to use with this book,
though, allows you to download the tools you need for free; search for
the packages containing `gcc` and `binutils`.

## Dependencies

Before we start, a word on dependencies. GNU/Linux developers often
don't write an application from scratch; they rely heavily on work that
has been done previously by other programmers. This is a smart practice,
of course, because it saves time, and to aid this process many
kind-hearted individuals have made libraries of code that other
programmers can easily access and use within their own programs. These
libraries are stored in fixed places in the GNU/Linux file system,
usually in the directories whose names begin with */lib*, */usr/lib*,
and */usr/share/lib*.

If you install an application that requires certain libraries, it's
easy as long as you have those libraries already installed on your
system. However, if you don't have the required libraries, you need to
find them and install them. If the programmers are thoughtful, they will
have included information about dependencies in either the *README* or
*INSTALL* files that you will find in the source directory of the
application. Some extremely nice programmers give you both the name and
the URL where you can get the necessary bits.

However, if you are installing software on a distribution other than the
one it was developed on, you are likely to find libraries packaged quite
differently than on the developer's system. In this case you may have
to use the trial and error method: try compiling the source, and when
you get an error message telling you of a missing dependency, try to
install it. If you can't install it using the name given, you may need
to ask someone more experienced how to find the appropriate package, or
go looking for documentation of your distribution's packaging
policies.

Usually, lazy GNU/Linux users don't bother to read these files so they
just go through the standard process and find that the configure stage
will give an error telling them what libraries are missing. These lazy
types (this author included) then find the required bits and pieces
online and install them.

However, if you are new to GNU/Linux, I suggest that you read the
*README* and *INSTALL* before starting any installation process. It will
save you time and heartache.

Just remember that although a dependency list might be long, you simply
get all the necessary packages and install them one by one, following
the same process described in the previous section, until finally you
have everything you need for the program of your dreams to install and
run.

Next, let's look at the installation process a little more in depth.

## Unpack the archive

Most software sources come in the form of a compressed "tape archive"
files that usually have a suffix like ".tar" or ".tgz". The GNU
`tar` command can automatically uncompress files ending with a *.gz* or
*.tgz* suffix (which means the distributor used GZIP compression) but if
other forms of compression were used (such as BZIP2 or LZMA) you have to
use the appropriate uncompression program to retrieve the *.tar* file
(colloquially known as a *tar ball*). To unpack the archive, use the
`tar` command:

    $ tar zxvf packagename.tar.gz

Where "packagename" in the example above is the actual name of your
package that you wish to install. The `tar` command followed by the
parameters `zxvf` uncompresses a *tar.gz* file and creates a new
directory with all the extracted sources. The 'z' specifies BZIP
compression; if the file suffix is ".tgz2", specifiy BZIP2 compression
by using a 'j'. Don't worry-- if it fails to extract you just get an
error message. You can remove the *tar.gz* file after it successfully
unpacks.

Now you must change your working directory to this new directory using
the `cd` command. Usually the new directory name is the name of the
compressed source package minus the compression suffix. For example, if
my package really was called *newsoftpack-1.0-alpha.tar.gz*, then after
running the `tar zxvf` command on it I would be left with a new
directory called *newsoftpack-1.0-alpha* and would type
`cd newsoftpack-1.0-alpha` to enter this new directory. If you are not
sure of the name of the newly created directory, type `ls`.

## Run the configure script

Once inside the new directory, we want to start the actual installation
process. To do this, most of the time you will need to type the
following:

    $ ./configure

Properly packaged source distributions usually contain a script that
checks for needed utilities and libraries and prepares the source tree
for building and installation. In this case, we will assume that it is
`configure`, since it is a very popular choice for such a script.
Sometimes, the command you need to use is different. In those cases,
look for information in the *README* or *INSTALL* file.

In the command shown, by putting a dot and a slash before the name of
the script (`./configure`) you are telling GNU/Linux to execute (run) a
script called `configure` from the current directory (denoted by
"`./`"). The script then does its stuff, checking what kind of a
computer you have, what you already have installed, what kind of
GNU/Linux you are running, and so on.

One option to `configure` is particularly common: the `--prefix` option,
which tells `configure` you want the software installed in a non-default
location. On most systems, the default is fine, and it may be where
other software expects to find the software or library you're
installing. But sometimes you can't install the software into a shared
location or you want it somewhere under your own home directory because
you know you're the only person using it. To change the directory where
the software will ultimately be installed, specify it with `--prefix`:

    $ ./configure --prefix ~/bin/myprogs

The most common problem that will occur at this stage is that the
configure script will halt and tell you that some software library that
the new software depends on is missing. If you do experience this error,
check the *README* and *INSTALL* files in case they tell you how to
repair the problem, then use a search engine if necessary to find out
what software the error message is talking about and where to get it.
Then start the installation process again with this new package. This
means that an installation sometimes can take days while you search and
download all the packages you need. This is one of the great advantages
of package management systems such as yum and apt-get: when developers
create packages for these systems, they automate the installation of
dependencies.

In some cases, dependencies are optional. The `configure` script
actually supports a lot of options. You can see what options your
software package supports by running:

    $ ./configure --help

## Compile the software

Assuming the `configure` process finished successfully, the next command
to type in the installation process is:

    $ make

If you have several processors or processor cores, you can use multiple
jobs to speed up processing by adding a `-j` option:

    $ make -j3

These commands actually make ("compile") the software for you. You
will then end up with a whole lot of compiled files which in total makes
up your software. The `make` process can take a while, depending on the
speed of your machine and the size of the package sources you are
installing. Running other processor-intensive applications will also
slow down the process.

In the second command shown, `-j3` tells `make` to try to run 3
compilation processes simultaneously, which will allow you to utilize
processor resources better if you have a dual-core or bigger machine.
The number after `-j` is arbitrary, but a good rule of thumb is the
number of processor cores plus one.

As with `configure`, you may encounter errors during compilation. In
such a case, if you can't fix the problem yourself, contact the
developer of the software and politely ask for help, explaining your
problem very clearly. The Web page
[http://www.catb.org/~esr/faqs/smart-questions.html](https://web.archive.org/web/20160417194719/http://www.catb.org/~esr/faqs/smart-questions.html)
explains how to write polite and helpful problem reports. But first, see
if there are log files from the configure and make steps. These may give
you more information than was presented on the screen, even if you
managed to see what was on the screen as it all flashed by. You can also
repeat these steps, adding " &> logfile" to the command in order to
capture all output in logfile (use a filename that does not already
exist). Before repeting the make step you should probably do "make
clean"  in order to remove objects made by previous attempts.

## Install the software

After `make` has finished without errors, type the following:

    $ sudo make install

This will install the newly created files from your software in the
correct locations in your system. This is usually under */usr/local/*,
though this can be overridden with a `configure` option, as we have
seen. Because software is usually installed in a shared directory that
only the root user can write to, you need to start the command with
`sudo` to have permission to add your software. You don't need the
`sudo` if you told `configure` to install into a directory under your
own home directory.

So now you just need to type the name of the application in your
terminal window and it should run. If it fails to start, a common remedy
is to type `ldconfig` and then try again. `ldconfig` updates the system
so that your operating system knows that there are new library files
present.
