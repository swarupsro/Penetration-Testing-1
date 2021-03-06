### Port 139/445 - SMB

- Name:
- Version:
- Domain/workgroup name:
- Domain-sid:
- Allows unauthenticated login:


```
nmap --script=smb-enum-shares.nse,smb-ls.nse,smb-enum-users.nse,smb-mbenum.nse,smb-os-discovery.nse,smb-security-mode.nse,smbv2-enabled.nse,smb-vuln-cve2009-3103.nse,smb-vuln-ms06-025.nse,smb-vuln-ms07-029.nse,smb-vuln-ms08-067.nse,smb-vuln-ms10-054.nse,smb-vuln-ms10-061.nse,smb-vuln-regsvc-dos.nse,smbv2-enabled.nse INSERTIPADDRESS -p 445

enum4linux -a INSERTIPADDRESS

rpcclient -U "" INSERTIPADDRESS
	srvinfo
	enumdomusers
	getdompwinfo
	querydominfo
	netshareenum
	netshareenumall

smbclient -L INSERTIPADDRESS
smbclient //INSERTIPADDRESS/tmp
smbclient \\\\INSERTIPADDRESS\\ipc$ -U john
smbclient //INSERTIPADDRESS/ipc$ -U john
smbclient //INSERTIPADDRESS/admin$ -U john

Log in with shell:
winexe -U username //INSERTIPADDRESS "cmd.exe" --system

```

https://www.hackingarticles.in/5-ways-to-hack-smb-login-password/
https://www.hackingarticles.in/3-ways-scan-eternal-blue-vulnerability-remote-pc/
https://www.hackingarticles.in/smb-penetration-testing-port-445/

https://www.hackingarticles.in/netbios-and-smb-penetration-testing-on-windows/

https://medium.com/@arnavtripathy98/smb-enumeration-for-penetration-testing-e782a328bf1b


SMB Enumeration
nmap --script smb-enum-services -p 445 192.168.10.1 -d

enum4linux -S 192.168.10.1

smbmap
-------
smbmap -H 192.168.10.1
smbmap -R [shared dir] -H 192.168.10.1 = list contents
smbmap -R [shared dir] -H 192.168.10.1 -A [filename.xml] -q = download content 

smbmap -d active.htb -u [USER NAME] -p [PASSWORD] -H 192.168.10.1
smbmap -d active.htb -u [USER NAME] -p [PASSWORD] -H 192.168.10.1 -R [Users] = download all contents in a filename Users

smbclient ( -L list shares, -N anonymous logon,)
---------
smbclient -L //192.168.10.1
smbclient -L //10.10.10.100 -N

download all contents in a filename folder[ ! password required]
smbclient //192.168.10.1/filename
>recurse ON
>prompt OFF
>mget *

1. NBTSCAN

root@bt:~# nbtscan -r 10.0.2.0/24
Doing NBT name scan for addresses from 10.0.2.0/24

IP address       NetBIOS Name     Server    User             MAC address      
------------------------------------------------------------------------------
10.0.2.0	Sendto failed: Permission denied
10.0.2.10        <unknown>                  <unknown>        
10.0.2.15        METASPLOITABLE   <server>  METASPLOITABLE   00-00-00-00-00-00
10.0.2.18        TEST01		  <server>  TEST01	     00-11-21-22-1d-4d
10.0.2.45        TEST04	 	  <server>  TEST04           00-12-d2-34-11-55

2. NMAP

nmap -p 1-65535 -T4 -O -A -v 10.0.2.15

3. SMBCLIENT

root@bt:~# smbclient -L=10.0.2.15

Null Sessions

root@bt:~# smbclient \\\\10.0.2.15\\tmp
Enter root's password: 
Anonymous login successful


SMB Enumeration Techniques using Windows Tools:

1. NetBIOS Enumerator (nbtenum)

http://nbtenum.sourceforge.net/
------------------------------------------------------------

SMB\RPC Enumeration (139/445):

    enum4linux –a 10.0.0.1
    nbtscan x.x.x.x // Discover Windows / Samba servers on subnet, finds Windows MAC addresses, netbios name and discover client workgroup / domain
    py 192.168.XXX.XXX 500 50000 dict.txt
    python /usr/share/doc/python-impacket-doc/examples/samrdump.py 192.168.XXX.XXX
    nmap IPADDR --script smb-enum-domains.nse,smb-enum-groups.nse,smb-enum-processes.nse,smb-enum-sessions.nse,smb-enum-shares.nse,smb-enum-users.nse,smb-ls.nse,smb-mbenum.nse,smb-os-discovery.nse,smb-print-text.nse,smb-psexec.nse,smb-security-mode.nse,smb-server-stats.nse,smb-system-info.nse,smb-vuln-conficker.nse,smb-vuln-cve2009-3103.nse,smb-vuln-ms06-025.nse,smb-vuln-ms07-029.nse,smb-vuln-ms08-067.nse,smb-vuln-ms10-054.nse,smb-vuln-ms10-061.nse,smb-vuln-regsvc-dos.nse
    smbclient -L INSERTIPADDRESS
    smbclient //INSERTIPADDRESS/tmp
    smbclient INSERTIPADDRESS ipc$ -U john
