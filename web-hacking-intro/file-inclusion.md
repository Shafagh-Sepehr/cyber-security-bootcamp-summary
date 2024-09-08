# File Inclusion

### Why do File inclusion vulnerabilities happen?﻿

File inclusion vulnerabilities are commonly found and exploited in various programming languages for web applications, such as PHP that are poorly written and implemented. The main issue of these vulnerabilities is the input validation, in which the user inputs are not sanitized or validated, and the user controls them. When the input is not validated, the user can pass any input to the function, causing the vulnerability.

### What is the risk of File inclusion?

By default, an attacker can leverage file inclusion vulnerabilities to leak data, such as code, credentials or other important files related to the web application or operating system. Moreover, if the attacker can write files to the server by any other means, file inclusion might be used in tandem to gain remote command execution (RCE).



## ﻿Path Traversal

Also known as Directory traversal, a web security vulnerability allows an attacker to read operating system resources, such as local files on the server running an application. The attacker exploits this vulnerability by manipulating and abusing the web application's URL to locate and access files or directories stored outside the application's root directory.

Path traversal vulnerabilities occur when the user's input is passed to a function such as file\_get\_contents in PHP. It's important to note that the function is not the main contributor to the vulnerability. Often poor input validation or filtering is the cause of the vulnerability. In PHP, you can use the file\_get\_contents to read the content of a file. You can find more information about the function [here](https://www.php.net/manual/en/function.file-get-contents.php).

The following graph shows how a web application stores files in /var/www/app. The happy path would be the user requesting the contents of userCV.pdf from a defined path /var/www/app/CVs.\


<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/45d9c1baacda290c1f95858e27f740c9.png" alt=""><figcaption></figcaption></figure>

We can test out the URL parameter by adding payloads to see how the web application behaves. Path traversal attacks, also known as the dot-dot-slash attack, take advantage of moving the directory one step up using the double dots ../. If the attacker finds the entry point, which in this case get.php?file=, then the attacker may send something as follows, http://webapp.thm/get.php?file=../../../../etc/passwd\


Suppose there isn't input validation, and instead of accessing the PDF files at /var/www/app/CVs location, the web application retrieves files from other directories, which in this case /etc/passwd. Each .. entry moves one directory until it reaches the root directory /. Then it changes the directory to /etc, and from there, it read the passwd file.\


![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/3037513935e3242f74bd0fe97833b5ac.png)

As a result, the web application sends back the file's content to the user.

\


<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/c12d34456ebe25bafffeb829c58f98c0.png" alt=""><figcaption></figcaption></figure>

Similarly, if the web application runs on a Windows server, the attacker needs to provide Windows paths. For example, if the attacker wants to read the boot.ini file located in c:\boot.ini, then the attacker can try the following depending on the target OS version:

http://webapp.thm/get.php?file=../../../../boot.ini or

http://webapp.thm/get.php?file=../../../../windows/win.ini\


The same concept applies here as with Linux operating systems, where we climb up directories until it reaches the root directory, which is usually c:\\.\


Sometimes, developers will add filters to limit access to only certain files or directories. Below are some common OS files you could use when testing.&#x20;

\


<table data-header-hidden><thead><tr><th width="280"></th><th></th></tr></thead><tbody><tr><td>Location</td><td>Description<br></td></tr><tr><td>/etc/issue</td><td>contains a message or system identification to be printed before the login prompt.<br></td></tr><tr><td>/etc/profile</td><td>controls system-wide default variables, such as Export variables, File creation mask (umask), Terminal types, Mail messages to indicate when new mail has arrived</td></tr><tr><td>/proc/version</td><td>specifies the version of the Linux kernel<br></td></tr><tr><td>/etc/passwd</td><td>has all registered user that has access to a system<br></td></tr><tr><td>/etc/shadow</td><td>contains information about the system's users' passwords<br></td></tr><tr><td>/root/.bash_history</td><td>contains the history commands for root user<br></td></tr><tr><td>/var/log/dmessage</td><td>contains global system messages, including the messages that are logged during system startup<br></td></tr><tr><td>/var/mail/root</td><td>all emails for root user</td></tr><tr><td>/root/.ssh/id_rsa</td><td>Private SSH keys for a root or any known valid user on the server</td></tr><tr><td>/var/log/apache2/access.log</td><td>the accessed requests for Apache  webserver</td></tr><tr><td>C:\boot.ini</td><td>contains the boot options for computers with BIOS firmware</td></tr></tbody></table>

