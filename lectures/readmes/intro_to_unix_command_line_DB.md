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

#### Anatomy of a command

