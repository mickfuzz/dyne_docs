# Making Your Own Interpreter

The bash shell and many other programs obtain lines of command input
from the keyboard or a file, and then interpret them. In the case of
keyboard input, editing facility is provided (or should be). Often this
is done by using the readline library. Here we present a simple
calculator program which uses command lines. You can lift the C source
code out of this manual, put it in a file and compile and run it in
order to study the construction and use of a command line interpreter.
The tremendous advantages that this programming approach gives relative
to the GUI approach have been discussed elsewhere in this manual. With
bdc you can recall and re-execute complicated calculations, and you can
make script files in which such calculations are saved.

This is the source code, written for GNU-Linux:


    /* The Brain-Dead Calculator.
      Build:
    gcc -g -O -Wall -o bdc bdc.c -lm -lreadline -lcurses
      Run:
    ./bdc    or   ./bdc path-to-command-file   or   path-to-exe-bdc-command-file
      Example commands:
    3 q c -2.15 * s +     # calculates sqrt(3) + sin(-2.15*sqrt(3))
    ?                     # prints help message
    */
    #define _GNU_SOURCE
    #include <stdio.h>
    #include <stdlib.h>
    #include <readline/readline.h>
    #include <readline/history.h>
    #include <math.h>
    #include <string.h>

    #define MAXLEVS 12
    int levs = 8;
    double st[MAXLEVS];

    int do_file( char *path );

    int parse( char *comm )
    {
      int cc, pop, ii, ps;
      int cp = 0, cp0;
      int cl = strlen(comm);
      double rollin;
      char *endp, *comfile;

      if ( !cl ) return 0;
      do {
        rollin = 0.0;  pop = 1;
        cc = comm[cp];
        switch( cc ) {
          default:  if ( (cc >= '0') && (cc <= '9') ) goto num;
            printf( "\nERROR at input position %d\n", cp );
            pop = 5;  break;
          case '#':
            pop = 3;  break;
          case '?':  printf(
    "\nBrain-Dead Calculator with %d-level stack;  x=quit\n", levs );
                     printf(
    "op=  + - * /   perform arithmetic:"
    "   st[1] = st[1] op st[0]   and shift st[j-1] <-- st[j]\n"
    "op=   s  q     perform functions:"
    "  st[0] = func(st[0])  s= sine  q= square-root\n"
    "op=   d c r    manipulate stack:"
    "  d= discard st[0];  c= copy st[0];  r= rotate st[j-1] <-- st[j]\n"
    "op=   &path    run the command file specified by path\n" );
          case ' ':  pop = 0;  break;
          case '+':  if ( (comm[cp+1] >= '0') && (comm[cp+1] <= '9') ) { num:
              memmove( st+1, st, (levs-1)*sizeof(double) );
              st[0] = strtod( comm+cp, &endp );
              cp += (endp-(comm+cp))-1;
              pop = 0;  break;
            }
            st[1] += st[0];  break;
          case '-':  if ( (comm[cp+1] >= '0') && (comm[cp+1] <= '9') ) goto num;
            st[1] -= st[0];  break;
          case '.':  goto num;
          case '*':  st[1] *= st[0];  break;
          case '/':  st[1] /= st[0];  break;
          case 'q':  st[0] = sqrt(st[0]);  pop = 0;  break;
          case 's':  st[0] = sin(st[0]);  pop = 0;  break;
          case 'd':  break;
          case 'c':  memmove( st+1, st, (levs-1)*sizeof(double) );
                     pop = 0;  break;
          case 'r':  pop = 2;  break;
          case '&':
            ii = 0;  cp += 1;  cp0 = cp;
            while ( comm[cp] > ' ' ) {
              cp += 1;  ii += 1;
            }
            comfile = malloc( ii+1 );  // we really should check malloc failure
            memcpy( comfile, comm+cp0, ii );  comfile[ii] = 0;
            ps = do_file( comfile );
            free( comfile );
            pop = 0;  break;
          case 'x':  pop = 4;  break;
        }
        switch( pop ) {
          case 0:  break;
          case 2:  rollin = st[0];
          case 1:  memmove( st, st+1, (levs-1)*sizeof(double) );
                   st[levs-1] = rollin;  break;
          default:  return pop;
        }
        cp += 1;
      } while ( cp < cl );
      return 0;
    }

    int do_file( char *path )
    {
      FILE *do_fb;
      char *comm;
      ssize_t len;  size_t len1;
      int  ps, ii, comcount = 0;

      do_fb = fopen( path, "r" );
      if ( !do_fb ) {
        perror( "do_file: cannot open" );
        return 0;
      }
      do {
        comm = NULL;
        len = getline( &comm, &len1, do_fb );
        if ( len == -1 ) { ps = 0;  break; }
        comm[len-1] = 0;
        ps = parse( comm );
        free( comm );  comcount += 1;
      } while ( (ps != 4) && (ps != 5) );
      fclose( do_fb );  // we really should check for fclose failure
      printf( "file %d st=", comcount );
      for ( ii = 0;  ii < levs;  ii++ ) printf( "  %g", st[ii] );
      printf( "\n" );
      return ps;
    }

    int main( int argc, char **argv )
    {
      char *comm;
      int ii, ps, comcount=0;

      if ( argc > 1 ) {
        ps = do_file( argv[1] );
        if ( ps == 4 ) return 0;
        if ( ps == 5 ) return 1;
      }
      do {
        do {
          comm = readline( "bdc> " );
        } while ( comm == NULL );
        add_history( comm );
        ps = parse( comm );
        free( comm );  comcount += 1;
        printf( "comm %d st=", comcount );
        for ( ii = 0;  ii < levs;  ii++ ) printf( "  %g", st[ii] );
        printf( "\n" );
      } while ( ps != 4 );
      return 0;
    }

