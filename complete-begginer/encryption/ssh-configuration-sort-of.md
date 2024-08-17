# SSH configuration (sort of)

suppose machine A (your machine) wants to connect to machine B (target machine) over ssh.

machine B has to have `openssh-server` installed (in order to restart: `sudo systemctl restart [sshd.service|sshd]`). and machine A has to have `openssh-client` installed which must be by default.

it goes without saying that the machines have to networked (use `ping` to find out).

## password authentication

then A can connect to B with `ssh [B's username]@[B's IP]` by providing B's password. (unless something in /etc/ssh/ is off and doesn't allow ssh connection by password or something 🤷‍♂️ (google it))

## key authentication

without providing any additional password you can connect to machine B with `ssh -i [rsa_private_key] [B's username]@[B's IP]`, **If** the corresponding public key is in `/home/[B's username]/.ssh/authorized_keys` file in machine B.

if you have the B's password and it allows ssh connection you can do the fallowing to automate authentication procedure:

1. run `ssh-keygen` on machine A (your machine). this creates a pair of RSA keys in `~/.ssh` (names are defaulted to `id_rsa` and `id_rsa.pub`). while doing so you are prompted three times. the first one is self explaneory. the second and third one asks you to enter a passphrase which is used to encrypt the private key (an additional layer of security). If someone gains access to the private key file, they won’t be able to use it without the passphrase.
2. run `ssh-copy-id [B's username]@[B's IP]`. this puts the generated public key in `/home/[B's username]/.ssh/authorized_keys` file in machine B.
3. now you can execute `ssh [B's username]@[B's IP]` without entering a password.

#### you should also know:

* the keys get cached. so if you remove them, you still can connect to B without entering any password until the cache gets cleared. you can see the cache list with `ssh-add -l` and delete its entries with `ssh-add -D`.
* if the public key gets removed from the `authorized_keys` file, you lose access. (you have to connect using password if it's allowed of course; cause some systems forbid that)
* by copying the `/home/[B's username]/.ssh/authorized_keys`  to `/root/.ssh/authorized_keys` you get access to root, but to do so you need root access🤣(I dunno if you have the root password and connected with another username you can execute `sudo su` to become root and do the copying)
* if someone get access to your private key and passphrase(or worse if your private key doesn't need one), they can use it to ssh to machine B. so you need to protect that with a strong passphrase.
* as mentioned ssh configs are found in `/etc/ssh` folder specially the `sshd_config` file.
* Note: If you get an error saying `Unable to negotiate with <IP> port 22: no matching how to key type found. Their offer: ssh-rsa, ssh-dss` this is because OpenSSH have deprecated ssh-rsa. Add `-oPubkeyAcceptedKeyTypes=+ssh-rsa` and `-oHostKeyAlgorithms=+ssh-rsa` to your command to connect.
