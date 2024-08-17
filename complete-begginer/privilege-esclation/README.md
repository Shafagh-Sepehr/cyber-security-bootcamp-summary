---
icon: escalator
---

# privilege esclation

## Enumeration

LinEnum is a bash script that automates privilege escalation commands, saving time for root access. Understanding its commands is essential for manual privesc vulnerability identification.

{% embed url="https://github.com/rebootuser/LinEnum/blob/master/LinEnum.sh" %}

```
-k Enter keyword
-e Enter export location
-t Include thorough (lengthy) tests
-r Enter report name
-h Displays this help text
```

### How do I get LinEnum on the target machine?

There are two ways to get LinEnum on the target machine. The first way, is to go to the directory that you have your local copy of LinEnum stored in, and start a Python web server using "python3 -m http.server 8000" \[1]. Then using "wget" on the target machine, and your local IP, you can grab the file from your local machine \[2]. Then make the file executable using the command "chmod +x FILENAME.sh".

#### Other Methods

In case you're unable to transport the file, you can also, if you have sufficient permissions, copy the raw LinEnum code from your local machine \[1] and paste it into a new file on the target, using Vi or Nano \[2]. Once you've done this, you can save the file with the ".sh" extension. Then make the file executable using the command "chmod +x FILENAME.sh". You now have now made your own executable copy of the LinEnum script on the target machine!

### Understanding LinEnum Output

The LinEnum output is broken down into different sections, these are the main sections that we will focus on:

_**Kernel**:_ Kernel information is shown here. There is most likely a kernel exploit available for this machine.

_**Can we read/write sensitive files**:_ The world-writable files are shown below. These are the files that any authenticated user can read and write to. By looking at the permissions of these sensitive files, we can see where there is misconfiguration that allows users who shouldn't usually be able to, to be able to write to sensitive files.

_**SUID Files**:_ The output for SUID files is shown here. There are a few interesting items that we will definitely look into as a way to escalate privileges. SUID (Set owner User ID up on execution) is a special type of file permissions given to a file. It allows the file to run with permissions of whoever the owner is. If this is root, it runs with root permissions. It can allow us to escalate privileges.&#x20;

_**Crontab**_ _**Contents**:_ The scheduled cron jobs are shown below. Cron is used to schedule commands at a specific time. These scheduled commands or tasks are known as “cron jobs”. Related to this is the crontab command which creates a crontab file containing commands and instructions for the cron daemon to execute. There is certainly enough information to warrant attempting to exploit Cronjobs here.

## Abusing SUID/GUID Files

<details>

<summary>What is an SUID binary?</summary>

As we all know in Linux everything is a file, including directories and devices which have permissions to allow or restrict three operations i.e. read/write/execute. So when you set permission for any file, you should be aware of the Linux users to whom you allow or restrict all three permissions. Take a look at the following demonstration of how maximum privileges (rwx-rwx-rwx) look:

r = read

w = write

x = execute\


&#x20;   user     group     others\


&#x20;   rwx       rwx       rwx\


&#x20;   _421       421       421_

The maximum number of bit that can be used to set permission for each user is 7, which is a combination of read (4) write (2) and execute (1) operation. For example, if you set permissions using "chmod" as 755, then it will be: rwxr-xr-x.

\
But when special permission is given to each user it becomes SUID or SGID. When extra bit **“4”** is set to user(Owner) it becomes **SUID** (Set user ID) and when bit **“2”** is set to group it becomes **SGID** (Set Group ID).\


Therefore, the permissions to look for when looking for SUID is:

SUID:

rws-rwx-rwx (chmod 4777)

GUID:

rwx-rws-rwx (chmod 2777)

</details>

### Finding SUID Binaries

We already know that there is SUID capable files on the system, thanks to our LinEnum scan. However, if we want to do this manually we can use the command: "`find / -perm -u=s -type f 2>/dev/null`" to search the file system for SUID/GUID files. Let's break down this command.

`find` - Initiates the "find" command

`/` - Searches the whole file system

`-perm` - searches for files with specific permissions

`-u=s` - Any of the permission bits _mode_ are set for the file. Symbolic modes are accepted in this form

`-type f` - Only search for files

`2>/dev/null` - Suppresses errors
