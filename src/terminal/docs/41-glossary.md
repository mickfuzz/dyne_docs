# Glossary

##

## &

(ampersand) Execute command in background.

## >

Redirect standard output.

## >>

Append standard output.\

## <

Redirect standard input.

## |

Pipe, connecting the standard output of the preceding program with the
standard input of the following program. You can break long program
lines after pipe symbols without changing their effect.

## .

(dot) In a file path, this refers to the current directory. Before a
shell script name, this means to execute the script as if typed into the
current shell, rather than starting a new shell and executing the
command in its environment.\

## ..

(double dot) Parent of the current directory. The parent of the root
directory `/` is itself.\

## ~

(tilde) Home directory.

## /

(slash) By itself or at the beginning of a path, the root directory; in
a path, the directory separator. Thus in `/usr/bin`, it serves both
functions.

##

(backslash) At the end of a line, continues a long command on the next
line. Before a special character, escapes that character, so that
commands can deal with filenames that contain special characters. This
allows users to search for text containing special characters, for
example by using "\\\*" to search for "\*".\

## #

(hash) Comment.

## *

(splat) In file globbing, wildcard to match any string. In regular
expressions, wildcard to match any number of occurrences of the previous
element.\

## ?

Wildcard to match any character.

## ^

(caret) Wildcard to match the beginning of a line.

## !

(bang) On the command line, execute the last command that begins with
the following characters. Thus `!mv` says to run the last `mv` command.
This function has modifiers to allow editing the last command before
running it, to search for strings within commands, to run commands by
number, to run the most recent command and to run different commands
with the same arguments.

In logical expressions, such as those inside if and while statements,
negate the result of the following expression. For example,

    $ if ! mkdir -p "$DST" ; then exit 1

attempts to create a new directory, and exits if the directory cannot be
created.\

## {}

In a command following the `-exec` option of the `find` command, this is
replaced by the name of a file that was found, so that the given command
is applied to every found file.\

## \` \`

(backtick backtick) Execute command inline, and replace it with the
result.

## $( )

Execute command inline, and replace it with the result.

## [\$( )]

Execute a command within a prompt.

## $

(dollar) In a command, indicates that the value of the following
environment variable should be used, not the name. For example,
`echo $USER`. In regular expressions, a wildcard to match the end of a
line. Dollar is also usually the last character in a bash prompt.

## #!

(hashbang) Marks the beginning of an executable script. Follow with the
program to execute this file, as in `#!/bin/bash`.\

## absolute path

A file path starting from the root directory, such as `/usr/share/doc`.
Contrast with **relative path**.\

## alias

The alias command gives a name to a command string. Aliases can be made
permanent by putting them in a bash startup script such as `~/.profile`.

## apt-get

Advanced Packaging Tool, a user interface command for managing and
installing gNewSense and Debian packages.

## aptitude

Terminal-mode package manager for Debian-style packages.

## archive

A file, usually compressed, containing multiple files.\

## argument

An input value required for a command to process. Also called
"parameter". Contrast with **option**.\

## ash

A smaller version of the Bourne shell (sh). The ash shell is a clone of
Berkeley\'s Bourne shell (sh). Ash supports all of the standard sh shell
commands, but is considerably smaller than sh. The ash shell lacks some
Bourne shell features (for example, command-line histories), but it uses
a lot less memory.

## aspell

GNU Aspell is a free software spell checker designed to eventually
replace Ispell. It can either be used as a library or as an independent
spell checker.

## awk

A scripting language for data extraction and analysis from structured
text files.\

## auto completion

When the shell can determine that there is only one file starting with
the latest text on the command line, pressing **Tab** will fill in the
rest of the name. If there are several matches, the shell will fill in
the part of the names (if any) that is unique, and let the user continue
from there.

## background

By adding an ampersand, "&", at the end of a command you tell the
shell to run the program in the background, without terminal input, and
to give you a prompt so that you can continue to give commands.\

## bash

The GNU Bourne Again shell, the default shell in the GNU/Linux operating
system. Bash is an sh-compatible shell that incorporates useful features
from the Korn shell (ksh) and C shell (csh). It is intended to conform
to the IEEE POSIX P1003.2/ISO 9945.2 Shell and Tools standard. It offers
functional improvements over sh for both programming and interactive
use. Most sh scripts can be run by Bash without modification.

## bug

Program behavior other than expected or desired.

## bug report

Usually an e-mail or an entry in a bug database asking for help with a
specific bug. Good bug reports state what software was used (Linux
distribution and version, application name and version), what the user
tried to do and what was the expected result, what happened instead, and
what the user tried in order to fix it. It is particularly helpful to
explain how to reproduce the problem, if it is repeatable. Log files of
the incident should be attached.\

## builtin

A command executed by the shell itself, not by calling a separate
program. The bash command

    $ man builtins

gives details on builtins for bash itself.

## character set

A collection of abstract characters, independent of the shapes in any
particular font, with a numbering and one or more encodings. The ASCII
character set maps to the numbers 0-127, encoded in seven bits of an
eight-bit byte. ISO-8859-1 and related character sets have the range
0-255, and are encoded in 8 bits.  Unicode maps to the numbers
0-1,114,111 (17 16-bit code pages of 65,536 code points each), and has
several encodings, of which the most commonly seen is the
variable-length UTF-8.

