# ▶️ hydra

## Hydra

Hydra is a very fast online password cracking tool, which can perform rapid dictionary attacks against more than 50 Protocols, including Telnet, RDP, SSH, FTP, HTTP, HTTPS, SMB, several databases and much more.

The syntax for the command we're going to use to find the passwords is this:

`hydra -t [paral-num] -l [user] -P [/path/to/dictionary] -vV [machine IP] [protocol]`

<table><thead><tr><th>SECTION</th><th>FUNCTION</th><th data-hidden></th></tr></thead><tbody><tr><td><code>hydra</code></td><td>Runs the hydra tool</td><td></td></tr><tr><td><code>-t [paral-num]</code></td><td>Number of parallel connections per target</td><td></td></tr><tr><td><code>-l [user]</code> </td><td>Points to the user who's account you're trying to compromise</td><td></td></tr><tr><td><code>-vV</code></td><td>Sets verbose mode to very verbose, shows the login+pass combination for each attempt</td><td></td></tr><tr><td><code>-P [path to dictionary]</code></td><td>Points to the file containing the list of possible passwords</td><td></td></tr><tr><td><code>[machine IP]</code></td><td>The IP address of the target machine</td><td></td></tr><tr><td><code>[protocol]</code></td><td>Sets the protocol</td><td></td></tr></tbody></table>

