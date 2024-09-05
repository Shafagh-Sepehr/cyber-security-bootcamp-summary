---
icon: rectangle-terminal
---

# hydra

## Hydra

Hydra is a very fast online password cracking tool, which can perform rapid dictionary attacks against more than 50 Protocols, including Telnet, RDP, SSH, FTP, HTTP, HTTPS, SMB, several databases and much more.

The syntax for the command we're going to use to find the passwords is this:

`hydra -t [paral-num] -l [user] -P [/path/to/dictionary] -vV [machine IP] [protocol]`

<table><thead><tr><th>SECTION</th><th>FUNCTION</th><th data-hidden></th></tr></thead><tbody><tr><td><code>hydra</code></td><td>Runs the hydra tool</td><td></td></tr><tr><td><code>-t [paral-num]</code></td><td>Number of parallel connections per target</td><td></td></tr><tr><td><code>-l [user]</code> </td><td>Points to the user who's account you're trying to compromise</td><td></td></tr><tr><td><code>-vV</code></td><td>Sets verbose mode to very verbose, shows the login+pass combination for each attempt</td><td></td></tr><tr><td><code>-P [path to dictionary]</code></td><td>Points to the file containing the list of possible passwords</td><td></td></tr><tr><td><code>[machine IP]</code></td><td>The IP address of the target machine</td><td></td></tr><tr><td><code>[protocol]</code></td><td>Sets the protocol</td><td></td></tr></tbody></table>

`[protocol]` can be: “`Asterisk, AFP, Cisco AAA, Cisco auth, Cisco enable, CVS, Firebird, FTP, HTTP-FORM-GET, HTTP-FORM-POST, HTTP-GET, HTTP-HEAD, HTTP-POST, HTTP-PROXY, HTTPS-FORM-GET, HTTPS-FORM-POST, HTTPS-GET, HTTPS-HEAD, HTTPS-POST, HTTP-Proxy, ICQ, IMAP, IRC, LDAP, MEMCACHED, MONGODB, MS-SQL, MYSQL, NCP, NNTP, Oracle Listener, Oracle SID, Oracle, PC-Anywhere, PCNFS, POP3, POSTGRES, Radmin, RDP, Rexec, Rlogin, Rsh, RTSP, SAP/R3, SIP, SMB, SMTP, SMTP Enum, SNMP v1+v2+v3, SOCKS5, SSH (v1 and v2), SSHKEY, Subversion, TeamSpeak (TS2), Telnet, VMware-Auth, VNC and XMPP.`”

For more information on the options of each protocol in Hydra, you can check the [Kali Hydra tool page](https://en.kali.tools/?p=220).

## Post Web Form

We can use Hydra to brute force web forms too. You must know which type of request it is making; GET or POST methods are commonly used. You can use your browser’s network tab (in developer tools) to see the request types or view the source code.

`sudo hydra -l <username> -P <wordlist> 10.10.43.23 http-post-form "<path>:<login_credentials>:<invalid_response>"`

| Option                | Description                                                                              |
| --------------------- | ---------------------------------------------------------------------------------------- |
| `-l`                  | the username for (web form) login                                                        |
| `-P`                  | the password list to use                                                                 |
| `http-post-form`      | the type of the form is POST                                                             |
| `<path>`              | the login page URL, for example, `login.php`                                             |
| `<login_credentials>` | the username and password used to log in, for example, `username=^USER^&password=^PASS^` |
| `<invalid_response>`  | part of the response when the login fails                                                |
| `-V`                  | verbose output for every attempt                                                         |

Below is a more concrete example Hydra command to brute force a POST login form:

`hydra -l <username> -P <wordlist> 10.10.43.23 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V`

* The login page is only `/`, i.e., the main IP address.
* The `username` is the form field where the username is entered
* The specified username(s) will replace `^USER^`
* The `password` is the form field where the password is entered
* The provided passwords will be replacing `^PASS^`
* Finally, `F=incorrect` is a string that appears in the server reply when the login fails
