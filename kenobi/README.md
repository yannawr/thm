# KENOBI

```
export IP=10.10.115.244
```
```
sudo nmap -sC -sV -O -oN nmap.out $IP 
```
```
nikto -h $IP | tee nikto.out
```
```
gobuster dir -u $IP -w /opt/dirbuster/wordlists/directory-list-2.3-medium.txt -x html,css,js,php,cgi,txt,py -o gobuster.out
```

open ports:
```
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         ProFTPD 1.3.5
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
80/tcp   open  http        Apache httpd 2.4.18 ((Ubuntu))
111/tcp  open  rpcbind     2-4 (RPC #100000)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
2049/tcp open  nfs_acl     2-3 (RPC #100227)
```
```
/opt/enum4linux/enum4linux.pl -a $IP | tee enum.out
```

```
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse $IP
```
```
smbclient //10.10.115.244/anonymous
```
```
smbget -R smb://10.10.115.244/anonymous
```
```
nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount $IP
```

```
searchsploit proftpd  
```
Exploits:
```
ProFTPd 1.3.5 - 'mod_copy' Command Execution ( | linux/remote/37262.rb
ProFTPd 1.3.5 - 'mod_copy' Remote Command Exec | linux/remote/36803.py
ProFTPd 1.3.5 - 'mod_copy' Remote Command Exec | linux/remote/49908.py
ProFTPd 1.3.5 - File Copy                      | linux/remote/36742.txt
```
