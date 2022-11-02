
#### Remember...if you are stuck reference this:
-> Have i enumerated all service versions?
-> Revisit some scan results, am I seeing everything?

# Initial Recon

rustscan -a IP
nmap -sC -sV -p- IP | tee nmap

# Recon

## Web
#### Intial Probing
- curl -I IP
- Initial probing with source code, loaded in files(network tab)
- Webanalyzer
#### Dir Enums
- ffuf -w -recursion ~/htb/arsenal/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -u "http://IP/FUZZ" | tee paths
- ffuf -w ~/htb/arsenal/SecLists/Discovery/DNS/subdomains-top1million-110000.txt -u "http://IP/" -H "Host: FUZZ.domain.com" -t 50 | tee subdirs
- ffuf -w ~/htb/arsenal/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -u "http://IP/FUZZ" -e ".php,.txt,.html,.js" | tee paths
- ./chopchop scan --url http://IP
#### VulnScanner
- nikto -h http://IP
- wpscan -a3 http://IP 
- 

## LDAP
- ldapsearch -x -H ldap://IP:389 -s base namingcontexts
- ldapsearch -x -H ldap://IP:389 -s sub -b 'DC=urmom,DC=com'

## DNS
- dnsrecon -d urmom.com -n IP -a

## SMB
- smbclient -L //IP
- smbmap -H IP -u '' -p ''
- enum4linux -a IP -u ' ' -p ' '  |||| -u ' ' and -p ' ' are optional for creds
RECURSIVE LIST
- smbmap -H IP -R SHARENAME -u '' -p ''
OTHER STUFF SEE 
- crackmapexec




# Exploitation
- GET VERSIONS OF EVERYTHING AND THEN JUST SEARCHSPLOIT, GOOGLE THAT BITCH.  Remember **ambassador** with grafana bullshit


# PrivEsc

## Active Directory
#### bloodhound
- python3 bloodhound-python -u user -p password -ns IP -d domain.com -c All
- Once ingested into bloodhound unroll permissions of user and their groups.  Do they have any advatagious permissions?


