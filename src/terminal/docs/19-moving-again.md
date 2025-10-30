# Moving Again

So far you have probably already used the `cd` command to change your
current working directory and the `pwd` command to find out what your
current working directory is.  After you work with the command line for
a while, you'll find yourself changing directories constantly.  In
order to make this easier, Bash provides a "directory stack" that you
can use to quickly move around directories where you are doing some
work.  (We'll show some examples in a moment that help explain the idea
of a "stack.") You have the following commands at your disposal:

    {align="center"}


  ----------------------------------- -----------------------------------
    Command                           Action

    dirs                              Display directory stack, top level
                                      first (left); other commands do
                                      this after their main action. Not
                                      all command options are shown in
                                      this table.

    pushd dir                         Push dir to the top of the stack
                                      and change the current working
                                      directory to it

    pushd                             Swap the top two stack levels and
                                      cd to the new top-of-stack

    pushd **+N**                      Rotate the whole stack **left**
                                      through **N** steps and cd to the
                                      new top-of-stack

    pushd **-N**                      Rotate the whole stack **right**
                                      through **N+1** steps and cd to the
                                      new top-of-stack

    popd                              Remove the top-of-stack dir and cd
                                      to the new top-of-stack
  ----------------------------------- -----------------------------------

If you need to have more of a visual aid to understanding a "stack",
the simplest way is to imagine the "stack" as a pile of papers on your
desk, you "push" new pieces of paper onto the top of the "stack" and
you "pop" the top most piece of paper off the "stack". Both methods
work on the principle of LIFO (last in first out).

You can play around with these commands to understand how they work.
For example the following table provides a list of commands, their
effect on the current working directory and their effect on the stack.

```
+-----------------------+-----------------------+-----------------------+
| Command               | Current working       | Stack after command   |
|                       | directory after       |                       |
|                       | command               |                       |
+-----------------------+-----------------------+-----------------------+
| cd                    | ~                     | ~                     |
+-----------------------+-----------------------+-----------------------+
| pushd /               | /                     | /                     |
|                       |                       |                       |
|                       |                       | ~                     |
+-----------------------+-----------------------+-----------------------+
| pushd /usr/bin        | /usr/bin              | /usr/bin              |
|                       |                       |                       |
|                       |                       | /                     |
|                       |                       |                       |
|                       |                       | ~                     |
+-----------------------+-----------------------+-----------------------+
| pushd +1              | /                     | /                     |
|                       |                       |                       |
|                       |                       | ~                     |
|                       |                       |                       |
|                       |                       | /usr/bin              |
+-----------------------+-----------------------+-----------------------+
| pushd +1              | ~                     | ~                     |
|                       |                       |                       |
|                       |                       | /usr/bin              |
|                       |                       |                       |
|                       |                       | /                     |
+-----------------------+-----------------------+-----------------------+
| popd                  | /usr/bin              | /usr/bin              |
|                       |                       |                       |
|                       |                       | /                     |
+-----------------------+-----------------------+-----------------------+
| pushd +1              | /                     | /                     |
|                       |                       |                       |
|                       |                       | /usr/bin              |
+-----------------------+-----------------------+-----------------------+
| popd                  | /usr/bin              | /usr/bin              |
+-----------------------+-----------------------+-----------------------+
```
