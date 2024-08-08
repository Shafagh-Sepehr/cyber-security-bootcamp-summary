---
icon: rectangle-terminal
---

# nmap

In a security audit of specified IP addresses, the first step is to identify the services running on each target through port scanning. This process reveals open ports that facilitate network connections. Ports are crucial for managing multiple services on a server, such as HTTP and HTTPS for web servers. For instance, a client may connect from port 49534 to a server's port 443. Understanding this landscape is essential for effective security assessment.

<figure><img src="../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

Every computer has a total of 65535 available ports; however, many of these are registered as standard ports. For example, a HTTP Webservice can nearly always be found on port 80 of the server. It is important to note; however, that especially in a CTF setting, it is not unheard of for even these standard ports to be altered, making it even more imperative that we perform appropriate enumeration on the target.

To effectively attack a target, it is essential to begin with a port scan, as knowledge of open ports is critical. This is typically done using a tool called Nmap, which can perform various types of port scans. Nmap connects to each port on the target, determining their status as open, closed, or filtered by a firewall. Once open ports are identified, the next step is to enumerate the services running on each port, either manually or using `nmap`.

So, why nmap? 'cuase nmap powerfull, that's why.

***

## some usefull switches for nmap:

<table><thead><tr><th width="218">switch</th><th>description</th><th data-hidden></th></tr></thead><tbody><tr><td><code>-sT</code></td><td>TCP Connect Scans</td><td></td></tr><tr><td><code>-sS</code></td><td>SYN "Half-open" Scans</td><td></td></tr><tr><td><code>-sU</code></td><td>UDP scan</td><td></td></tr><tr><td><code>-sN</code></td><td>TCP Null Scans</td><td></td></tr><tr><td><code>-sF</code></td><td>TCP FIN Scans</td><td></td></tr><tr><td><code>-sX</code></td><td>TCP Xmas Scans</td><td></td></tr><tr><td><code>-O</code></td><td>detect which operating system the target is running on</td><td></td></tr><tr><td><code>-sV</code></td><td>detect the version of the services running on the target</td><td></td></tr><tr><td><code>-sC</code></td><td>Performs a script scan using the default set of scripts.</td><td></td></tr><tr><td><code>-v</code></td><td>lvl 1 verbosity (increase the verbosity for more information)</td><td></td></tr><tr><td><code>-vv</code></td><td>lvl 2 verbosity</td><td></td></tr><tr><td><code>-A</code></td><td>hungry for more result? "aggressive" mode (activates service detection, operating system detection, a traceroute and common script scanning)</td><td></td></tr><tr><td><code>-oA</code></td><td>save the nmap results in three major formats</td><td></td></tr><tr><td><code>-oN</code></td><td>save the nmap results in a "normal" format</td><td></td></tr><tr><td><code>-oG</code></td><td>save the nmap results in a "<code>grep</code>able" format</td><td></td></tr><tr><td><code>-T[x]</code><br><code>-T [template]</code></td><td>"timing" template(speed of scan(higher speeds risk getting detected))<br><code>x</code>: <code>0</code>|<code>1</code>|<code>2</code>|<code>3</code>|<code>4</code>|<code>5</code><br>according to templates<br><code>template</code>s: <code>paranoid</code>|<code>sneaky</code>|<code>polite</code>|<code>normal</code>|<code>aggressive</code>|<code>insane</code></td><td></td></tr><tr><td><code>-p [port]</code><br><code>-p [from-to]</code><br><code>-p-</code> (scan all)</td><td>port(s) to scan</td><td></td></tr><tr><td><code>--script</code><br><code>--script=[category]</code></td><td>activate a script from the nmap scripting library</td><td></td></tr><tr><td><code>-sn</code></td><td>ping sweep (sends ICMP packet to each possible IP address to find which IP is alive)</td><td></td></tr><tr><td><code>-Pn</code></td><td>(No ping)<br>tells Nmap to not bother pinging the host before scanning it.<br>This means that Nmap will always treat the target host(s) as being alive, effectively bypassing the ICMP block.(some firewalls do that like windows)</td><td></td></tr><tr><td><code>-f</code></td><td>Used to fragment the packets (i.e. split them into smaller pieces) making it less likely that the packets will be detected by a firewall or IDS.</td><td></td></tr><tr><td><code>--mtu [number]</code></td><td>accepts a maximum transmission unit size to use for the packets sent. This <em>must</em> be a multiple of 8.</td><td></td></tr><tr><td><code>--scan-delay [time]ms</code></td><td>adds a delay between packets sent. useful if network is unstable.<br>also for evading any time-based firewall/IDS triggers which may be in place.</td><td></td></tr><tr><td><code>--badsum</code></td><td>generates invalid checksum for packets. real TCP/IP stack would drop this packet.<br>firewalls may respond.<br>can be used to determine the presence of a firewall/IDS.</td><td></td></tr><tr><td><code>--data-length</code></td><td>Append random data to sent packets</td><td></td></tr></tbody></table>

