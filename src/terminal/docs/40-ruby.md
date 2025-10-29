# Ruby

Ruby is a programming language that can be used to perform tasks that
would be difficult or cumbersome on the command line. To get started
with Ruby, you need to install it. Usually, there is a program on your
computer such as "Add/Remove Programs" or "Package Manager" which
allows you to easily install Ruby. You can also go to
[http://www.ruby-lang.org](https://web.archive.org/web/20160417194719/http://www.ruby-lang.org/),
where you can find downloads for Ruby as well as more information on the
language.

Just like the command line, you can either use Ruby by typing commands
in individually, or you can create a script file. If you want to type
commands in individually, install the "irb" program and use the `irb`
command at the command line:

    $ irb
    > 10 + 10
    => 20
    > exit
    $

As you can see, you can use Ruby to do basic mathematics. An important
note about Ruby is that instead of printing a value only when you use
*echo* (which is *puts* in Ruby) , Ruby will print out the result of
whatever command you enter \-- this is what the "=\>" means. When you
enter the "10 + 10" command, the result is "20". Also, remember that
you can use *exit* to get out of the `irb` program.

To write a multi-line script in Ruby, you create a file and save it with
a ".rb" at the end. You can use any text editor to create this file
\-- Emacs, Vim, Gedit, or whatever your favorite is. A script could look
like this:

    a = 1 + 2
    puts a

In this example, we create a variable, *a*, which stores the result of
"1 + 2". It then uses the *puts* command to print out the result,
which should be 3. If we save this file as *ruby.rb*, we can run it from
the command line:

    $ ruby ruby.rb
    3

The Ruby program printed out "3", just like we expected. Of course, we
can use Ruby to do more useful things. For example, we can look at all
the files in a directory:

    $ irb
    > Dir.entries('my-directory')
    => ["ruby-script.rb", "other-script.rb", "document.odt", "photo.png"]

We used the *Dir.entries* method to look at the files in *my-directory*.
You probably noticed that we pass parameters differently in Ruby.
Instead of separating them with spaces, we surround them in parenthesis.
We also need to enclose all words in quotes - not just ones that have
special characters in them.

 Let\'s try doing something with these files \-- here\'s a way to find
all of the ".rb" files in a directory:

    > files = Dir.entries('my-directory')
    => ["ruby-script.rb", "other-script.rb", "document.odt", "photo.png"]
    > for file in files
    > puts file if file.include?('.rb')
    > end
    ruby-script.rb
    other-script.rb

First, we used the *for* command to loop through each of the files. We
then get to work with each file. The next line says that we want to
print out the file if it includes the text ".rb". Finally, we end the
*for* loop.

We can also use command line code in Ruby by enclosing it in backticks.
For example, if we wanted to delete all of the ".rb" files, we could
use:

    > files = Dir.entries('my-directory')
    => ["ruby-script.rb", "other-script.rb", "document.odt", "photo.png"]
    > for file in files
    > `rm #{file}` if file.include?('.rb')
    > end

Notice how we enclosed the `rm` command in backticks. We also used #{}
to enclose the Ruby variable, so it is put in to the command properly
instead of the literal string "file".

## Learning More About Ruby

If you want to learn more about Ruby,
[http://www.ruby-lang.org](https://web.archive.org/web/20160417194719/http://www.ruby-lang.org/)
is the homepage for Ruby, and
[http://www.ruby-doc.org](https://web.archive.org/web/20160417194719/http://www.ruby-doc.org/)
is a great place to find tutorials and documentation.
:::


