# Files and Directories

Although you\'re most interested in files in your own folder or
directory, it helps to know what else is on your system. In this chapter
we\'ll look around a GNU/Linux system.

Here is a list of the most common directories right beneath the root
directory (the one whose name is just "/"):

    /bin     basic programs (Programs that are absolutely needed,
             shell and commands only)
    /boot    initialization files (Required to actually boot your computer)
    /dev     device files (Describe physical stuff like hard disks
             and partitions)
    /etc     configuration files
    /home    users' home directories
    /lib     libraries (collections of data and functions) for sytem boot
             and running system programs
    /media   mount points for removable media
    /mnt     mount points (For system admins who need to temporarily
             mount a filesystem)
    /opt     third-party programs
    /proc    proc filesystem (Describe processes and status info,
             not stored on disk)
    /root    system administrator's files
    /sbin    basic administration programs (Like bin, but only
             usable by administators)
    /srv     service-specific files
    /sys     sys filesystem (Similar to proc, stored in memory
             based filesytem: tempfs)
    /tmp     temporary files (Files kept only a short time depending
             on system policy, often in tempfs)
    /usr     users' programs (Another bin, lib, sbin, plus local,
             share, src, and more)
    /var     variable data preserved between reboots

You don\'t need to know about the directory structure outside your home
directory in order to run applications, but this knowledge occasionally
comes in handy. Perhaps the most common uses are when you want to change
a system-wide configuration file or view log messages. Log files
generally contain progress information and error reports from programs,
and may reveal the source of problems (bugs, configuration errors,
missing or corrupted files) on your system. Many log files are kept in
the */var/log* directory, but some programs put their log files in
hidden directories in the user\'s home directory. An example is
\~/.sugar/default/logs.\

Historically, GNU/Linux system configuration was done through editing
text files. Today, most popular GNU/Linux systems encourage users to
make changes to the system configuration through graphical
administration tools. Sometimes however, this is not possible or
desirable, and you may find yourself editing the configuration files in
a text editor. This is usually trickier, as you need to know where these
files are and how to edit them, and in some cases you also need to
signal or restart a running program so it will read in your changes.
That said, this method has its advantages, such as the ability to
configure computers with no graphics capabilities, or configure programs
that have no graphical configuration program (or a clumsy and incomplete
one).

NOTE: For a full description of the file-system structure of a typical
Linux system, try typing `man hier` in a terminal. This not only gives a
brief on the above top-level directories, but also gives insight into
why Linux has *both* a */usr/bin* and a */usr/sbin*, for instance.\

\
:::


