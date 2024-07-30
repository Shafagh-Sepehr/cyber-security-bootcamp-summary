# basic CMD command

The command `hostname` will output the computer name.

<figure><img src="https://assets.tryhackme.com/additional/win-fun2/hostname.png" alt="" width="563"><figcaption></figcaption></figure>

***

The command `whoami` will output the name of the logged-in user.

![](https://assets.tryhackme.com/additional/win-fun2/whoami.png)

***

A command used often is `ipconfig`. This command will show the network address settings for the computer.

![](https://assets.tryhackme.com/additional/win-fun2/ipconfig.png)

A  command to retrieve the help manual for a command is `/?`.

For example, to see the help manual for ipconfig, you can use the following command: `ipconfig /?`

![](https://assets.tryhackme.com/additional/win-fun2/ipconfig-help.png)

***

Note: To clear the command prompt screen, the command is `cls`.

***

`netstat` will display protocol statistics and current TCP/IP network connections.&#x20;

![](https://assets.tryhackme.com/additional/win-fun2/netstat.png)

***

The `net` command is primarily used to manage network resources. This command supports sub-commands.

If you type net without a sub-command, the output will show the syntax for the root command showing a few of the sub-commands you can use.

<figure><img src="https://assets.tryhackme.com/additional/win-fun2/net.png" alt="" width="563"><figcaption></figcaption></figure>

For the net command, to display the help manual `/?` will not work. In this case, you need to use different syntax, which is `net help`.

<figure><img src="https://assets.tryhackme.com/additional/win-fun2/net-help.png" alt=""><figcaption></figcaption></figure>

So, if you wish to see the help information for `net user` , the command is `net help user`.&#x20;

![](https://assets.tryhackme.com/additional/win-fun2/net-help-user2.png)

You can use the same command to view the help information for other useful net sub-commands, such as localgroup, use, share, and session.&#x20;
