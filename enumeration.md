## Enumeration
> If you don't have a solution it's probably because you haven't enumerated everything!

### Services
**http**
```sh
Gobuster:
gobuster -w /usr/share/wordlists/dirbuster/wordlist.txt -u http://bruh.com -x php -t 80
```

```sh
Burpsuite:

Proxy stuff

intruder stuff

intercept stuff

repeater stuff
```

```sh
nmap scripts:
nmap --script=/usr/share/nmap/scripts/script.nse 192.168.1.1 
```

```sh
wpscan:
wpscan --url http://bruh.com
```

**smb**
```sh
smbmap
```

```sh
smbclient
```

```sh
nmap -A -sV
```



**ftp**

```sh
nmap scripts, checking for anonymous login
```
```sh
wget grab all:
wget -rm ftp://anonymous:anonymous@192.168.1.1
```

***snmp***
**dns**
**ldap**
```sh
nmap scripts for ldap:

```

**mysql**

**mssql**

**smtp**

**rdp**

**postgres**

**oracledb**

**nfs**

**rpc**

**ntp**






## Exploitation

**Searchsploit**
```sh
Using searchsploit:
searchsploit term

searchsploit -m exploit.py
```
**CVE Research**
```sh
Google

dogpile
```



## Once shell is achieved

Upgrading shell:
python -c "import pty; pty.spawn('/bin/bash')"
ctrl+z
stty raw -echo
fg
export TERM=xterm
