## Domain controller

NTDS.dit - database that contains info of an active directory domain controller.  Users and hashes.
-> Stored %SystemRoot%\NTDS
->only accessible by domain controller


## Forests, tree, and OUs

->forest
-->tree
--->domain
---->Organizational Unites(OUs)
defined by 
------>Domain Schema

## Users and Groups
Groups
- Domain Admins
- Service Accounts
- Local Admins
- Domain Users
- Security Groups
- Distribution Groups

## Trusts + Policies

Two types of trusts
- Directional = Between two domains flows between
- Transitive = Trust relationship expands beyond just two domains.  Includes other trusted domains.

Domain policies = think group policies

## Domain Services

Services
- LDAP
- Certificate Services
- DNS, LLMNR, NBT-NS
- Kerberos

Domain Authentication
- Kerberos
- NTLM