## clobber

Overwrite a file with new data. A common result of forgetting to append
standard output with "\>\>" and instead writing a new file with
"\>".\

## command

Executable file or shell builtin.

## command completion

In bash and other shells typing part of a command or file name that is
on the path or in the current directory, and then pressing **Tab**,
often fills in the rest of the name. If not, pressing **Tab** again
gives a list of names beginning with the characters so far typed.\

## coreutils

The GNU Core Utilities are the basic file, shell and text manipulation
utilities of the GNU operating system. These are the core utilities
which are expected to exist on every operating system.

## cron job

A command to be executed automatically on a schedule set with the `cron`
command or one of its variants, such as `anacron`.

## default

In many commands, the value preset for a particular option if the
command line does not specify something different. The user can specify
a default value for unset environment variables, for example,

    $ cat "${VARIABLE_FILE_NAME:=/home/user/file}"

## dependency

Software required to run a particular piece of software. This can
include other applications, library files, fonts, images, and other
data.

## directory

A special kind of file that lists specific information on the files it
contains, including owner, group, and permissions.\

## directory stack

A place to store recently used directory paths for easy retrieval with
the commands `pushd` and `popd`. The command `dirs` displays the
directory stack.\

**emacs**

GNU Emacs text editor, originally short for Editing Macros, now
jocularly Escape, Meta, Alt, Control, Shift, from its pervasive reliance
on key combinations. You can be sure that emacs can do it, you just need
to find out how.

## environment variable

A string value assigned to a name in the environment of the current
shell.

## escape

A character that changes the interpretation of a character or sequence
of characters that follow it. This is the original use of the Escape
character. For example, in a text search the character "\*" matches a
wide range of text, while the string "\\\*" matches only an asterisk.\

## exit status

A value returned by a command to the shell, useful in scripting for
deciding what to do next.\

## file

Utility for determining file types.

## filesystem

The basic directory layout for a GNU/Linux system.

## filter

Command-line program that reads standard input and writes standard
output so that it is suitable for use in a pipeline, where each command
performs a specific transformation on the data.\

## findutils

The GNU versions of find utilities. find, locate, updatedb and xargs.

## FLOSS

Free/Libre Open Source Software, licensed so as to guarantee the
essential freedoms of software users to source code and reuse. A
combination of Free Software and Open Source Software, with Libre added
in to emphasize that software freedom is essentially a matter of rights,
not price.\

## folder

The name used in GUIs for directories.\

## fontconfig

Font configuration and customization library.

## ftp

The standard File Transfer Protocol client.

## function

In bash, the function builtin allows the user to create functions on the
fly with the syntax

    $ function name() {body}

## gawk

The GNU version of the awk text processing utility. gawk is a program
that you can use to select particular records in a file and perform
operations upon them.

## gedit

A simple and easy text editor for GNOME. It is UTF-8 compatible,
provides tools for editing source code and can be extended using
plugins.

## globbing

Referring to a group of files with an abbreviation, such as "\*" for
all of the files in a directory.\

## GNOME

A desktop, set of libraries, and application suite for X.\

## GNU

Recursive acronym for GNU\'s Not Unix. It is the Free Software
Foundation project to create a freely-licensed replacement for the
proprietary Unix operating system.\

## GNU/Linux

Operating system combining the Linux kernel with GNU software tools.

## Graphical User Interface (GUI)

User interface offering windows, icons, mouse control, multiple fonts,
and so on.\

## grep

The GNU versions of grep pattern matching utilities. Grep searches one
or more input files for lines containing a match to a specified pattern.

## group

To simplify security, Unix systems organize users into groups, and
assign a group owner as well as an individual owner to every file. In
this way, system operators, for example, can be given control over
certain system resources all at once, or everybody working on a project
can gain access to all project files by joining the project group. Each
user has a group with the same name for that user\'s files.\

## gzip

The GNU data compression application. gzip.org

## history

Record of previously executed commands that can be recalled and executed
again with the up arrow key.\

## intltool

Utility for internationalizing various kinds of data files.

## kernel

The Linux kernel, core of the GNU/Linux operating system. kernel.org

## kernel-utils

Kernel and Hardware related utilities.

## less

A text file browser similar to `more`, but better as it can move back
and forth through the file.

## locale

Values of a set of environment variables that store information on the
user\'s language, country, and character encoding, and options for date
and time formatting, money, measurements, and other such information.
Also the name of the command to display all of these environment
variables.

## log

A file, often in the `/var/log` directory, that contains notes made by
running programs about their progress and about any problems they
encounter. Vital information whenever something goes wrong.\

## lsof

A utility which lists open files on a GNU/Linux system.

## lynx

###

A text-based Web browser. lynx.browser.org

## make

An essential program for installing much unpackaged source code
software. The developers can write down all of the complex information
about how to configure, compile, and install their work in make files
that you usually won\'t have to read. Just check the README or INSTALL
files that come with the source code to see whether it uses this system,
or has different instructions. Another program with similar functions is
jhbuild.\

