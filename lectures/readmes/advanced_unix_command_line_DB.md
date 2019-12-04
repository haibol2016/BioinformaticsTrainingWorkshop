# Advanced Unix command line tutorial
---
Instructor: 	Derek M. Bickhart
Contact: 		derek **(dot)** bickhart **(at)** usda **(dot)** gov

## Table of Contents

## Sed, awk and grep text file manipulation

Oh! You're still here? I'm glad I haven't scared you off yet! Let me take this opportunity to tell you about the unholy trinity of tools that Unix provides for text and stream editing! But before I do that, let's talk a bit about **regular expressions**, which are ways to match patterns in text. Here are some examples of when you would need to use regular expressions (AKA: **RE**) in your daily routine:

*  You need to find all instances of the word, "Peabody," in a group of text files.
*  You wanted to find and replace all instances of the word, "irregardless," with "regardless" in a text file.
*  You wanted to count how many "A's" were in a sequence data file

Basically, *pattern matching*! In order to talk about regular expressions, we need to talk about **wildcards** which, just like in poker, can be any card they need to be! A wildcard can stand for any character or set of characters that is appropriate for a pattern match. Unix has a set of wildcards that can be used for matching patterns, and regular expressions have a different set of wildcards. This can be very confusing, but it will make more sense if we talk about each separately. For reference, here is a table with a side-by-side comparison.

| Wildcard type | Unix version | RE version |
| :-------------| :------------| :----------|
| Single character | **?**   | **.** |
| Zero or more characters | *  |  .*  |
| A set of specific characters | **[**(characters here)**]** | **[**(characters here)**]** |

#### Unix pattern matching

The reason why I want to discuss this first is because of two key facts:

1. You'll use Unix pattern matches very frequently on the command line
2. They're far more limited than RE patterns

As you saw in the table above, you can use three distinct sets of wildcards to select multiple files or patterns in Unix. 

```bash
# Uh oh! This is another comment! Remember these from the last lesson? They're baaaaaaack!
# To list all files in the current directory that end in ".gz"
ls *.gz

# To list a group of files that differ only in one character with "?"
ls file_number_?.txt

# To gzip only the ".txt" files that start with the letter "C" or "c"
gzip [Cc]*.txt

# Wildcards work with PATHs as well! Note that "cp" is smart enough to copy the name of the file in the target directory
cp ./*/*/*.gz ./gzip_directory/
```

So, there are some ways that you can select multiple files for common Unix utilities, but the functionality is very limited. This is where regular expressions come into play.

#### Regular expression (RE) pattern matching

RE are patterns that are meant to account for allot of common word structures used in English plain text. The format is so popular that it has spawned a multitude of books on the subject, and has wormed its way into many programming languages as a method for text manipulation. Unix gives you a couple of tools that help you implement your own RE patterns (joy!), so it is useful to know about the syntax. This is by no means exhaustive, but here is a table with some common RE symbols that are different from the table above:

| RE Character | Description  | Example | What the example does |
| :------------| :------------| :---------| :------------------|
| ^       | The start of a line | ^hat   | Finds lines that start with the characters "hat" |
| $       | The end of a line   | [a-z]$ | Finds lines that end with a letter (ie. for finding sentences without a period!)|
| \d      | A number (0-9)   | file\d | Finds any words that are equivalent to file0 through file9 |
| \D      | A non-number (Hah!) | file\D | Finds any words that are equivalent to fileA  but not file1 |
| \s      | A white space character | white\shouse | Finds "white house" with a space, newline or tab in the middle |
| \t      | A tab           | 1\t10 | Finds the specific set of characters: "1(tab)10" |

In order to introduce how to use RE to you, let's talk about the Unix tool, **grep**. Grep is pretty famous. If we had to rank its fame on the Hollywood scale, it would be right next to George Clooney and Angelina Jolie. It's so famous, that frequent Unix users use it as a common vernacular verb to describe a process by which you can extract something from something larger. Or... at least I hope I'm not the only one who does this!

Grep works by taking a regular expression pattern in argument #1 and applying it to a text file that is entered as argument #2. 

```bash
# Finding all lines in a text file that start with the word "root"
grep ^root list_of_files.txt

# Finding all lines in a file that contain the word "true"
grep true differential_expression.tab

# Finding all lines that contain a word that has one or more w's. Note the single quotes! More explanation below
grep 'w*' list_of_silly_words.txt
```

Because Unix and RE have competing wildcards, you sometimes need to **escape** your RE code to avoid Unix mistakenly interpreting the values. Think of it like when you see someone talking in a store, and you mistakenly think that they're talking to you, but they're on a hand's free bluetooth phone call. It's pretty awkward! To let Unix know that you're not talking to it, you should use **single quotes** for all of your grep RE patterns. The single quotes tell Unix: "nope, not talking to you!"

This goes without saying, but make sure you add the closing single quote afterwards! That's even more awkward when you use only one...

#### Stream editor (sed)

While not as famous as grep, **sed** -- or the "stream editor" command -- is still very, very useful. Sed allows you to edit your files via a "find and replace" style of formatting. This is not the only use of sed, but I don't plan on writing the authoritative use of the command in the limited space of this tutorial! 

Sed commands use a delimited style of input, where the first delimited text is the pattern you are matching and the second is the replacement text. The delimiter is typically a "/", but you can use underscores ("_") or colons (":") if you need to use a "/" in your pattern matching! 

```bash
# This is the same...
sed 's/pattern/replacement/' file

# ... as this
sed 's_pattern_replacement_' file
```
One more tip before we go too deeply in the woods. Remember above how I showed you several RE symbols that started with **backslashes** ("\")? The backslash is another "escape" character, but it escapes the next character immediately following itself. This is pretty often innocuous, but it has subtle meaning. For example a "\t" doesn't mean an escaped "t", but rather is the symbol for a (TAB)! The same with "\d", which means a single number in RE syntax. This means that if you want to use special symbols in your sed and grep RE patterns, you need to make sure they're escaped!

Here's where this is important:

```bash
# If you are replacing folder paths, you have to escape all of these slashes! Otherwise sed gets confused
sed 's/\/path\/to\/folder/\/new\/path\/folder/' file

# Using a different delimiter helps here! Note that the "/" isn't special because of the use of underscores
sed 's_/path/to/folder_/new/path/folder_' file
```

#### AWK (format editor)

Last but not least is the **AWK** scripting language. I am not going to devote enough time to AWK 

## Piping and redirection


## Shell scripting


## HPC job management using SLURM