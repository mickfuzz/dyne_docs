# Nano

Nano is a simple editor. To open it and begin creating a new text file,
type the following at the command line:

    $ nano filepath

where *filepath* is the path to the file you want to edit (or nothing).
The screen is taken over by the program as shown in Figure 1.

![nano_openfile](images/IntroCommandLine-nano-nano_openfile-en.png "nano_openfile"){height="341"
width="507"}

 Figure 1. Opening screen for nano

The screen is no longer a place to execute commands; it has *become* a
text editor.

## Exiting nano

To exit `nano`, hold down the **Ctrl** key and press the **x** key (a
combination we call **ctrl + x** in this book).  If you have created or
altered some text but have not yet saved it, `nano` asks:

    Save modified buffer (ANSWERING "No" WILL DESTROY CHANGES) ?

To save the changes, just type **y** and nano prompts for a destination
filepath. To abandon your changes, type **n**.
To save changes without exiting, press **ctrl + o**. `nano` asks you for
the filename in which to save the text:

    File Name to Write:

Type the name of the file, and press the **Enter** key (or if the buffer
already has the right name just press **Enter**).  For instance:

    File Name to Write: textfile.txt

## Exploring Files

You can move around the file and view different parts using the arrow
keys. This is a very fast and responsive way to explore a file.

## Help

Be sure to read the man page because it has a lot of good hints. There
is help available in your nano session by typing **ctrl + g** and to get
back to your file type **ctrl + x**.