## man

A set of documentation tools: man, apropos and whatis.

## mc

The Midnight Commander, a user-friendly text console file manager and
visual shell.

## more

A utility for displaying text files one screenful at a time. See also
*less*.\

## openssh

The OpenSSH implementation of SSH protocol versions 1 and 2.

## option

A value specified to a command using the form `--option` (long option)
or `-o` (short option). Contrast with **argument**, which is a required
input.\

## package manager

Software to install, remove, and otherwise manage applications as
packaged by a particular distribution, particularly making sure that
dependencies and compatibilities between software components are
observed. The two main varieties are Red Hat/Yellow Dog `yum` and Debian
`apt-get`.

## parameter

Argument.\

## passwd

The passwd utility for setting/changing passwords using PAM.
netadmintools.com

## PATH

Environment variable specifying where the current shell should look for
command files.\

## Perl

Practical Extraction and Report Language, or jocularly, Pathologically
Eclectic Rubbish Lister. Perl is a dynamic programming language
particularly suitable for text processing and manipulation.\

## pipe

A connection between two commands, so that the standard output of the
first becomes the input of the second. Indicated with the character
"\|".

## plain text

A message or file represented as a sequence of characters in a specific
character encoding, with no extra formatting information. Contrast with
**rich text**. HTML files are plain text, but contain markup tags so
that they represent rich text.\

## process

A running program. Each process has a process ID, a number that you can
use to identify the process in a command.

## prompt

The text string displayed by a shell when waiting for command input.

## recursive command execution

With the `-r` or `-R` options, many commands will act on the current
directory and any subdirectories. Check command documentation to
determine the precise syntax.\

## redirection

Sending a file or standard output from a command to standard input of a
command, or sending standard output or error output of a command to
standard input of another command, or to a file.\

## regular expression

A string such as "\*.png" that defines a pattern for matching text or
filenames using special characters to indicate which alternatives to
include.\

## relative path

A file path starting from the current directory, such as `docs` or
`../Pictures`. Contrast with **absolute path**.

## rich text

Text with formatting, including fonts, multiple type sizes, positioning,
color, and much more. HTML and word processing files are forms of rich
text. Contrast with **plain text**.\

## root

1.  root user, or superuser, a required account with permission to do
    anything on a GNU/Linux system.\
2.  The starting point of the directory tree, written "/". All other
    directories are specified by paths from this root directory.

## script

An executable text file.\

## sed

A GNU stream text editor.

## shell

A command interpreter such as sh or bash.

## stack

A way to keep track of tasks and other information so that the last item
saved (pushed) on the stack is the first item retrieved (popped) from
the top of the stack. An example is the bash history stack.\

## standard input, standard output, and standard error

Communication channels provided to every running command. If not
otherwise specified at the command line, they connect to the user\'s
terminal, but they can be redirected to files or through pipes to other
programs.

## string

A [string]{.il} is a [sequence]{.il} of characters in a particular
character set. Examples in ASCII include the sentence "Hello, World",
the URL
"[http://www.flossmanuals.net/](https://web.archive.org/web/20160417194719/http://www.flossmanuals.net/){target="_blank"}",
and  the text message "No such file or directory." Unicode strings can
include any combination of languages, such as "Japan (日本) and Korea
(대한민국)".

## sudo

A command to allow

specified users and groups to run specified programs with superuser
privileges

. The file `/etc/sudoers` contains the specifications. The command
`sudoedit` is provided for editing this file. It checks whether the
edited file is in the correct format.

## superuser

The root account, which has all permissions.

## syntax highlighting

Displaying the text of various elements of a program, such as function
names, variable names, strings, and keywords, in distinctive colors
appropriate to the programming language (bash, Perl, Python, etc.) or
markup format (HTML, XML, etc.) used. The colors are not stored with the
file, but computed by the text editor when loading a file and during
editing.\

## telnet

The client program for the telnet remote login protocol.

## terminal

Originally, a printing terminal such as a Teletype, or a video terminal.
Now a virtual terminal in a text or graphics window. In all cases, a
device or program for typing input and displaying output.\

## time

A GNU utility for monitoring a program\'s use of system resources.

## Unicode

The universal character set, meant to replace the jumble of more than a
hundred other character set standards for 30 modern writing systems and
dozens of others.

## unzip

A utility for unpacking zip archives.

## UTC

Coordinated Universal Time, or Temps Universel Coordonné, which has
replaced Greenwich Mean Time as the world standard. The abbreviation UTC
is a compromise between the English and French names.\

## vi

Visual editor, a powerful terminal-mode editor.\

## vim

The VIM editor, an extension of vi.

## wget

A utility for retrieving files using the HTTP or FTP protocols.

## which

Displays where a particular program in your path is located.

## wildcard

A character that can match more than one string in file globbing or
regular expression matching.\

## X

The standard windowing system for GNU/Linux.\

## yum

Yellowdog Update Manager, a package manager used in Red Hat and related
GNU/Linux distributions that use RPM packages.

## zip

A file compression and packaging utility compatible with PKZIP.

