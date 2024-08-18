# file transfer between two machines

if you have the target password, use scp

you can use python http server

or this:

listen on the target machine: `nc -vlp [port] > file`

then send the file from the other one: `nc -n [target ip] [port] < root.service`
