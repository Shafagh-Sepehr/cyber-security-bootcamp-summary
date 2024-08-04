# ▶️ nmap

In a security audit of specified IP addresses, the first step is to identify the services running on each target through port scanning. This process reveals open ports that facilitate network connections. Ports are crucial for managing multiple services on a server, such as HTTP and HTTPS for web servers. For instance, a client may connect from port 49534 to a server's port 443. Understanding this landscape is essential for effective security assessment.

<figure><img src="../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

Every computer has a total of 65535 available ports; however, many of these are registered as standard ports. For example, a HTTP Webservice can nearly always be found on port 80 of the server. It is important to note; however, that especially in a CTF setting, it is not unheard of for even these standard ports to be altered, making it even more imperative that we perform appropriate enumeration on the target.

To effectively attack a target, it is essential to begin with a port scan, as knowledge of open ports is critical. This is typically done using a tool called Nmap, which can perform various types of port scans. Nmap connects to each port on the target, determining their status as open, closed, or filtered by a firewall. Once open ports are identified, the next step is to enumerate the services running on each port, either manually or using `nmap`.

So, why nmap? 'cuase nmap powerfull, that's why.

***

some usefull switches for nmap

<table><thead><tr><th width="194">switch</th><th>description</th><th data-hidden></th></tr></thead><tbody><tr><td><code>-sS</code></td><td>Syn Scan</td><td></td></tr><tr><td><code>-sU</code></td><td>UDP scan</td><td></td></tr><tr><td><code>-O</code></td><td>detect which operating system the target is running on</td><td></td></tr><tr><td><code>-sV</code></td><td>detect the version of the services running on the target</td><td></td></tr><tr><td><code>-v</code></td><td>lvl 1 verbosity (increase the verbosity for more information)</td><td></td></tr><tr><td><code>-vv</code></td><td>lvl 2 verbosity</td><td></td></tr><tr><td><code>-A</code></td><td>hungry for more result? "aggressive" mode (activates service detection, operating system detection, a traceroute and common script scanning)</td><td></td></tr><tr><td><code>-oA</code></td><td>save the nmap results in three major formats</td><td></td></tr><tr><td><code>-oN</code></td><td>save the nmap results in a "normal" format</td><td></td></tr><tr><td><code>-oG</code></td><td>save the nmap results in a "<code>grep</code>able" format</td><td></td></tr><tr><td><code>-T[x]</code><br><code>-T [template]</code></td><td>"timing" template(speed of scan(higher speeds risk getting detected))<br><code>x</code>: <code>0</code>|<code>1</code>|<code>2</code>|<code>3</code>|<code>4</code>|<code>5</code><br>according to templates<br><code>template</code>s: <code>paranoid</code>|<code>sneaky</code>|<code>polite</code>|<code>normal</code>|<code>aggressive</code>|<code>insane</code></td><td></td></tr><tr><td><code>-p [port]</code><br><code>-p [from-to]</code><br><code>-p-</code> (scan all)</td><td>port(s) to scan</td><td></td></tr><tr><td><code>--script</code><br><code>--script=[category]</code></td><td>activate a script from the nmap scripting library</td><td></td></tr></tbody></table>
