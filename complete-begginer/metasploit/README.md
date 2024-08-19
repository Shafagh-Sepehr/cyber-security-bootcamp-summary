---
icon: rectangle-terminal
---

# metasploit

rankings given in `search`:

<figure><img src="../../.gitbook/assets/image (98).png" alt=""><figcaption></figcaption></figure>

You can direct the search function using keywords such as type and platform:

{% code title="Search by module type" %}
```sh
msf6 > search type:auxiliary telnet

Matching Modules
================

   #   Name                                                Disclosure Date  Rank    Check  Description
   -   ----                                                ---------------  ----    -----  -----------
   0   auxiliary/admin/http/dlink_dir_300_600_exec_noauth  2013-02-04       normal  No     D-Link DIR-600 / DIR-300 Unauthenticated Remote Command Execution
   1   auxiliary/admin/http/netgear_r6700_pass_reset       2020-06-15       normal  Yes    Netgear R6700v3 Unauthenticated LAN Admin Password Reset
   2   auxiliary/dos/cisco/ios_telnet_rocem                2017-03-17       normal  No     Cisco IOS Telnet Denial of Service
   3   auxiliary/dos/windows/ftp/iis75_ftpd_iac_bof        2010-12-21       normal  No     Microsoft IIS FTP Server Encoded Response Overflow Trigger
   4   auxiliary/scanner/ssh/juniper_backdoor              2015-12-20       normal  No     Juniper SSH Backdoor Scanner
   5   auxiliary/scanner/telnet/brocade_enable_login                        normal  No     Brocade Enable Login Check Scanner
   6   auxiliary/scanner/telnet/lantronix_telnet_password                   normal  No     Lantronix Telnet Password Recovery
   7   auxiliary/scanner/telnet/lantronix_telnet_version                    normal  No     Lantronix Telnet Service Banner Detection
   8   auxiliary/scanner/telnet/satel_cmd_exec             2017-04-07       normal  No     Satel Iberia SenNet Data Logger and Electricity Meters Command Injection Vulnerability
   9   auxiliary/scanner/telnet/telnet_encrypt_overflow                     normal  No     Telnet Service Encryption Key ID Overflow Detection
   10  auxiliary/scanner/telnet/telnet_login                                normal  No     Telnet Login Check Scanner
   11  auxiliary/scanner/telnet/telnet_ruggedcom                            normal  No     RuggedCom Telnet Password Generator
   12  auxiliary/scanner/telnet/telnet_version                              normal  No     Telnet Service Banner Detection
   13  auxiliary/server/capture/telnet                                      normal  No     Authentication Capture: Telnet


Interact with a module by name or index, for example use 13 or use auxiliary/server/capture/telnet

msf6 >
```
{% endcode %}

***

Parameters you will often use are:

* `RHOSTS`: “Remote host”, the IP address of the target system. A single IP address or a network range can be set. This will support the CIDR (Classless Inter-Domain Routing) notation (/24, /16, etc.) or a network range (10.10.10.x – 10.10.10.y). You can also use a file where targets are listed, one target per line using file:/path/of/the/target\_file.txt, as you can see below.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/603df7900d7b6f1dff18b0bd/room-content/138a36f26c25994fcfe47e1fab085ac8.png)

* `RPORT`: “Remote port”, the port on the target system the vulnerable application is running on.
* `PAYLOAD`: The payload you will use with the exploit.
* `LHOST`: “Localhost”, the attacking machine (your AttackBox or Kali Linux) IP address.
* `LPORT`: “Local port”, the port you will use for the reverse shell to connect back to. This is a port on your attacking machine, and you can set it to any port not used by any other application.
* `SESSION`: Each connection established to the target system using Metasploit will have a session ID. You will use this with post-exploitation modules that will connect to the target system using an existing connection.

***

## some usefull exploits:

#### enumeration:

```
auxiliary/scanner/http/http_version
```

```
scanner/smb/smb_version
```

```
scanner/discovery/udp_sweep
```

```
search portscan
```

explotation:

```
auxiliary/scanner/smb/smb_login
```

```
scanner/vnc/vnc_login
```

if you have username and password of a smb user:

```
exploit/windows/smb/psexec
```

handler:

```
exploit/multi/handler
```

post exploitation:

```
post/linux/gather/hashdump
```

