# Important Directories

## /etc

**One of the most important**

Short for etcetera

A commonplace location to store system files that are used by OS.

`sudoers` file contains a list of the users & groups that have permission to run sudo or a set of commands as the root user.

Also "passwd" and "shadow" files. These two files are special for Linux as they show how your system stores the passwords for each user in encrypted formatting called sha512.

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

## /var

Short for variable data

Stores data that is frequently accessed or written by services or applications running on the system.

E.g. log files from running services and applications are written here (/var/log), or other data that is not necessarily associated with a specific user (i.e., databases and the like).

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

## /var/log

These files and folders contain logging information for applications and services running on your system.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Logs for services such as a web server contain information about every single request - allowing developers or administrators to diagnose performance issues or investigate an intruder's activity.

E.g. the two types of log files below that are of interest:

* access log
* error log

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

## /root

The /root folder is actually the home for the "root" system user.

There isn't anything more to this folder.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

## /tmp

Short for temporary.

the /tmp directory is volatile and is used to store data that is only needed to be accessed once or twice.

Similar to the memory on your computer, once the computer is restarted, the contents of this folder are cleared out.

What's useful for us in pentesting is that any user can write to this folder by default. Meaning once we have access to a machine, it serves as a good place to store things like our enumeration scripts.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>
