---
icon: file-lines
---

# Shell

## Tools for receiving shells:

### Netcat:

Netcat is the traditional "Swiss Army Knife" of networking. It is used to manually perform all kinds of network interactions, including things like banner grabbing during enumeration, but more importantly for our uses, it can be used to receive reverse shells and connect to remote ports attached to bind shells on a target system. Netcat shells are very unstable (easy to lose) by default, but can be improved by techniques that we will be covering in an upcoming task.

### Socat:

Socat is like netcat on steroids. It can do all of the same things, and _many_ more. Socat shells are usually more stable than netcat shells out of the box. In this sense it is vastly superior to netcat; however, there are two big catches:

1. The syntax is more difficult
2. Netcat is installed on virtually every Linux distribution by default. Socat is very rarely installed by default.

There are work arounds to both of these problems, which we will cover later on.

Both Socat and Netcat have .exe versions for use on Windows.

### Metasploit -- multi/handler:

The `exploit/multi/handler` module of the Metasploit framework is, like socat and netcat, used to receive reverse shells. Due to being part of the Metasploit framework, multi/handler provides a fully-fledged way to obtain stable shells, with a wide variety of further options to improve the caught shell. It's also the only way to interact with a _meterpreter_ shell, and is the easiest way to handle _staged_ payloads -- both of which we will look at in task 9.

### Msfvenom:

Like multi/handler, msfvenom is technically part of the Metasploit Framework, however, it is shipped as a standalone tool. Msfvenom is used to generate payloads on the fly. Whilst msfvenom can generate payloads other than reverse and bind shells, these are what we will be focusing on in this room. Msfvenom is an incredibly powerful tool, so we will go into its application in much more detail in a dedicated task.

***

for finding shells:

{% embed url="https://web.archive.org/web/20200901140719/http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet" %}

{% embed url="https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md" %}

***

## Types of Shell

* Reverse shells are when the target is forced to execute code that connects _back_ to your computer. On your own computer you would use one of the tools mentioned in the previous task to set up a _listener_ which would be used to receive the connection. Reverse shells are a good way to bypass firewall rules that may prevent you from connecting to arbitrary ports on the target; however, the drawback is that, when receiving a shell from a machine across the internet, you would need to configure your own network to accept the shell. This, however, will not be a problem on the TryHackMe network due to the method by which we connect into the network.
* Bind shells are when the code executed on the target is used to start a listener attached to a shell directly on the target. This would then be opened up to the internet, meaning you can connect to the port that the code has opened and obtain remote code execution that way. This has the advantage of not requiring any configuration on your own network, but may be prevented by firewalls protecting the target.

As a general rule, reverse shells are easier to execute and debug.

***

## interactive or non-interactive shells:

*   _Interactive:_ If you've used Powershell, Bash, Zsh, sh, or any other standard CLI environment then you will be used to\
    interactive shells. These allow you to interact with programs after executing them. For example, take the SSH login prompt:\


    <figure><img src="../../.gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>

    Here you can see that it's asking _interactively_ that the user type either yes or no in order to continue the connection. This is an interactive program, which requires an interactive shell in order to run.\

*   _Non-Interactive_ shells don't give you that luxury. In a non-interactive shell you are limited to using programs which do not require user interaction in order to run properly. Unfortunately, the majority of simple reverse and bind shells are non-interactive, which can make further exploitation trickier. Let's see what happens when we try to run SSH in a non-interactive shell:\


    <figure><img src="../../.gitbook/assets/image (101).png" alt=""><figcaption></figcaption></figure>

    Notice that the `whoami` command (which is non-interactive) executes perfectly, but the `ssh` command (which _is_ interactive) gives us no output at all.
