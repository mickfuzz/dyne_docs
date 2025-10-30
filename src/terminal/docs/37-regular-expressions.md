# Regular Expressions

When you're looking through files or trying to change text, your needs
are often ambiguous or approximate. Typical searches include:

-   Finding an indeterminate number of things, such as "one or more
    zeroes".
-   Finding text that can have variants, such as "color" and
    "colour", or "gray" and "grey".
-   Extracting parts of text that forms a pattern. For instance, suppose
    you have a list of email addresses such as
    [somebody@fsf.org](https://web.archive.org/web/20160417194719/mailto:somebody@fsf.org)
    and
    [whoever@flossmanuals.net](https://web.archive.org/web/20160417194719/mailto:whoever@flossmanuals.net),
    and you want to extract the parts after the @ sign (fsf.org and
    flossmanuals.net, respectively).

To search for such strings and (if you want) make replacements, a
special language called *regular expressions* is invaluable. This
section offers a quick introduction to regular expressions. The language
can be intimidating at first--but it is not complicated, only terse.
You have to use it a bit so that your brain gets used to picking the
regular expressions apart.

The easiest way to learn and practice regular expressions is to use the
simple filters provided with the shell, such as grep, Sed, and AWK. The
grep command has popped up several times already in this book. In this
section we'll use an "extended" version named egrep, because it
provides more of the features people use frequently in regular
expressions. Sed and AWK were introduced in previous sections; we'll
use Sed a lot in this one.

By the end of this section, you'll understand grep, egrep, and Sed
pretty well. You will then be able to move on and use regular
expressions in other situations where they are even more powerful.
Nearly every modern language, including the other scripting languages
mentioned in this book (Perl, Python, and Ruby) offer regular
expressions. Even databases offer some form of regular expressions.

The details of regular expressions vary from one tool to another, and
even from one version of a tool to another version of the same tool.
We'll show pretty common features, but they don't all work in every
tool.

## Plain text

A regular expression doesn't have to be fancy. Up to now, the grep
commands we've shown look for plain text:

    $ cat color_file
    Primary colors blue and red make the color magenta
    Primary colors blue and green make the colour cyan
    Primary colors red and green make the colour yellow
    Black and white make grey
    $ grep 'colour' color_file
    Primary colors blue and green make the colour cyan
    Primary colors red and green make the colour yellow

Because "colour" contains no metacharacters the shell would interpret,
we don't need the single quotes, but we're using them to get into the
habit. The egrep commands in this section will use lots of
metacharacters.

## Indeterminate quantities

One simple application of regular expressions is to search for "any
number" of something, or a fuzzy amount such as "3 to 5" hyphens.
Here are the metacharacters that support this. For the sake of
simplicity, we'll show some in isolation and then use them in some
tools.

  ----------------------------------- -----------------------------------
    Match zero or more of X           X*

    Match one or more of X            X+

    Match zero or one of X            X?

    Match from 3 to 5 of X            X{3,5}
  ----------------------------------- -----------------------------------

These look a lot like shell (globbing) metacharacters, but there are
subtle differences. Focus on what they mean in regular expressions and
remember that it is really a separate language from the shell
metacharacters.

Now we can see how to find both "color" and "colour" in one search.
We want either zero or 1 "u", so we specify:

    $ egrep 'colou?r' color_file
    Primary colors blue and red make the color magenta
    Primary colors blue and green make the colour cyan
    Primary colors red and green make the colour yellow

The asterisk (*) is one of the most common and useful metacharacters in
regular expressions, but also one of the most confusing and misused.
Suppose you wanted to remove zeros from a line. You might try to remove
"any number of zeros" through an asterisk:

    $ echo "There are 40006 items" | sed "s/0*/X/"

But the output is:

    XThere are 40006 items

This happened because Sed replaces the first occurrence of the pattern
you request. You requested "zero or more" and the first occurrence of
that is the beginning of the line!

In this case, you want the plus sign (0+), but many versions of Sed
don't support it. You can work around that with:

    $ echo "There are 40006 items" | sed "s/00*/X/"
    There are 4X6 items

If you put a single digit in the brackets, such as {3}, it means "match
this number exactly". If you include the comma without a second digit,
such as {3,}, it means "match any number of three or more."

## Indeterminate Matches, Classes, and Ranges

To match any character, just specify a dot or period (.). Thus, the
following matches a slash followed by any single character and another
slash:

    $ egrep '/./' file

The dot is commonly combined with one of the fuzzy quantifiers in the
previous section. So the following matches any number of characters (but
there has to be at least one) between slashes:

    $ egrep '/.+/' file

The following is the same except that it also finds lines with two
slashes in a row (//):

    $ egrep '/.*/' file

A period is a common character in text, so you often want a dot to mean
a dot--not to have its special metacharacter meaning. Whenever you need
to search for a character that your tools considers a metacharacter,
precede it with a backslash:

    $ egrep '.' file

That command finds just a dot. Because the backslash causes the dot
character to escape being treated as a metacharacter, we speak of
*escaping* characters through a backslash.

If you find yourself searching for strings that contain punctuation and
you don't want any of the punctuation treated as a metacharacter, it
can be tiresome and difficult to escape each character. Consider using
fgrep (which stands for "fixed grep") for these strings instead of
grep or egrep. The fgrep command looks for exactly what you pass, and
doesn't treat anything as a metacharacter. You still have to use single
quotes so the shell doesn't treat anything as a metacharacter.

Square brackets let you specify combinations of characters. For
instance, to search for both "gray" and "grey" you specify:

    $ egrep "gr[ae]y" color_file
    Black and white make grey

To match the letters that are commonly used as vowels in English, write:

    [aeiouy]

The order of characters never matters inside the brackets. We can find
lines without vowels by submitting the regular expression to egrep.
We'll start with a rather garbled file name *letter_file*:

    This is readable text.
    Ths s grbg txt.
    This is more readable text.
    aaaai

Note that the second line contains no vowels, whereas the last line
contains only vowels. First we'll search for a vowel:

    $ grep '[eauoi]' letter_file
    This is readable text.
    This is more readable text.
    aaaai

The line without vowels failed to match.

Now let's look for non-vowel characters. That means not only
consonants, but punctuation and spaces. We can *invert* the character
class by putting a caret (^) at the front. In a character class (and
nowhere else) the caret means "everything except the following".
We'll do that here with five vowels (allowing the "y" to be matched
because it can also be a consonant):

    $ grep '[^eauoi]' letter_file
    This is readable text.
    Ths s grbg txt.
    This is more readable text.

This time only the last line was left out, because it had vowels and
nothing else.

A character class can also contains ranges, which you indicate by a
hyphen separating two characters. Instead of [0123] you can specify
[0-3]. People often use the following combinations:

  ----------------------------------- -----------------------------------
  Any digit                           [0-9]

  Any lowercase letter                [a-z]

  Any uppercase letter                [A-Z]

  Any letter                          [a-zA-Z]
  ----------------------------------- -----------------------------------

As the last example in the table shows, you can combine ranges inside
the brackets. You can even combine ranges with other characters, such as
the following combination of letters with common punctuation:

    [a-zA-Z.,!?]

We didn't have to escape the dot in this example because it isn't
treated as a metacharacter inside the square brackets.

Note that a character class, no matter how large, always matches a
single character. If you want it to match a lot of characters, use one
of the quantifiers from the previous section:

    $ egrep '([a-zA-Z.,!?]+)' file

This matches parentheses that enclose any number of the characters in
the character set. We had to escape the parentheses because they are
metacharacters--very special ones, it turns out. We'll look at that
next.

## Groups

Parentheses allow you to manipulate multiple characters at once.
Remember that character classes in square brackets always match a single
character, even though that single character can be many different
things. In contrast, groups can match a sequence. For instance, suppose
you want to match quantities of a thousand, a million, a billion, a
trillion, etc. You want to match:

1,000

1,000,000

1,000,000,000

etc.

You can do that by putting the string ",000" in a group, enclosing it
in parentheses. Now anything you apply to it--such as the +
character--applies to the whole group:

    $ egrep '1(,000)+' file

But parentheses do even more. They store what they match, which is
called *capturing* it. Then you can refer to it later.

This is a subtle and possibly confusing feature. Let's show it by
looking at a file that repeats some sequences of characters:

    This bell is a tam-tam.
    This sentence doesn't appear in the egrep-generated output.
    I want it quick-quick.

The first line contains the word "tam", a hyphen, and then "tam"
again. The third line contains "quick", a hyphen, and then "quick"
again. These lines don't actually have strings in common, except for
the hyphen (which appears in the second line too, so searching for it
doesn't distinguish the first and third lines from the second). What
the first and third lines share is a pattern: a word followed by a
hyphen and itself. So we can grab those two lines by capturing a word
and repeating it (the file is named *doubles*):

    $ egrep ' ([a-z]+)-1' doubles
    This bell is a tam-tam.
    I want it quick-quick.

Puzzled? The regular expression, broken into pieces, is:

  ----------------------------------- -----------------------------------
   (                                   Start a group

   [a-z]+                            Any number (one or more) of
                                      letters

   )                                   Close the group

   -                                   Hyphen (a simple match)

  1                                 Repeat the group captured earlier
  ----------------------------------- -----------------------------------

The 1 is a special syntax recognized by tools that allow parentheses
to capture text. In the first line of the files, it matches "tam"
because that's what [a-z]+ matched. In the third line, it matches
"quick" because that's what [a-z]+ matched. It says, "whatever you
found, I want it again."

To extract the second part of an email address, such as "fsf.org" from
"[someone@fsf.org](https://web.archive.org/web/20160417194719/mailto:someone@fsf.org)",
use a regular expression such as:

    ([a-z._]+)@([a-z._]+)

In this case, 1 matches the part before the @ sign, while 2 matches
the part after the @. So extract 2 to get "fsf.org".

## Alternation

We saw that a character class matches only one character at a time. If
you have two or more sequences that can appear in the same place, you
specify them through *alternation*. This involves separating them with a
vertical bar (|). Thus, the following finds an instance of "FSF" or
"Free Software Foundation":

    FSF|Free Software Foundation

You can put as many alternatives as you want in alternation:

    gnu.org|FSF|Free Software Foundation

Because the alternatives are usually embedded in a larger regular
expression, you generally need to put them in parentheses to mark where
they start and end:

    The (FSF|Free Software Foundation)

## Anchoring

If you want to match something when it occurs at the beginning of the
line, but nowhere else, preface the regular expression with a caret
(^). Thus, you can use the following to catch lines that begin with a
lowercase letter:

    ^[a-z]

This use of the caret has nothing to do with the caret that we saw
before inside of square brackets. A caret means "beginning of line"
when it's the first character of a regular expression, but only in that
position.

Similarly, you can match something at the end of a line by adding a
dollar sign ($) to the end of the regular expression:

    [0-9]$

In the phrase "I added 3 and 5 to make 8", the previous regular
expression will match the 8 because it's at the end of the line.

When you're searching for lines that match a regular expression exactly
(no extra text at the start or end), use both anchors. For instance, if
you want to make sure a line consists only of digits, enter:

    ^[0-9]+$

The [0-9]+ part specifies "one or more digits", and the ^ and $
ensure that the regular expression takes up the whole line.

We wanted to take you just far enough to get a sense of what regular
expressions can do for you. There are many, many more features, but some
of them are useful only in programming languages. As we warned before,
different tools support different features, so you have to read the
documentation for egrep, Sed, or any other tool or language to find out
what works there.

When you're testing regular expressions, you can try lots of online or
stand-alone tools (some offered gratis and some for sale) that help
debug problems such as mismatched brackets and parentheses. Such tools
can help you learn the more complex features and make complex regular
expressions easier to write.
