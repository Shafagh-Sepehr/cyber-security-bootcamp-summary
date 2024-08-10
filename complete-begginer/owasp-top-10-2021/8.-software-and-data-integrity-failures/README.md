# 8. Software and Data Integrity Failures

## What is Integrity?

When talking about integrity, we refer to the capacity we have to ascertain that a piece of data remains unmodified. Integrity is essential in cybersecurity as we care about maintaining important data free from unwanted or malicious modifications. For example, say you are downloading the latest installer for an application. How can you be sure that while downloading it, it wasn't modified in transit or somehow got damaged by a transmission error?

To overcome this problem, you will often see a hash sent alongside the file so that you can prove that the file you downloaded kept its integrity and wasn't modified in transit. A hash or digest is simply a number that results from applying a specific algorithm over a piece of data. When reading about hashing algorithms, you will often read about MD5, SHA1, SHA256 or many others available.

Let's take WinSCP as an example to understand better how we can use hashes to check a file's integrity. If you go to their [Sourceforge repository](https://sourceforge.net/projects/winscp/files/WinSCP/5.21.5/), you'll see that for each file available to download, there are some hashes published along:

![WinSCP hashes](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/b93dd140259193ee75ae1d12562bbd29.png)\


These hashes were precalculated by the creators of WinSCP so that you can check the file's integrity after downloading. If we download the `WinSCP-5.21.5-Setup.exe` file, we can recalculate the hashes and compare them against the ones published in Sourceforge. To calculate the different hashes in Linux, we can use the following commands:

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Since we got the same hashes, we can safely conclude that the file we downloaded is an exact copy of the one on the website.

## Software and Data Integrity Failures

This vulnerability arises from code or infrastructure that uses software or data without using any kind of integrity checks. Since no integrity verification is being done, an attacker might modify the software or data passed to the application, resulting in unexpected consequences. There are mainly two types of vulnerabilities in this category:

* Software Integrity Failures
* Data Integrity Failures