Here are instructions for building and running bdc. We are doing it in
/tmp just for concreteness, but you could put it anywhere.

-   Lift the source out of this manual and put it in the file /tmp/bdc.c
-   cd to /tmp
-   Build the program by running the gcc command shown in the comment at
    top of bdc.c
-   Run the program by typing **./bdc**
-   Give bdc commands strings at the **bdc>** prompt, such as *1.23 s
    3 + q*
-   To quit, use the *x* command

The bdc command interpreter starts at the beginning of a command string
and processes commands and numbers until it gets to the end (or an
error). It maintains a stack of numbers, and commands act on them in
various ways. The command interpreter is called in two places: a loop
that gets command strings from a file, and a loop that gets command
strings from the keyboard. The keyboard input is obtained by readline,
and the strings are stored in a history list. Therefore, commands can be
edited just like shell commands, and previous ones can be recalled.

Here is a small bdc command file. Lift it out and put it in
/tmp/example.bdc. Then you can run it by typing **&example.bdc** on the
bdc command line. You can also run it from the shell by typing **./bdc
example.bdc**. It will perform the calculations, display the resulting
stack and give an input prompt. You can then do more calculations. If
you make it executable (**chmod +x example.bdc**) you can start bdc from
the shell by typing **./example.bdc**. This has the same effect as
typing **./bdc example.bdc**; in both cases the shell starts the bdc
program and passes "example.bdc" as an argument.

    #!/tmp/bdc
    # Calculate sin(1.5+sqrt(3.14))*7.9
    3.14 q 1.5 + s 7.9 *
    # Calculate sqrt(5+(previous result))
    c 5 t + q

For simplicity we have used only one-character commands, and very few of
them. The experix project on SourceForge is a much more capable
calculator. Its stack can hold numbers, arrays, strings, file controls
and other things, and its operators and commands do what makes sense
with the stack arguments that they get. There are comparison and
conditional branch operators so that experix command strings can be
full-fledged programs. Stack objects and command sequences can be
packaged in variables and then invoked by name. It draws graphs and
writes text on a framebuffer screen, using a separate server program. In
order to see the command line on the graphics screen, characters from
stdout are packaged into server commands. Some of the command files
demonstrate the technique of passing arguments to experix by including
them on the "#!..." line of the command file. Installation of experix
is far from being simple and automated, but help is available by
contacting the author.
