---
icon: database
---

# mysql

## When you would begin attacking MySQL

MySQL is likely not going to be the first point of call when getting initial information about the server. You can, as we have in previous tasks, attempt to brute-force default account passwords if you really don't have any other information; however, in most CTF scenarios, this is unlikely to be the avenue you're meant to pursue.

## The Scenario

Typically, you will have gained some initial credentials from enumerating other services that you can then use to enumerate and exploit the MySQL service. As this room focuses on exploiting and enumerating the network service, for the sake of the scenario, we're going to assume that you found the credentials: "root:password" while enumerating subdomains of a web server. After trying the login against SSH unsuccessfully, you decide to try it against MySQL.

to connect to the remote MySQL server we'll use `default-mysql-client`.

we can use Metasploit for mysql enumeration.

## Alternatives

It's worth noting that everything we will be doing using Metasploit can also be done either manually or with a set of non-Metasploit tools such as nmap's mysql-enum script: [https://nmap.org/nsedoc/scripts/mysql-enum.html](https://nmap.org/nsedoc/scripts/mysql-enum.html) or [https://www.exploit-db.com/exploits/23081](https://www.exploit-db.com/exploits/23081).

`mysql -h [IP] -u [username] -p`

we use `mysql_sql` module of metasploit to enumerate(which needs the mysql username and password but we assume that we got those). first we run it with `SQL` option defaulted to `select version()` which returns the mysql server version. and then with the `SQL` option set to `show databases` to get the name of the databases it contains.

## Schema:

In MySQL, physically, a _schema_ is synonymous with a _database_. You can substitute the keyword "SCHEMA" instead of DATABASE in MySQL SQL syntax, for example using CREATE SCHEMA instead of CREATE DATABASE. It's important to understand this relationship because some other database products draw a distinction. For example, in the Oracle Database product, a _schema_ represents only a part of a database: the tables and other objects owned by a single user.

In MySQL hashes can be used in different ways, for instance to index data into a hash table. Each hash has a unique ID that serves as a pointer to the original data. This creates an index that is significantly smaller than the original data, allowing the values to be searched and accessed more efficiently.

now we use `mysql_schemadump` and `mysql_hashdump` to gather more data. from the last one we find a username and password hash pair(`[username]:*[hash]`).

we then use the `john hash.txt` to crack the password.
