# Weak File Permissions

### Readable /etc/shadow

just get the root hash and crack it using john

### Writable /etc/shadow

replace the root password hash with `mkpasswd -m sha-512 newpasswordhere`or `openssl passwd -1 -salt [salt] [password]` or `mkpasswd newpasswordhere`'s outpus.

### Writable /etc/passwd

with commands in the previous page, create hash of you desired password and put it between the first and second colon replacing 'x'. (now the password which its hash was in shadow is unusable until you put the 'x' back there)

you can also create a new superuser user by copying the root line and replacing the username and the password hash.
