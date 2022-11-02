# Quick Reference for Services
Listing:
- [SNMP](#snmp)
- [SMTP](#smtp)
- [POP3](#pop3)

## <a name="snmp"></a> SNMP
Check for snmp walk n shit


## <a name="smtp"></a> smtp
connect with:
```
telnet IP 25
```

| command | output |
|    :---:	|	:---: |
| helo | enumerate domain |
| VRFY root | list out the user 'root' |
| EXPN root | list out the user 'root' |

Send an email
```
mail from: urmom@example.com
++
rcpt to: validuser@validdomain.com
++
DATA
++
Write whatever
++
Subject: oh yeah yeah
++
.
++ Email sent
QUIT
```


## <a name="pop3"></a> pop3
connect with:
```
telnet IP 110
```

| command | output |
|    :---:	|	:---: |
| USER example | logins as example |
| PASS password | use password password |
| LIST | list the emails saved |
| RETR 1 | retrieve email 1 |

