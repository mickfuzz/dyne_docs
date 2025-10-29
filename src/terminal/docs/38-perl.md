# Perl

Perl is a programming language that can be used to perform tasks that
would be difficult or cumbersome on the command line. Perl is included
by default with most GNU/Linux distributions. Usually, one invokes Perl
by using a text editor to write a file and then passing it to the `perl`
program.

Perl scripts can be named anything but conventionally end with ".pl".
You can use any text editor to create this file \-- Emacs, Vim, Gedit,
or whatever your favorite is. A script could look like this:

    my $a = 1 + 2;
    print $a,"\n";

In this example, we create a variable (by using *my*) which is called
*\$a* (the dollar sign is Perl\'s way of denoting a variable), which
stores the result of "1 + 2". It then uses the *print* function to
print the result, which should be 3. The comma concatenates two or more
strings together. In this case a newline is appended to the end of the
printed string. All statements in Perl are terminated with a semicolon,
even if they are on separate lines. If we save this file as *first.pl*,
we can run it from the command line.

    $ perl first.pl
    3

The Perl program printed out "3", just like we expected. If we don\'t
want to type "perl" in order to run the script, we can put this line:

#!/usr/bin/perl

at the start (be sure to use the correct path on your system), and do
*chmod +x first.pl* to make it executable. Then we type *./first.pl* to
run it.\

Of course, we can use Perl to do more useful things. For example, we can
look at all the files in the current directory.

    my $filename;
    opendir DH, "." or die "Could not open directory!";
    while( $filename = readdir(DH) ){
      print $filename,"\n";
    }

    $ perl first.pl
    perl-script.perl
    other-script.perl
    document.odt
    photo.png

Here we use the *opendir* function to open a directory for reading.
"DH", will be our directory handle, how we refer to the open directory
for reading. A directory handle is not declared like a variable, just
created at the invocation of the *opendir* function. We pass the
directory name as a string (enclosed in double quotes); the single dot
refers to the current directory. The *or* and *die* statements tell Perl
to terminate execution if the directory cannot be opened. The final
string tells Perl what to print as an error message.

Inside the *while* loop, the *readdir* function takes our directory
handle and returns the next *filename* in the directory, storing it in
the default variable "\$\_".

Let\'s try doing something with these files \-- here\'s a way to find
all of the ".pl" files in a directory.

    opendir DH, "." or die "Could not open directory!";
    while( $_ = readdir(DH) ){
      print $_,"\n" if /.pl$/;
    }

Above we use a Perl shorthand to compress the print and evaluation into
one line since both *print* and *if* take the default variable "\$\_"
as their argument if one is not specified. The "/.pl\$/" operator
says: match any word that ends with ".pl". Below is a simpler but
wordier way to pick out the all of the files with ".txt" in them.

    my $filename;
    opendir DH, "." or die "Could not open directory!";
    while( $filename = readdir(DH) ){
      if ($filename =~ /.txt$/){
        print $filename,"\n";
      }
    }

Perl uses braces ({ }) to group statements in branching and loop
constructs, such as *if* and *while*. The "=\~" operator tells *if*,
yes, when ".txt" appears at the end of the variable or string.\

We can also use command line code in Perl by using the *system*
function. For example, if we wanted to delete all of the ".txt" files,
we could use.

    my $dir = "./";
    system("rm $dir*.txt");

Above, *system* passes its argument to a shell, which executes it
exactly as it would if we typed it in. Now if we look for any files
ending in, ".txt" we won\'t find any.

    system("ls *.txt");

## More information about Perl

The Perl web site at
[http://www.perl.org](https://web.archive.org/web/20160417194719/http://www.perl.org/)
contains an impressive amount of information and documentation about the
Perl language.
:::


