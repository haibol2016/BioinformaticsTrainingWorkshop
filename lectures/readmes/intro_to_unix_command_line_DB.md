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

To exit the command line (preferably to make your hasty retreat after you just made on your boss's important files; yikes!), use the exit command:

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

Now, the novice reader is likely to be somewhat bewildered at this point. "How are we supposed to memorize this stuff," you may ask? Well, you don't have to! The Unix command line comes with some built-in help functions. In order to get the help menus for built-in Unix commands, you can usually just type in the **man** command (short for manual; not the male Homo sapien):

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

The later type of file is 