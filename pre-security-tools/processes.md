# ▶️ processes

We can use the friendly `ps` command to provide a list of the running processes as our user's session and some additional information.

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

To see the processes run by other users and those that don't run from a session (i.e. system processes), we need to provide aux to the `ps` command like so: `ps aux`

<figure><img src="../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

Another very useful command is the `top` command; top gives you real-time statistics about the processes running on your system instead of a one-time view.(refreshes every 10 seconds or when you use the arrow keys to browse the various rows).

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

`kill` command ends a process. you can provide arguments to determine how rough should the killing be done.

* SIGTERM - Kill the process, but allow it to do some cleanup tasks beforehand
* SIGKILL - Kill the process - doesn't do any cleanup after the fact
* SIGSTOP - Stop/suspend a process

the default is SIGTERM.

***

`systemctl [option] [service]` starts a process/service.

options are:

* Start (now)
* Stop (now)
* Enable (on boot)
* Disable (on boot)

e.g. `systemctl start apache2`

***

also we have `Ctrl + Z` to pause a process that is blocking the terminal. and adding it to jobs.

now with `bg` and `fg` we can decide to continue its running in the back-ground or fore-ground.

also `command --arg [blah]`**`&`**  makes the command to run in the back-gorund and also adding it to jobs.

you can see them jobs with the `jobs` command.

also you can kill them with `kill %x` command where `x` is the number of that job in the job list.
