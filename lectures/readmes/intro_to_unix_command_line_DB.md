# Introduction to the Unix Command Line
---
Instructor: 	Derek M. Bickhart
Contact: 		derek **(dot)** bickhart **(at)** usda **(dot)** gov

## Table of Contents


## What is Unix? What am I doing here? Help?

The term, "Unix," refers to a family of operating systems that all have the same basic elements:

* They are largely driven by command line commands
* They have specific input and output streams
* They treat almost everything as a "file" 

There are more standards, but these are likely to be the most important for now! You can find Unix-based operating systems silently working underneath your Mac OS and your favorite flavor of Linux. Because Unix comes with many very useful (and open source) commands for analysis, programming and file management, it is the preferred operating system for many scientific computing tasks.

You've been used to using a graphical user interface (**GUI**) if you have been working on Windows or Mac PCs, but now we're going to run things on the **command line**! While GUIs are very useful for singular tasks, they tend to fail when you need to run lots of repetitive tasks or deal with huge data files! This is where knowing how to use the command line is very useful!

My goal in this lecture is to get you familiar with basic Unix commands and how to navigate the command line. I will try to **"bold"** computer science concepts and commands throughout this text for your reference. In order to reinforce these lessons, I tell jokes and force you to type commands in the command line. I have been told that I have a very strange sense of humor -- you've been warned!

## Connecting to the command line terminal

