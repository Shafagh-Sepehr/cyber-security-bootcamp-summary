---
icon: rectangle-terminal
---

# smb

SMB - Server Message Block Protocol - is a client-server communication protocol used for sharing access to files, printers, serial ports and other resources on a network. \[[source](https://searchnetworking.techtarget.com/definition/Server-Message-Block-Protocol)]

The SMB protocol is known as a response-request protocol, meaning that it transmits multiple messages between the client and server to establish a connection. Clients connect to servers using TCP/IP (actually **NetBIOS** over TCP/IP as specified in RFC1001 and RFC1002), NetBEUI or IPX/SPX.

<figure><img src="../.gitbook/assets/image (73).png" alt=""><figcaption></figcaption></figure>

Clients can send commands (SMBs) to the server to access shares, open files, and read/write files over the network.

Since Windows 95, Microsoft Windows has supported the SMB protocol for clients and servers. Samba, a server for Unix systems, also supports SMB.

## Enumeration

Enumeration involves gathering information on a target to identify potential attack vectors for exploitation. It is crucial for successful attacks, as it helps avoid ineffective exploits. This process can collect usernames, passwords, network details, hostnames, application data, and services valuable to attackers.

## SMB

SMB share drives on a server allow users to view or transfer files. They can be a valuable starting point for attackers seeking sensitive information, as these shares may contain unexpected data.

## Port Scanning

The first step of enumeration is to conduct a port scan to gather information about the target machine's services, applications, structure, and operating system.

## Enum4Linux

Enum4linux is a tool for enumerating SMB shares on Windows and Linux systems, serving as a wrapper for Samba tools.

`enum4linux [options] ip`

<table><thead><tr><th>TAG</th><th>FUNCTION</th><th data-hidden></th></tr></thead><tbody><tr><td><code>-U</code></td><td>get userlist</td><td></td></tr><tr><td><code>-M</code></td><td>get machine list</td><td></td></tr><tr><td><code>-N</code></td><td>get namelist dump (different from <code>-U</code> and <code>-M</code>)</td><td></td></tr><tr><td><code>-S</code></td><td>get sharelist</td><td></td></tr><tr><td><code>-P</code></td><td>get password policy information</td><td></td></tr><tr><td><code>-G</code></td><td>get group and member list</td><td></td></tr><tr><td><code>-a</code></td><td>all of the above (full basic enumeration)</td><td></td></tr></tbody></table>

## Types of SMB Exploit

While vulnerabilities like [CVE-2017-7494](https://www.cvedetails.com/cve/CVE-2017-7494/) can enable remote code execution via SMB, misconfigurations are often the primary entry point. In this case, we'll exploit anonymous SMB share access, a common misconfiguration that can provide information leading to a shell.

from out enumeration stage we would know some usefull data like The SMB share location and The name of an interesting SMB share.

To access an SMB share, we will use **SMBClient**, part of the default Samba suite.

`smbclient //[IP]/[SHARE] -U [username] -p [port]`

the default smb port is 139 or 445 which we would get from port scanning.

an smb share may be configured to allow anonymous access using the username "`Anonymous`" without supplying a password.

## Download share recursively

`smbget -R smb://10.10.98.233/anonymous`

## Nmap scripts

`nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse [ip]`
