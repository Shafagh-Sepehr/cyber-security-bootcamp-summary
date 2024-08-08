---
icon: rectangle-terminal
---

# some Linux web command

`wget` command allows us to download files from the web via HTTP -- as if you were accessing the file in your browser.

e.g.:

`wget https://assets.tryhackme.com/additional/linux-fundamentals/part3/myfile.txt`

***

`scp` command allows us to upload to or download from a machine on the network with having its IP, username and password.

uploads `important.txt` to `ubuntu@192.168.1.30:/home/ubuntu/transferred.txt`:

`scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt`

downloads `notes.txt` from `ubuntu@192.168.1.30:/home/ubuntu/documents.txt notes.txt`

`scp ubuntu@192.168.1.30:/home/ubuntu/documents.txt notes.txt`

***

`python3 -m http.server` is used to start a simple http server that serves the files in the directory where you run the command.

<figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>
