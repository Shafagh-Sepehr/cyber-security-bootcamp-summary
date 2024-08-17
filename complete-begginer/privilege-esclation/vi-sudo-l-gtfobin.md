# Vi, sudo -l, GTFOBin

## `Sudo -l`

This exploit comes down to how effective our user account enumeration has been. Every time you have access to an account during a CTF scenario, you should use "sudo -l" to list what commands you're able to use as a super user on that account. Sometimes, like this, you'll find that you're able to run certain commands as a root user without the root password. This can enable you to escalate privileges.

## Escaping Vi

if in sudo -l you see that a you can run "sudo vi", This will allow us to escape vi in order to escalate privileges and get a shell as the root user!

just typr `:!sh` to get a root shell.

## Misconfigured Binaries and GTFOBins

If you find a misconfigured binary during your enumeration, or when you check what binaries a user account you have access to can access, a good place to look up how to exploit them is GTFOBins. GTFOBins is a curated list of Unix binaries that can be exploited by an attacker to bypass local security restrictions. It provides a really useful breakdown of how to exploit a misconfigured binary and is the first place you should look if you find one on a CTF or Pentest.

{% embed url="https://gtfobins.github.io/" %}

