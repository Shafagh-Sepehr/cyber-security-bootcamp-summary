---
icon: rectangle-terminal
---

# ssh

Short for secure shell, with this command we remotely log in to a machines account.

This connection is secure and encrypted.

`ssh [username]@[IP]` is the syntax afterwards we get prompted to enter a password and then we log in to that account.

e.g. `ssh root@45.65.98.102` or `ssh jack@10.15.159.205`

<figure><img src="../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

If you have a private rsa key instead of password:

`ssh -i [rsa_key_file] [username]@[IP]`
