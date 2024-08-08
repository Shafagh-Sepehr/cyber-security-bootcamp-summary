---
icon: rectangle-terminal
---

# nslookup

`nslookup --type=[type] domainName.tld`

the above command is used to make dns queries.

type can be:

* A: resolve to IPv4 addresses
* AAAA: resolve to IPv6 addresses
* CNAME: resolves to another domain name
* MX: resolve to the address of the servers that handle the email for the domain you are querying
* TXT: TXT records are free text fields where any text-based data can be stored.

