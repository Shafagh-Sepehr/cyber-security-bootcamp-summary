---
icon: rectangle-terminal
---

# NFS

## What is NFS?

NFS stands for "Network File System" and allows a system to share directories and files with others over a network. By using NFS, users and programs can access files on remote systems almost as if they were local files. It does this by mounting all, or a portion of a file system on a server. The portion of the file system that is mounted can be accessed by clients with whatever privileges are assigned to each file.

## How does NFS work?

First, the client will request to mount a directory from a remote host on a local directory just the same way it can mount a physical device. The mount service will then act to connect to the relevant mount daemon using RPC.

The server checks if the user has permission to mount whatever directory has been requested. It will then return a file handle which uniquely identifies each file and directory that is on the server.

If someone wants to access a file using NFS, an RPC call is placed to NFSD (the NFS daemon) on the server. This call takes parameters such as:

* &#x20;The file handle
* &#x20;The name of the file to be accessed
* &#x20;The user's, user ID
* &#x20;The user's group ID\


These are used in determining access rights to the specified file. This is what controls user permissions, I.E read and write of files.

## What runs NFS?

Using the NFS protocol, you can transfer files between computers running Windows and other non-Windows operating systems, such as Linux, MacOS or UNIX.

A computer running Windows Server can act as an NFS file server for other non-Windows client computers. Likewise, NFS allows a Windows-based computer running Windows Server to access files stored on a non-Windows NFS server.

The first of which is key to interacting with any NFS share from your local machine: `nfs-common`.

## NFS-Common

It is important to have this package installed on any machine that uses NFS, either as client or server. It includes programs such as: **lockd**, **statd**, **showmount**, **nfsstat**, **gssd**, **idmapd** and **mount.nfs**. Primarily, we are concerned with "**showmount**" and "**mount.nfs**" as these are going to be most useful to us when it comes to extracting information from the NFS share. If you'd like more information about this package, feel free to read: https://packages.ubuntu.com/jammy/nfs-common.

## **Mounting NFS shares**

Your client’s system needs a directory where all the content shared by the host server in the export folder can be accessed. You can create\
this folder anywhere on your system. Once you've created this mount point, you can use the "mount" command to connect the NFS share to the mount point on your machine like so:

`sudo mount -t nfs [IP]:[share] /tmp/mount/ -nolock`

| Tag            | Function                                                                     |
| -------------- | ---------------------------------------------------------------------------- |
| `sudo`         | Run as root                                                                  |
| `mount`        | Execute the mount command                                                    |
| `-t nfs`       | Type of device to mount, then specifying that it's NFS                       |
| `[IP]:[share]` | The IP Address of the NFS server, and the name of the share we wish to mount |
| `-nolock`      | Specifies not to use NLM locking                                             |

## What is root\_squash?

By default, on NFS shares- Root Squashing is enabled, and prevents anyone connecting to the NFS share from having root access to the NFS volume. Remote root users are assigned a user “nfsnobody” when connected, which has the least local privileges. Not what we want. However, if this is turned off, it can allow the creation of SUID bit files, allowing a remote user root access to the connected system.

## SUID

So, what are files with the SUID bit set? Essentially, this means that the file or files can be run with the permissions of the file(s) owner/group. In this case, as the super-user. We can leverage this to get a shell with these privileges!

## Method

This sounds complicated, but really- provided you're familiar with how SUID files work, it's fairly easy to understand. We're able to upload files to the NFS share, and control the permissions of these files. We can set the permissions of whatever we upload, in this case a bash shell executable. We can then log in through SSH, as we did in the previous task- and execute this executable to gain a root shell!

\
we download `/bin/bash` from the target machine to out machine. then we make sure that root owns it with `sudo chown root bash`, then we give it special permission with `sudo chmod +s bash` (known as SUID). a file with special permission always executes as the user who owns the file, regardless of the user passing the command(we can double check with `ls -la bash`). then we run it with the `-p` which persists the permissions, so that it can run as root with SUID- as otherwise bash will sometimes drop the permissions. we can do this because root\_squashing is off in the nfs which is a misconfiguration.
