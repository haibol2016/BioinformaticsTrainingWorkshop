# Advanced Unix command line tutorial
---
Instructor: 	Derek M. Bickhart
Contact: 		derek **(dot)** bickhart **(at)** usda **(dot)** gov

## Table of Contents
* [Sed, awk and grep text file manipulation](#manip)
	* [Unix pattern matching](#pattern)
	* [Regular expression (RE) pattern matching](#re)
	* [Grabbing text (grep)](#grep)
	* [Stream editor (sed)](#sed)
	* [AWK (format editor)](#awk)
* [Piping and redirection](#piping)
	* [Pipes](#pipes)
* [Shell scripting](#scripting)
	* [For loops](#for)
	* [Conditional if statements](#if)
* [HPC job management using SLURM](#hpc)
	* [Interactive sessions with srun](#srun)
	* [SLURM job requirement settings](#jobreqs)
	* [Batch job submission with SBATCH](#sbatch)
	* [Managing cluster jobs](#managing)
* [Parting words](#parting)

<a name="manip"></a>
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

<a name="pattern"></a>
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

<a name="re"></a>
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

<a name="grep"></a>
#### Grabbing text (grep)

In order to introduce how to use RE to you, let's talk about the Unix tool, **grep**. Grep is pretty famous. If we had to rank its fame on the Hollywood scale, it would be right next to George Clooney and Angelina Jolie. It's so famous, that frequent Unix users use it as a common vernacular verb to describe a process by which you can extract something from something larger (e.g., "let me grep that out of the dataset"). Or... at least I hope I'm not the only one who does this!

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

<a name="sed"></a>
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

<a name="awk"></a>
#### AWK (format editor)

Last but not least is the **AWK** scripting language. I am not going to devote enough time to AWK to do it justice, but I will provide some basic commands to help you along. You could spend quite a while learning the [intricacies of AWK](https://www.gnu.org/software/gawk/manual/gawk.html), and there are several useful [tutorials](https://www.grymoire.com/Unix/Awk.html) online for the especially curious.

AWK divides text-delimited files (ie. tab or comma delimited values!) into columns that can be parsed with special **variables**. These variables are all preceeded by a dollar sign "$" and are numbered in the order of their appearance. For example, $1, $2 and $3 are the first, second and third columns. The $0 variable is special, in that it prints the whole line, delimiters and all! Because of this, the most frequent use of AWK in real-life situations is to change the format of a file:

```bash
# Print the first and third columns of a file, but swap them
awk '{print $3,$1}' my_tab_delimited.file

# Normally awk works with space or tab delimiters. What if you have a comma separated file? Use -F,
awk -F, '{print $1,$3,$5}' my_comma_separated_stuff.csv

# You can also specify preliminary values and variables by using a "BEGIN" statement
awk 'BEGIN{test="This is my special header sentence!"; print test} {print $0}' my_headerless_file.txt

# Finally, you can change the separator of files easily by using built-in variables to AWK. Such as a CSV (FS=",") to a tab delimited file (OFS="\t")
awk 'BEGIN{FS=","; OFS="\t"} {print $0}' file_to_convert.csv
```

Note the semicolons to separate variable assignment above! If you have separate statements, you must add a semicolon to let AWK know that you're done with one task before going to another. It's like saying "the end" after the long angry rant you directed at that family member you don't like very much. You know who I'm talking about...

```bash
# Final example: AWK allows you to filter output with an "if" statement
awk '{if ($1 > 15){print $0}}' my_gene_expression_data.tab

# This will only output lines that have a value that is greater than 15 in the first column
```

AWK is very extensible and is a scripting language in its own right! Several Bioinformaticians still use AWK scripting extensively because of its presence on every Unix system and it is a perfectly useful language. However, for all of the time you can spend learning AWK, I would highly recommend that you spend at least an equal amount of time learning a scripting language like [Perl](https://www.tutorialspoint.com/perl/index.htm) or [Python](https://docs.python.org/3/tutorial/) as these languages have extensive custom libraries for scientific analysis. 

Your time will not be wasted learning AWK, but Perl and Python are among the most predominant languages used in Bioinformatics presently. Being able to look at someone else's code and interpret it (or snicker as you think about ways to improve it! Muwahaha!) is invaluable. One of AWK's limitations is that its default usage expects some form of input in order to work. Perl and Python are more than happy to work (and break) on just air alone!

<a name="piping"></a>
## Piping and redirection

If you've been typing along with some of our commands (you have, haven't you? I don't mean to guilt trip you, but... ), you may have noticed that most of the commands print to the console terminal. So if you type an awk command, it prints text right underneath, like this:

```bash
awk 'BEGIN{test="Hey, I'm STDOUT!"} {print test}' empty_file.empty
	Hey, I'm STDOUT!
```

Text that is printed directly to your console terminal is a special sort of output, it is **STDOUT** (or Standard Output). You can think of it as being like a program that is trying to communicate with you. What would the terminal say if it had a voice? Most likely: "Syntax error!" or "Stop it, human scum!"

There are three special **streams** that you need to remember and recognize in Unix systems:

| Stream | Redirect |Description |
| :------|:---------|:-----------|
| STDOUT | >        |Output to the console window. |
| STDERR | 2>       |Output to the console window. Is a separate stream from STDOUT to allow for error messages |
| STDIN  | <        |Input from the keyboard **OR** STDOUT from a prior command! |

It's a bit confusing to have two "output" streams, but I hope to demonstrate shortly why this happens. Just for now, remember that **STDERR** (Standard Error) is a different animal than STDOUT. Also, please note that there is a way to input information into a file using **STDIN** (Standard Input)!

You may have also noticed the second column of that table, that gives the **Redirect** symbol for each data stream. This is very handy stuff! Let me demonstrate this with the **echo** command:

```bash
# "echo" prints any text following it to the STDOUT stream
echo "I'm printing out to your console! Wooo!"
	I'm printing out to your console! Wooo!

# But, let's throw echo for a loop and redirect the output:
echo "Now I'm text captured in a file... :(" > redirected_output

# Note that nothing printed out to the console this time! That's because of the redirect symbol ">"
# That symbol captured the STDOUT of echo and saved it in a file. Don't take my word for it! read that file!
head redirected_output
	Now I'm text captured in a file... :(
``` 

So, the redirect symbols capture or stream output/input out from or into a command. This means that you can save STDOUT and STDERR into a file, or you can read a file into a command using STDIN. Let's demonstrate!

```bash
# This is a really silly and redundant example, but hey...
# We're going to use STDIN to redirect a text file into cat, which will then print it to STDOUT
# But! In a twist that would make M. Night Shyamalan envious, we're going to capture the STDOUT into a file!
cat < my_text_file > copy_of_my_text_file

cat copy_of_my_text_file
# Should return the exact same text as the first file
```

Now that I've shown you a really silly example, let's see how this would work practically. Let's say that you want to process a sequence data file using a custom script that you've written. The script takes a sequence data [fastq](https://en.wikipedia.org/wiki/FASTQ_format) file via STDIN and prints a tab delimited list of GC% values and read names via STDOUT. Because you are a smart programmer, you built in a way to print error messages via STDERR so that they are saved DIFFERENTLY from your normal program output. 

Now, how would you run that script?

```bash
# First, make it executable!
chmod +x ./your_script.sh

# Legend:        1. STDIN fastq      2. STDOUT file     3. An error log. Will be empty if no errors!
./your_script.sh < flawed_file.fastq > gc_values.output 2> error.log

# Now to check to see if there are any error messages that require your attention
head error.log
``` 

Makes sense? You are taking advantage of all three streams in this example (you don't always have to do this!) by storing 2. normal output and 3. error messages while 1. reading in a file that already exists. Note that the redirect symbols "point" towards or away from your script. This makes it easier to remember what they do!

<a name="pipes"></a>
#### Pipes

Now that you've seen this in action, I want to show you one more Unix symbol that has special meaning, the **Pipe** ("|"). Unix commands are frequently built around a concept known as **modularity** -- or they are designed as individual components to "flow" together to create work flows. This means that a good Unix command prints to STDOUT and can read STDIN if needed. The pipe ("|") allows you to daisy-chain multiple unix commands together for the greater good. Let's demonstrate:

```bash
# Now that you're a redirect expert, let's start out by saving files to text! We want to find the 10 largest items in our current directory
# We start by printing the disk usage in human-readable format (-h) and saving to file
du -h > disk_usage

# Next, we sort the contents by human-readable sizes and save to a new file
sort -h < disk_usage > disk_usage.sorted

# Finally, we print only the 10 largest items
tail disk_usage.sorted

# Note that we had to save the STDOUT from each command to use the next one? This is where pipes fit in!
# Let's combine that into one command! Print the disk usage of a folder, in human-readable format (-h), sort the contents by human-readable size (-h) and print only the 10 largest items
du -h | sort -h | tail
``` 

Pretty cool, huh? And we didn't need all of those intermediary files that could clutter up our system! Any program that prints to STDOUT can be piped into another program that uses STDIN. Let's go back to your custom script (previous section) and use pipes in that example.

```bash
# Here was your original command:
./your_script.sh < flawed_file.fastq > gc_values.output 2> error.log

# Let's try this with a pipe, where we "cat" the fastq file in, instead of using the STDIN redirect
cat flawed_file.fastq | ./your_script.sh > gc_values.output 2> error.log

# Now, let's take this up a notch and filter the output so that we capture only the reads that have a GC% greater than 25%
cat flawed_file.fastq | ./your_script.sh 2> error.log | awk '{if ($2 > 0.25){print $0}}' > filtered_gc_values.output
```

Notice how we put an awk command as our last step? We can daisy-chain as many awk statements as we like, but in this instance we wanted to get the reads that were above a specific GC threshold. Pretty handy, huh? You can also use awk to reformat the file, use grep to find only specific reads, use sed to change values or simply take a peak at the results:

```bash
# Just a peak...
cat flawed_file.fastq | ./your_script.sh 2> error.log | head

# Now just counting the output lines...
cat flawed_file.fastq | ./your_script.sh 2> error.log | wc -l
``` 

Hopefully now you can see the power of piping! 

<a name="scripting"></a>
## Shell scripting

Before I release you into the wild, I need to train you in one final lesson on this topic: **shell scripting**. Basically, the process of creating workflows and pipelines using all of the unix tools and commands that I have been showing you from the beginning. Most of what you've been doing on the command line is **interactive** -- basically, you enter commands on the command prompt and wait until the command sends you results via STDOUT. This is all well and good when you have plenty of time on your hands and your commands only take three seconds to run, but some of us have day jobs! You'll have to use **scripts** -- basically pre-written commands -- to automate longer running analysis. 

The simplest Unix shell scripts run a series of commands in sequential order, just like we've been doing! The major difference is that you save these commands to a text file that is then run by a **shell** interpreter. Let's take a look at a text file that contains a shell script:

##### word_count.sh

```bash
#!/usr/bin/bash

wc -l $1
```

That's all there is to it! Let me show you how you can make this script yourself:

```bash
# First, you can use the nano text editor to write the file
nano word_count.sh

# However, since it's such a small script, you can even just use echo and redirect here!
echo -e '#!/usr/bin/bash\n\nwc -l $1' > word_count.sh

# And here's how you run it
chmod +x word_count.sh
./word_count.sh your_text_file.txt
```

So... what does the script do? Let's annotate the script (I'll use '##' to show new comment annotations) and break it down:

```bash
#!/usr/bin/bash
## The above statement is a "shebang." It tells Unix (and chmod) which interpreter to use to run this script
## In this case, we want Unix to use "bash" shell to interpret this script. This is pretty typical

wc -l $1
## OK, so you may have recognized "wc"  from our first lesson -- it's the word count command!
## The -l in wc just shows the line numbers
## The "$1" is new. This is just the shell script variable for arguments to the script. They are listed in numeric order just like AWK columns
## Example run of this script; ./word_count.sh your_text_file.txt
## "your_text_file.txt" is stored in the variable $1. A second argument (unused here!) would be in $2, and so on.
```

This example is very simple, but hopefully you have the basics down! You need 1) a shebang to show which shell interpreter you want to use and 2) a set of commands you want to run!

<a name="for"></a>
#### For loops

Shell scripts have control statements that allow you to direct the flow of programs beyond just simple sequential commands. The first concept I will introduce you to is the venerable **for loop**. A **"loop"** is a programming convention for an iterative task that continues until there is no more work, or there is a forceful exit! Loops are very handy for queuing up large tasks, and they follow this format:

```bash
# this is the general syntax for a Unix for loop (note the new lines!)
for i in stuff
do
	command $i
done

# You can compose this all on one line, but you'll need to use semicolons (just like with AWK, above!) to delimit the components of the loop
for i in stuff; do command $i; done
# This is the same as above!
```

So, what's going on here? You're taking a series of items, storing them in a variable ("i" or "$i") and running a task ("command") on each separate $i. The "stuff" field in the format above can be filled in several ways. You can simply add space delimited words, the output of a program, or data from a file. Let's demonstrate this with a few examples.

```bash
# Lets start with a list of text files in a directory
for i in text_file1.txt text_file2.txt text_file3.txt
do
	echo $i
	cat $i | wc -l	 
done

# We can use Unix wildcards to make this easier!
for i in text_file*.txt
do 
	echo $i
	cat $i | wc -l
done

# But, what if wild cards aren't applicable? We can actually use a combination of things
# Below, I'm showing "backtics" which capture the STDOUT of a unix command in the first portion
# You can use backticks for "cat", "ls" or any other command here!
for i in `ls file*.txt` another.txt yet_another.txt finally*.txt
do
	echo $i
	cat $i | wc -l
done
```

For loops are most effective when you can use them to queue up multiple successive tasks!

<a name="if"></a>
#### Conditional if statements

Finally, you might want some control over what you're doing in your script! Thankfully, the Unix **if-then-else** statements allow you to check to see if a statement is "true" or "false" and then do something about it! Unix's definition of "truth" may be a bit different than yours though:

* Truth is a non-empty series of text, the number "0" or the "**true**" command
* False is the number "1" or the "**false**" command

Let's demonstrate this using the syntax for the if statement:

```bash
if true
then
	echo "This prints!"
fi
```

Your if statements need to have a "then" statement and must be flanked by a "fi" terminator. Anything between those two words will execute if (and only "if!" Get it?) the statement is true. Let's see how this would work in a practical scenario:

```bash
# Remember your super script above? Let's check to see if there's anything in that error log
if grep 'ERROR' error.log
then
	echo "Hey dude, there's an error! Stop the presses!"
	exit(-1)
fi
```

It's far better for your shell scripts to "**fail fast**" (or end as soon as they encounter an error) than to keep plugging away after an error is encountered! You can also check to see if files exist, variables are the right value or many other settings are proper using the Unix **test** command. For ease of use, Unix packages both the word "test" and a syntax that uses square brackets (**[]**). The later is more frequently used in shell scripts because of how easy it is to remember and because it looks much nicer.

```bash
# Test to see if an output file exists
if test -s full_file.txt
then
	echo "Yes! It liiiiives!"
fi

# This is the same as above! Notice the brackets that surround the test
if [ -s full_file.txt ]
then
	echo "Yes! It Liiiiives!"
fi
```
Unix test can check allot of conditions! The "-s" condition tests to see if a file exists and is not empty, and there are many more that are useful for checking on the contents of files. Use "man test" to see all of the possible conditional arguments!

Just in case you want to do something when a test fails, you can add additional conditionals (**elif**) or a default command to run if everything fails (**else**). Here's a slightly more complex example:

```bash
if [ -s full_file.txt ]
then
	echo "File exists!"
elif [ -f full_file.txt ]
then
	# This runs if the file is present, but is empty!
	echo "File exists, but doesn't have any contents!"
else
	# This only runs if everything failed!
	echo "Oh boy! File doesn't exist! Uh oh!"
	exit(-1)
fi
```
These statements are very useful when combined in scripts that produce temporary output files. If the files aren't made by a previous script, it's best to check that with an "if" statement and then print an error so that you know as soon as possible!

<a name="hpc"></a>
## HPC job management using SLURM

Now that you're all certified script wizards, let's talk about how to use some real hardware! Most modern computing takes place on **high performance computing clusters** (or **HPCs**) because of how much cheaper and efficient it is to chain several servers (termed "**nodes**") together than to make one massive server with billions of CPU cores and Petabytes of RAM! Since these servers run tens or hundreds of separate computers all linked together, you can't just run interactive Unix commands on the command prompt as you've been doing this far! No, you need a new strategy and some help by a **job scheduler tool**. 

The job scheduler that I am going to teach you to use today is called **SLURM** (Simple Linux Utility for Resource Management). The person who came up with that acronym deserves a huge raise. SLURM comes with several tools designed to help you submit jobs to separate computers on the same system and to monitor them as they run. Here are a few commands that are useful for working with SLURM:

| Command | Description |
|:--------| :-----------|
| sinfo   | Shows the nodes and paritions that are available for use. |
| squeue  | Shows the currently queued tasks on the cluster   |
| scancel (jobId) | Cancels a job (use squeue to find the jobId. Don't try to cancel your friends' jobs!) |
| sbatch  | Submit a non-interactive job |
| srun    | Request an interactive job session |

The last two commands actually send jobs to the system, so let's discuss them in greater detail.

<a name="srun"></a>
#### Interactive sessions with srun

The **srun** command queues up an interactive Unix session on a node so that you can enter Unix commands to your heart's content. This is very similar to what you've been doing up until now -- running interactive sessions on the command line. Now, you might ask, "why would we want to queue up an interactive session? Aren't we already doing that?" To which I would respond, what a great question! Next question?

There are a handful of reasons why we would want to queue up an interactive session with srun:

1. Most HPCs are set up with one "head node" where everyone logs into the system. This node is responsible for handling logins and job scheduling, so if someone is running a very memory intensive task on that node, it could interrupt normal user logins! Separate interactive sessions allow you and the head  node to work in peace by separating tasks on other hardware

2. If you know the exact amount of computational resources that your job will require, you can set up a workspace that has precisely that amount of allocated RAM and CPU cores. This is a very efficient partitioning of the system and allows you to avoid colliding with other users. It's so efficient, you might even reduce your carbon footprint!

3. Because I said so!

Srun takes several arguments that specify the exact server/node resources that you want for your job. Many of these will be optional based on your server administrators' requirements, but here are a few common arguments and some common settings for each:

<a name="jobreqs"></a>
##### SLURM job requirement settings

| Argument | Abbrev | Common Setting | Description |
| :--------|:-----|:---------------|:------------|
| --nodes= | -N	  | 1             | The number of nodes that your job will use. Unless you use MPI, this will be one |
| --ntasks-per-node= | -n | 1    | The number of CPU cores (specifically, "tasks") that your job will use. Typically only 1! |
| --mem=  | (none) | 1000  | The number of megabytes of RAM your job will use. Depends on the job! |
| --partition= | -p | (depends on server) | The set of nodes that will be selected for your job. Totally depends on the server admin! Some jobs have dedicated nodes, like "GPU" or "HighMem" |
| --time= | -t  | 1-0 | The time it will take your job to run. Formats can be in "minutes", " minutes:seconds", "hours: minutes:seconds" or "days-hours." Depends on your job!|
| --job-name= | -J | "Bob" | The name of your job. Totally cosmetic, but you should think about using something that will make your System Admin smile. Try their name! Or their ex-spouse's name! |
| --output= | -o  | %x-%j.out | The STDOUT of your job. Important! Slurm creates a "slurm-xxxx.out" file for you automatically if you don't specify this |
| --error= | -e  | %x-%j.err | The STDERR of your job. Again, Important! Slurm will add the STDERR to your "output" STDOUT file if not specified. |


As I mentioned above, several of these are not required, so here is a perfectly reasonable way to queue up an interactive session on your pocket cluster:

```bash
# Uses 1 node, 16 Gbs of RAM, expects two tasks (cpus) and runs on the short partition
srun --nodes=1 --mem=16000 --ntasks-per-node=2 -p short --pty bash
```

**Just memorize this:** the "--pty bash" portion is required for your interactive session, otherwise you will automatically exit the session as soon as it is given the resources it needs!

Now that you know how to queue up interactive sessions, let's talk about submitting jobs to run in the background!

<a name="sbatch"></a>
#### Batch job submission with SBATCH

In order to submit jobs to run on different nodes, you will need to use a different SLURM command called **sbatch**. The good news is that sbatch takes many of the same commands as srun, but instead of queuing up an interactive shell for you to futz around with, sbatch will clap the dust off of its hands and let that job run unhindered on a different node. The sudden loss of control over your job is something that is a bit scary, but once you have your pipeline down, this becomes a very useful thing to rely on! The problem with interactive sessions is that you can only focus your attention on one task at a time! Let me demonstrate:

```bash
# You need to gzip all of the text files in a directory
for i in *.txt
do
	echo $i
	gzip $i
done
```

A million years later, your task will complete and you will be able to do work again! This is a very inefficient way to work on an HPC, though! This is where SBATCH can come in handy. If you take your for loop and write it to a shell script (using the nano text editor) that includes the '#!/usr/bin/bash' shebang, sbatch will take care of the rest!

```bash
# Assuming that you took the time to write this all down to a script...
for i in *.txt
do
	sbatch --nodes=1 --mem=500 --ntasks-per-node=1 -p short gzip_script.sh $i
done
```

You will get a ton of messages for each file, stating that the job was submitted! Good work! Now all of the jobs will be running separately on the cluster and you will be able to do other work! No rest for the weary...

Note that the sbatch command takes the same required resource arguments as srun did, [above](#jobreqs). Also, each run of sbatch will start a STDOUT output file. If you have allot of files to gzip, you'll also have allot of these in your folder! Since gzip is such a simple process, it may be best to remove clutter by deleting them, but be cautious of this for more complex tasks in the future.

There is more than one way to skin a cat though! Let's try the same task, but this time we will make use of the **wrap** argument to sbatch that allows us to write an anonymous bash script for each task:

```bash
# the --wrap= argument allows us to pass in a shell command that will run in the background. Very handy for simple tasks!
for i in *.txt
do
	sbatch --nodes=1 --mem=500 --ntasks-per-node=1 -p short --wrap="gzip $i"
done
```

That saves us from having to write a script for every job we want to run, but what about routine tasks that we need to run frequently? Or pipelines? If we know the resource requirements, we can actually write them in the script text so that sbatch knows what resources are needed. You can preface these with a special comment that looks like this, "#SBATCH". Let me demonstrate with this example script text file below:

```bash
#!/usr/bin/bash
# you still need the shebang!
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --mem=500
#SBATCH -p short

# Remember that $1 is the first argument to a shell script!
gzip $1
```

Now, your example for loop would look like this:

```bash
for i in *.txt
do
	sbatch gzip_script.sh $i
done
```

Much less typing! 

<a name="managing"></a>
#### Managing cluster jobs

Now that you've submitted hundreds of jobs and they're all causing your system administrator's blood pressure to start rising, let's talk about how you can monitor them and cancel them (if need be!). This is perhaps the easiest part of this tutorial, and -- if you're obsessive like I am -- you'll probably use some of these tools very frequently.

The first (and most frequently used) command will be **squeue**. Squeue shows you all of the currently queued and running jobs on the cluster in a list format. It will only show you jobs that are running on partitions that you can use, but without any arguments, it will show you everything that will be running or is currently running!

```bash
# The default command will show you everything running on all of the partitions you can see
squeue

# You can specify that you want to see only your own jobs with the "-u" argument
squeue -u your.name

# Remember our good friend, the pipe? Use pipes to reformat this list to your liking!
squeue -u your.name | grep 'cluster1'
```

The second command is **sinfo** which is used to list cluster partitions and their availability. Sinfo gives you hints as to which partitions are available for your use, and which are completely full! Here's an example:

```bash
# The default use shows everything
sinfo
	   PARTITION AVAIL TIMELIMIT NODES STATE  NODELIST
       batch     up     infinite     2 alloc  adev[8-9]
       batch     up     infinite     6 idle   adev[10-15]
       debug*    up        30:00     8 idle   adev[0-7]

# key:  1.       2.        3.       4.  5.     6.
```

The information is partitioned according to the key above:

	1. The name of the partition. Specify this in your srun and sbatch commands with the '-p' argument
	2. Availability of the cluster. "up" is good! "down" is bad!
	3. The length of time allowed for jobs on this queue. Jobs set to run longer than this will never run!
	4. The number of nodes that are on this partition.
	5. This is important: The nodes can have several different states ranging from "down", to "drain" (about to go down), "idle" (ready for use), "mix" (being used, but still some resources available) to "alloc" (completely allocated and full!). Use this in planning when to run your jobs!
	6. This shows you the names of the nodes on this list. Numbers in brackets are meant to be sequential, so adev0, adev1 ... adev7 are all on the debug partition.

Always check the partitions to see what is available!

<a name="parting"></a>
## Parting words

Now you know enough to be dangerous. The best way to continue learning these materials is to practice constantly! Get out there and start typing your scripts. If you run into any trouble, use many of the bolded keywords in these tutorials as the "seeds" of your future Google searches. There are lots of people who have probably raised the same questions that you have!

May your scripts never give syntax errors!