If you are working on a Windows, Mac or Unix PC, you will first need to open a compatible terminal window. On Windows, you can now use the [Windows Powershell](https://docs.microsoft.com/en-us/powershell/scripting/getting-started/getting-started-with-windows-powershell?view=powershell-6). For Mac and Linux machines, you need to hit the hotkeys for the [Mac terminal](https://www.idownloadblog.com/2019/04/19/ways-open-terminal-mac/) or **Control-T** for most Linux distributions. You should be greeted by a happy dollar sign: **"$"**. You're in the money! 

If you're on Mac or Linux, you should have access to the entire panoply of Unix command already! Congratulations! But before you blow up your laptop, it may make sense to connect to another server. To do this, you use the **ssh** command (replace values in the parentheses with your assigned username and server ip address!):

```bash
ssh (your username)@(the server ip address)
```

Just to keep things clear, I will use **comments** to explain concepts on the command line and to antagonize you. Comments are any line prefaced by a '#' sign, and you can type these into the command line as well!

```bash
# This is a comment! It doesn't do anything when run!
# Anything after a '#' is not interpreted
# I will use these frequently! Feel free to ignore them like Unix does!
```

To exit the command line (preferably to make your hasty retreat after you just deleted your boss's important files; yikes!), use the exit command:

```bash
exit
```


## Navigating the command line

One of the issues with the command line is that you're probably used to nice windows and graphical icons to help you navigate your computer's file systems. Well, navigating the command line is a different paradigm, as you don't have any graphical boxes to check! You should think of navigating the command line as being similar to feeling your way around in a dark room -- you have to constantly feel objects around you to get your bearings. Once you get the hang of things, it will come naturally to you!

Unix gives you **commands** that allow you to navigate around on the command line! Commands are executable files that perform a task for you. 

Let's start with the most important command: 

```bash
ls			# prints a list of the files in your current directory
ls /usr/	# prints the files in a specific directory (space delimited)
```

You will be typing **ls** (an abbreviation for *"list"*) very frequently! The paint on the "l" key on my keyboard is almost completely rubbed away by now, and it wasn't from frequently typing "leonidas", "leopard" or "lulu!"

We mentioned that "ls" could print from your current directory, but it doesn't give you a sense of "where" that is! In order to get your current directory, you use **pwd** (**p**rint **w**orking **d**irectory):

```bash
pwd 	# You should get something like: /home/user/(yourname)/
```

Note the "/" (forward slash) delimiters in the directory **path**, these slashes indicate the subfolders to your files, with the beginning "/" indicating the **root directory**. Unlike Windows, which uses "Drives" (like the C: drive), all files original from the root directory in Linux. Try looking at it now:

```bash
ls /	# You'll get a bunch of folders like /home, /bin, /usr
```
Theoretically, you can trace a path from the root directory to any file in the system, even very long ones!

The ls command is very flexible and can be used to search all folders in your relative location in the file system.

```bash
ls      # This prints the files in the current directory
ls ./   # This produces the same thing as the ls command immediately above it!
ls ../  # This prints the directory immediately above the present one
```

In order to get different views of the files in your directories, we're going to have to talk about adding special options to "ls" and other commands.

#### Anatomy of a command

Unix has a pretty straightforward "grammar" for commands (aka: **syntax**). You can think of each "sentence" of a Unix command being as follows:

```bash
(the command) (argument1) (argument2) ... (argumentN)
```

You may have noticed that I have these parenthetical notations called **arguments** above. Please note that I'm not talking about "spirited discussions between two people" but extra values that you can pass to commands. These are split into two different categories:

1. **Options**: Arguments that take a value
2. **Flags**: An argument that does not take a value (ie. like a switch it is either "on" or "off")

The "ls" command has several arguments, many of which are very, very useful! Here's a short list of some of the many useful "ls" arguments and what they do:

| Argument | Type  | Description  |
|:---------|:------|:------------ |
|-a        | Flag  | Shows hidden files|
|-l        | Flag  | Displays files in list format |
|--format=long| Option | The same as "-l" |
|-t        | Flag  | Sorts files by time of creation |
|-h -a -l  | Flag  | A Combination of -a and -l, but make the file sizes human-readable |
|-hal      | Flag  | The same as the previous command! Just shorter! (I'm sorry Dave, I'm afraid I can't do that) |

#### Getting the help you need

Now, the novice reader is likely to be somewhat bewildered at this point. "How are we supposed to memorize this stuff," you may ask? Well, you don't have to! The Unix command line comes with some built-in help functions. In order to get the help menus for built-in Unix commands, you can usually just type in the **man** command (short for manual; not the male of the Homo sapiens species):

```bash
man ls		# You can navigate the man page using the arrow keys on your keyboard!
			  # the "q" key exits the man page
```

Most commands are installed with a man page (the output of the man command), even the man command itself!

```bash
man man 	# avoid the urge to sing the theme song to "Two and a Half Men"
```

Even if your command doesn't have a man page, you can usually get a usage or help statement by doing one of the following tricks:

* Run the command without arguments
* Run the command with -h
* Run the command with --help
* Post a hashtag of the command on twitter (#ls)

#### Final lessons on orientation

Now that you're getting the help you need (poor thing), let's finish up this section by talking about moving around in Unix folders and getting your bearings. We already talked about "pwd" to get your current directory and "ls" for listing file contents, but how do you actually go to a different folder? How do you even MAKE a new folder? What IS a folder? Let's ignore that last question and move ahead by talking about the **cd** (**c**hange **d**irectory) command:

```bash
cd folder				# changes your current directory to the folder
cd /path/to/folder		# Just like the the above, but links to the full path
cd ..					# Changes your current directory to the folder directly above this one 
cd .					# Changes your current directory to... the current directory! 
```

That last command illustrates an interesting point: Unix has some short cut **aliases** (or synonyms) for common directories that you may use. Here are the common shortcuts that you will use frequently to maneuver around the command line:

| Shortcut | Description |
|:---------|:------------|
|**.**		   |The same folder. Useful when you need to run a script or command that is present in your current directory! |
|**..**	       |The previous folder. Very helpful to go back a step! |
|**~**	   |Your home folder |

Please note that Unix is **case-sensitive!** Unix is also sensitive to spaces, so please be careful when naming your folders! This is the reason why you are likely to see more underscores ("_") in file and folder names while working with Unix. 

Finally, I have a tip to share that will stop you from developing carpal tunnel, improve your relationships and make you lots of money: use the [TAB] key! Unix allows you to auto-complete entries using [TAB] so long as you match enough characters in a file. Here's an example:

```bash
# You run an ls in the following directory and here are the files:
ls
	directory	file1	file2

# To change to the directory, you can use tab completion! Type the following:
cd dir[TAB]
# Unix will automatically fill the word to read:
cd directory/
# Handy, huh? That saved you 1 calorie!
```

## Assessing and viewing text files

In this section, we will talk about several common Unix commands that can help you view text files. If you remember from my rant above, Unix treats everything as a file. There are three distinct files on Unix systems that you need to concern yourself with at this point:

1. Directories
2. Binary files (you can't read these with text editors easily!)
3. Text files

The later type of file is going to be the main workhorse of most of your analysis! Finding, viewing and manipulating huge batches of large text files is going to be your bread-and-butter task on Unix systems. Thankfully, Unix comes with many, many tools that are useful for text file manipulation!

#### Exploratory text viewing

There are several commands that allow you to view portions of text files. Sometimes your text files will be millions of lines long! Microsoft Word can no longer help you with this! To view portions of a text file without blowing up your terminal session, you can use two very useful commands, **head** and **tail**:

```bash
# To view the first 10 lines of a text file:
head my_text_file.txt

# and to view the last 10 lines! 
tail my_text_file.txt

# Both head and tail can show more lines if you enter the following options:
head -n 20 my_text_file.txt
 (shows the first 20 lines instead of 10!)
```

If your file is short enough to print the entire batch to the console, or you want to combine the output of multiple files, you can use the **cat** command:

```bash
cat my_text_file.txt
 (prints the whole file to the screen)

# Note, the following command takes as many txt files as you can hand it! It will print them in the order they are listed
cat i_am_printed_first.txt i_am_printed_second.txt i_am_printed_last.txt
```

#### NOTE: Stopping any long-running process on the terminal:
---
Important note, if you accidentally "cat" a very long file, you could be staring a text scrolling by the screen for quite a while! Unlike in the Matrix, you won't be able to pick out people's haircolor from the scrolling text, so you'll be twiddling your thumbs for no good reason! If you get in this scenario, Unix gives you a special command to escape: **[control]-C**.

Just hold down the [control] key and press the "c" key (it does not need to be capital case!). That will terminate the running task on your current terminal session. 

---

You can view a file in a more orderly fashion by using the **more** and **less** commands. Believe it or not, but the old axiom, "less is more," applies here, as the less command gives you more control over reading files. Who'd have thought? Still, both commands can be useful but you need to know how to control them. In both cases, you can exit the commands by typing "q" (just the letter on the keyboard). Remember this!

The big difference between the two commands is that you can scroll back up the file using less, whereas you can only scroll "down" with more. Let's demonstrate:

```bash
# You can only scroll "down" with more by using the "enter" key:
more text.txt

# You can scroll with the arrow keys by using less instead:
less text.txt

# To exit either command, hit this key as hard as you can:
q
```

Finally, let's talk about how to edit files directly on the cluster. For this, you can use one of several text editors, though the **nano** editor is best for beginners. Nano will open text files (if they exist) or allow you to create new files via the following commands:

```bash
# Open existing file for editing
nano secret_accounts.tab

# Create new file (file is actually only created after you save the text you enter!)
nano wannabe_secret_accounts.tab
```

To save your text, hold [control] and hit the "w" key. Nano will ask you if you want to save the file as the same name, to which you press "enter" to confirm. To exit the Nano editor, hold [control] and hit the "x" key. All of these commands are actually written for you at the bottom of the text editor window! How handy! The commands above are listed as "^W" and "^X", respectively, with the "^" being a proxy for the control key!

We've covered allot of ground with the text editting functions, so here's a handy table to keep them all straight:

#### Common text viewing options on Unix

| Command  | Common Uses  |
|:---------|:-------------|
| head     | To view the first (10) lines of a file |
| tail     | To view the last (10) lines of a file  |
| cat      | To combine two (or more!) files or to print one or more of them all out to the terminal |
| more     | To scroll through a file's text using enter |
| less     | To scroll through a file's text, but with more control (and with arrow keys!) |


## File permissions and tasks


## Miscellaneous commands