# Linux Fundamentals

## A Bit of Background on Linux

### Where is Linux used?

Linux is much lighter than other OS's and its vastly used in different systems and computers such as:

* Websites (their servers)
* Car entertainment/control panels
* Point of Sale systems such as registers in shops
* Critical infrastructures such as medical systems and devices or traffic light controllers or industrial sensors

### different kinds of Linux

"Linux" is actually a name for all OS's that are based on UNIX (another OS).

Linux being open-source caused the creation of multiple other Linux variants in all sizes for every possible use case.

For example, Linux and Debian are one of the most common distros because of their extensibility. you can run ubuntu as a server or as a desktop OS.

## Running a few Commands

The lightweight-ness of Linux isn't disadvantge-free as some of them don't come with a GUI (graphical user interface), or what is also known as a desktop environment that we can use to interact with the machine. A large part of interacting with these systems are through using the "Terminal".

It's completely text-based and there are commands to do basic functions such as making, writing to and deleting a file or making and removing directories.

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption><p>terminal appereance</p></figcaption></figure>

Here are the first two commands:

<table><thead><tr><th width="284">Command</th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>echo</td><td>Output any text that we provide.</td><td></td></tr><tr><td>whoami</td><td>Find out what user we're currently logged in as.</td><td></td></tr></tbody></table>



### Interacting With the Filesystem

<table><thead><tr><th width="229">Command</th><th>Full Name</th></tr></thead><tbody><tr><td>ls</td><td>listing a directory's content</td></tr><tr><td>cd</td><td>change directory</td></tr><tr><td>cat</td><td>concatenate (printing a files content to the terminal)</td></tr><tr><td>pwd</td><td>print working directory</td></tr></tbody></table>

### Listing Files in Current Directory (ls)

To know what exists in the current directory we use "ls" command (short for listing)

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption><p>ls command</p></figcaption></figure>

In the screenshot above, we can see there are the following folders:

* Important Files
* My Documents
* Notes
* Pictures

We can also run "ls" on a folder without navigating to it with providing the relative or complete path to it after the "ls" command. e.g. "ls Notes"

### Changing Current Directory (cd)

"cd" is short for **c**hange **d**irectory and is self explanatory. If we want to go to Notes folder we execute "cd Notes".

### Outputting the Contents of a File (cat)

"cat" is short for concatenating and is a great way to output the contents of files (not just text files).

<figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption><p>cat command</p></figcaption></figure>

We can also run "cat" on a file that is not in current directory with providing the relative or complete path to it. e.g. "cat folder/file"

### Finding out the full Path to Current Working Directory (pwd)

It's easy to lose track of where we are in the file system. by using the pwd command we get the full path to current working directory.

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption><p>pwd command</p></figcaption></figure>

## Searching for Files

#### **Using Find**

The "find" command is fantastic in the sense that it can be used both very simply or rather complex depending upon what it is you want to do exactly.

In the snippet below, we can see a list of directories available to us:

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

Directories can hold more directories within them along with files. So, it could be very hard to find what we want. We can use the "find" command to facilitate this.

Let's assume that we know the name of the file we're looking for but forgot where it is. e.g. we're looking for "passwords.txt".

If we remember the filename, we can simply use `find -name passwords.txt` where the command will look through every folder in our current directory for that specific file.

<figure><img src="../.gitbook/assets/image (33).png" alt=""><figcaption><p>find command</p></figcaption></figure>

"find" has managed to find the file we where looking for. But let's say that we don't know the name of the file or want to search for every file that has an extension such as ".txt".

We can use a wildcard(\*) to search for what we want to find. the command needed to run is `find -name *.txt`.

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption><p>find command with wildcard</p></figcaption></figure>

The command has found two files with .txt extention.

#### **Using Grep**

The great command of "grep" allows us to search within a file, for example looking for an IP address in server logs.

We can use `grep` to search the entire contents of this file for any entries of the value that we are searching for. for example we'll search a web server's access log, we want to see everything that the IP address "81.143.211.90" has visited.

<figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

"grep" has searched through this file and has shown us any assurance of the IP.

## An Introduction to Shell Operators

a way to work more efficient and faster with the terminal is the use of shell operators.

<table><thead><tr><th width="192">Symbol / Operator</th><th>Description</th></tr></thead><tbody><tr><td><code>&#x26;</code></td><td>This operator allows you to run commands in the background of your terminal.</td></tr><tr><td><code>&#x26;&#x26;</code></td><td>This operator allows you to combine multiple commands together in one line of your terminal.</td></tr><tr><td><code>></code></td><td>This operator is a redirector - meaning that we can take the output from a command (such as using cat to output a file) and direct it elsewhere.</td></tr><tr><td><code>>></code></td><td>This operator does the same function of the <code>></code> operator but appends the output rather than replacing (meaning nothing is overwritten).</td></tr></tbody></table>

