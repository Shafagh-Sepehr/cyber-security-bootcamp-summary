---
icon: rectangle-terminal
---

# ftp

## What is FTP?

File Transfer Protocol (FTP) is, as the name suggests , a protocol used to allow remote transfer of files over a network. It uses a client-server model to do this, and- as we'll come on to later- relays commands and data in a very efficient way.\


## How does FTP work?

A typical FTP session operates using two channels:

* a command (sometimes called the control) channel
* a data channel.

As their names imply, the command channel is used for transmitting commands as well as replies to those commands, while the data channel is used for transferring data.

FTP operates using a client-server protocol. The client initiates a connection with the server, the server validates whatever login credentials are provided and then opens the session.

While the session is open, the client may execute FTP commands on the server.

## Active vs Passive

The FTP server may support either Active or Passive connections, or both.&#x20;

* In an Active FTP connection, the client opens a port and listens. The server is required to actively connect to it.&#x20;
* In a Passive FTP connection, the server opens a port and listens (passively) and the client connects to it.&#x20;

info: This separation of command information and data into separate channels is a way of being able to send commands to the server without having to wait for the current data transfer to finish. If both channels were interlinked, you could only enter commands in between data transfers, which wouldn't be efficient for either large file transfers, or slow internet connections.

default port is 20 or 21.

We're going to be exploiting an anonymous FTP login, to see what files we can access- and if they contain any information that might allow us to pop a shell on the system. This is a common pathway in CTF challenges, and mimics a real-life careless implementation of FTP servers.

## Alternative Enumeration Methods

It's worth noting  that some vulnerable versions of in.ftpd and some other FTP server variants return different responses to the "cwd" command for home directories which exist and those that don’t. This can be exploited because you can issue cwd commands before authentication, and if there's a home directory- there is more than likely a user account to go with it. While this bug is found mainly within legacy systems, it's worth knowing about, as a way to exploit FTP.

we can check to see if we are able to login anonymously to the FTP server. We can do this using by typing `ftp [IP]`([see earlier ftp page](../pre-security-tools/ftp.md)) into the console, and entering "anonymous", and no password when prompted.
