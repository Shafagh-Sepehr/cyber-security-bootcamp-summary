---
icon: rectangle-terminal
description: we use it to crack passwords.
---

# john the ripper

to crack `[username]:*[hash]`:

`john hash.txt`

***

here are multiple versions of John, the standard "core" distribution, as well as multiple community editions- which extend the feature set of the original John distribution. The most popular of these distributions is the "Jumbo John".

### **Building from Source for Linux**

If you wish to build the package from source to meet your system requirements, you can do this in five fairly straightforward steps. Further advice on the installation process and how to configure your build from source can be found [here.](https://github.com/openwall/john/blob/bleeding-jumbo/doc/INSTALL)\


1. &#x20;Use `git clone https://github.com/openwall/john -b bleeding-jumbo john` to clone the jumbo john repository to your current working
2. &#x20;Then `cd john/src/` to change your current directory to where the source code is.&#x20;
3. Once you're in this directory, use `./configure` to check the required dependencies and options that have been configured.
4. If you're happy with this output, and have installed any required dependencies that are needed, use `make -s clean && make -sj4` to build a binary of john. This binary will be in the above run directory, which you can change to with `cd ../run`
5. You can test this binary using ./john --test

***

#### basic syntax:

`john [options] [path to file]`

#### **Automatic Cracking:**

`john --wordlist=[path to wordlist] [path to file]`

#### **Identifying Hashes:**

Sometimes John won't play nicely with automatically recognising and loading hashes, that's okay! We're able to use other tools to identify the hash, and then set john to use a specific format. There are multiple ways to do this, such as using an online hash identifier like [this](https://hashes.com/en/tools/hash\_identifier) one. I like to use a tool called [hash-identifier](https://gitlab.com/kalilinux/packages/hash-identifier/-/tree/kali/master), a Python tool that is super easy to use and will tell you what different types of hashes the one you enter is likely to be, giving you more options if the first one fails.

To use hash-identifier, you can just pull the python file from gitlab using: `wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py`.

Then simply launch it with `python3 hash-id.py` and then enter the hash you're trying to identify- and it will give you possible formats!

**Format-Specific Cracking**

Once you have identified the hash that you're dealing with:

`john --format=[format] --wordlist=[path to wordlist] [path to file]`

#### Example Usage:

john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash\_to\_crack.txt

A Note on Formats:

When you are telling john to use formats, if you're dealing with a standard hash type, e.g. md5 as in the example above, you have to prefix it with `raw-` to tell john you're just dealing with a standard hash type, though this doesn't always apply. To check if you need to add the prefix or not, you can list all of John's formats using `john --list=formats` and either check manually, or grep for your hash type using something like `john --list=formats | grep -iF "md5"`.

***