We should always save the output of our scans -- this means that we only need to run the scan once (reducing network traffic and thus chance of detection), and gives us a reference to use when writing reports for clients.

***

## TCP Connect scan (`-sT`):

<figure><img src="../.gitbook/assets/image (62).png" alt="" width="307"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (64).png" alt="" width="563"><figcaption></figcaption></figure>

_"... If the connection does not exist (CLOSED), then a reset (RST) is sent in response to any incoming segment except another reset. A SYN segment that does not match an existing connection is rejected by this means."_ [RFC 9293](https://datatracker.ietf.org/doc/html/rfc9293)

<figure><img src="../.gitbook/assets/image (61).png" alt=""><figcaption></figcaption></figure>

this way nmap finds if a port is closed.

What if the port is open, but hidden behind a firewall? Many firewalls are configured to simply **drop** incoming packets. Nmap sends a TCP SYN request, and receives nothing back. This indicates that the port is being protected by a firewall and thus the port is considered to be _filtered_.

That said, it is very easy to configure a firewall to respond with a RST TCP packet.

## SYN scan (`-sS`):

Where TCP scans perform a full three-way handshake with the target, SYN scans sends back a RST TCP packet after receiving a SYN/ACK from the server (this prevents the server from repeatedly trying to make the request)

<figure><img src="../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

pros:

* can be used to bypass older Intrusion Detection systems as they are looking out for a full three way handshake.
* SYN scans are often not logged by applications listening on open ports, as standard practice is to log a connection once it's been fully established.

is called stealth scan becuase of the two pros above.

* significantly faster(not completing a three-way handshake)

cons:

* requires sudo permissions
* Unstable services are sometimes brought down by SYN scans, which could prove problematic if a client has provided a production environment for the test.

For this reason, SYN scans are the default scans used by Nmap _if run with sudo permissions_. If run without sudo permissions, Nmap defaults to the TCP Connect scan.

finds about closed and filtered ports just like a TCP connect scan.

## &#x20;UDP scan (`-sU`):

Unlike TCP, UDP connections are _stateless_. This means that, rather than a "handshake", UDP connections rely on sending packets to a target port and essentially hoping that they make it.&#x20;

the lack of acknowledgement makes UDP significantly more difficult (and much slower) to scan.

When a packet is sent to an open UDP port, there should be no response. When this happens, Nmap refers to the port as being `open|filtered`. In other words, it suspects that the port is open, but it could be firewalled.

If it gets a UDP response (which is very unusual), then the port is marked as _**open**_. More commonly there is no response, in which case the request is sent a second time as a double-check. If there is still no response then the port is marked _open|filtered_ and Nmap moves on.

When a packet is sent to a _**closed**_ UDP port, the target should respond with an ICMP (ping) packet containing a message that the port is unreachable. This clearly identifies closed ports, which Nmap marks as such and moves on.

Due to this difficulty in identifying whether a UDP port is actually open, UDP scans tend to be incredibly slow in comparison to the various TCP scans (in the region of 20 minutes to scan the first 1000 ports, with a good connection). For this reason it's usually good practice to run an Nmap scan with `--top-ports <number>` enabled. For example, scanning with  `nmap -sU --top-ports 20 <target>`. Will scan the top 20 most commonly used UDP ports, resulting in a much more acceptable scan time.

usually sends completely empty requests -- just raw UDP packets. but for ports which are usually occupied by well-known services, it will instead send a protocol-specific payload.

## NULL, FIN and Xmas:

these scans are less commonly used. (used for firewall evasion)

tend to be even stealthier, relatively speaking, than a SYN "stealth" scan.

* As the name suggests, NULL scans (`-sN`) are when the TCP request is sent with no flags set at all. As per the RFC, the target host should respond with a RST if the port is closed.

<figure><img src="../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

* FIN scans (`-sF`) work in an almost identical fashion; however, instead of sending a completely empty packet, a request is sent with the FIN flag (usually used to gracefully close an active connection). Once again, Nmap expects a RST if the port is closed.

<figure><img src="../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

* As with the other two scans in this class, Xmas scans (`-sX`) send a malformed TCP packet and expects a RST response for closed ports.

<figure><img src="../.gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>

#### For all three, If the port is open then there is no response to the malformed packet.

Unfortunately (as with open UDP ports), that is _also_ an expected behaviour if the port is protected by a firewall, so NULL, FIN and Xmas scans will only ever identify ports as being `open|filtered`, `closed`, or `filtered`.

If a port is identified as filtered with one of these scans then it is usually because the target has responded with an ICMP unreachable packet.

It's also worth noting that while RFC 793 mandates that network hosts respond to malformed packets with a **RST** TCP packet for closed ports, and don't respond at all for open ports; this is not always the case in practice. In particular **Microsoft Windows** (and a lot of Cisco network devices) are known to respond with a **RST** to any malformed TCP packet -- regardless of whether the port is actually open or not. This results in all ports showing up as being closed.

That said, the goal here is firewall evasion. Many firewalls are configured to drop incoming TCP packets to blocked ports which have the SYN flag set (thus blocking new connection initiation requests). By sending requests which do not contain the SYN flag, we effectively bypass this kind of firewall. Whilst this is good in theory, most modern IDS solutions are savvy to these scan types, so don't rely on them to be 100% effective when dealing with modern systems.

***

## ping sweep:

In a black box assignment, the first goal is to map the network by identifying active hosts. This can be done using Nmap for a "ping sweep," where it sends ICMP packets to each IP address. Responding addresses are marked as active, providing a useful baseline, though it may not always be accurate.

To perform a ping sweep, we use the `-sn` switch in conjunction with IP ranges which can be specified with either a hypen (`-`) or CIDR notation. i.e. we could scan the `192.168.0.x` network using:

`nmap -sn 192.168.0.1-254`\
or\
`nmap -sn 192.168.0.0/24`

The `-sn` switch tells Nmap not to scan any ports.

In addition to the ICMP echo requests, the `-sn` switch will also cause nmap to send a TCP SYN packet to port 443 of the target, as well as a TCP ACK (or TCP SYN if not run as root) packet to port 80 of the target.

***

## The Nmap Scripting Engine (NSE):

The Nmap Scripting Engine (NSE) can be used to do a variety of things.

There are many categories available. Some useful categories include:

* `safe`:- Won't affect the target
* `intrusive`:- Not safe: likely to affect the target\

* `vuln`:- Scan for vulnerabilities
* `exploit`:- Attempt to exploit a vulnerability
* `auth`:- Attempt to bypass authentication for running services (e.g. Log into an FTP server anonymously)
* `brute`:- Attempt to bruteforce credentials for running services
* `discovery`:- Attempt to query running services for further information about the network (e.g. query an SNMP server).

A more exhaustive list can be found [here](https://nmap.org/book/nse-usage.html).

***

To run a specific script, we would use `--script=<script-name>` , e.g. `--script=http-fileupload-exploiter`.

Multiple scripts can be run simultaneously in this fashion by separating them by a comma. For example: `--script=smb-enum-users,smb-enum-shares`.

Some scripts require arguments (for example, credentials, if they're exploiting an authenticated vulnerability). These can be given with the `--script-args` Nmap switch. An example of this would be with the `http-put` script (used to upload files using the PUT method). This takes two arguments: the URL to upload the file to, and the file's location on disk.  For example:

`nmap -p 80 --script http-put --script-args http-put.url='/dav/shell.php',http-put.file='./shell.php'`

Note that the arguments are separated by commas, and connected to the corresponding script with periods (i.e.  `<script-name>.<argument>`).

A full list of scripts and their corresponding arguments (along with example use cases) can be found [here](https://nmap.org/nsedoc/).

Nmap scripts come with built-in help menus, which can be accessed using `nmap --script-help <script-name>`.

to _find_ needed scripts, We have two options. The first is the page on the [Nmap website](https://nmap.org/nsedoc/) (mentioned in the previous task) which contains a list of all official scripts. The second is the local storage on your attacking machine. Nmap stores its scripts on Linux at `/usr/share/nmap/scripts`. All of the NSE scripts are stored in this directory by default -- this is where Nmap looks for scripts when you specify them.

There are two ways to search for installed scripts. One is by using the `/usr/share/nmap/scripts/script.db` file. Despite the extension, it's not a db but a formatted text file containing filenames and categories for each available script.

<figure><img src="../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (71).png" alt=""><figcaption><p><em>Note the use of asterisks</em> (<code>*</code>) <em>on either side of the search term</em></p></figcaption></figure>

The same techniques can also be used to search for categories of script. For example:\
`grep "safe" /usr/share/nmap/scripts/script.db`

<figure><img src="../.gitbook/assets/image (72).png" alt=""><figcaption></figcaption></figure>

### _Installing New Scripts:_

install the scripts manually by downloading the script from Nmap:(`sudo wget -O /usr/share/nmap/scripts/<script-name>.nse https://svn.nmap.org/nmap/scripts/<script-name>.nse`). This must then be followed up with `nmap --script-updatedb`, which updates the `script.db` file to contain the newly downloaded script.

***

## Firewall Evasion

some techniques for bypassing firewalls are (think stealth scans, along with NULL, FIN and Xmas scans).

Your typical Windows host will, with its default firewall, block all ICMP packets. This presents a problem: not only do we often use _ping_ to manually establish the activity of a target, Nmap does the same thing by default. This means that Nmap will register a host with this firewall configuration as dead and not bother scanning it at all.

Nmap provides an option for this: `-Pn`, which tells Nmap to not bother pinging the host before scanning it. This means that Nmap will always treat the target host(s) as being alive, effectively bypassing the ICMP block; however, it comes at the price of potentially taking a very long time to complete the scan (if the host really is dead then Nmap will still be checking and double checking every specified port).

It's worth noting that if you're already directly on the local network, Nmap can also use ARP requests to determine host activity.

***

There are a variety of other switches which Nmap considers useful for firewall evasion.  they can be found [here](https://nmap.org/book/man-bypass-firewalls-ids.html).

The following switches are of particular note:

* `-f`:- Used to fragment the packets (i.e. split them into smaller pieces) making it less likely that the packets will be detected by a firewall or IDS.
* An alternative to `-f`, but providing more control over the size of the packets: `--mtu <number>`, accepts a maximum transmission unit size to use for the packets sent. This _must_ be a multiple of 8.
* `--scan-delay <time>ms`:- used to add a delay between packets sent. This is very useful if the network is unstable, but also for evading any time-based firewall/IDS triggers which may be in place.
* `--badsum`:- this is used to generate in invalid checksum for packets. Any real TCP/IP stack would drop this packet, however, firewalls may potentially respond automatically, without bothering to check the checksum of the packet. As such, this switch can be used to determine the presence of a firewall/IDS.
