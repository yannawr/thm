# Hashing - Crypto 101

## Password Cracking

```
sudo john --wordlist=~/../../usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt --format=raw-SHA256 password.txt  
```

## HASH-ID

```
python3 ~/hash-id/hash-id.py
```

## FIND JOHN FORMAT

```
john --list=formats | grep -iF "md5"
```

## HOW TO UNSHADOW

cat etchashes.txt                                                                                              
```
This is everything I managed to recover from the target machine before my computer crashed... See if you can crack the hash so we can at least salvage a password to try and get back in.

root:x:0:0::/root:/bin/bash
root:$6$Ha.d5nGupBm29pYr$yugXSk24ZljLTAZZagtGwpSQhb3F2DOJtnHrvk7HI2ma4GsuioHp8sm3LJiRJpKfIf7lZQ29qgtH17Q/JDpYM/:18576::::::
```

local_passwd:
```
root:x:0:0::/root:/bin/bash
```

local_shadow:
```
root:$6$Ha.d5nGupBm29pYr$yugXSk24ZljLTAZZagtGwpSQhb3F2DOJtnHrvk7HI2ma4GsuioHp8sm3LJiRJpKfIf7lZQ29qgtH17Q/JDpYM/:18576::::::
```

john command:
```
unshadow local_passwd local_shadow > unshadowed.txt

john --wordlist=~/../../usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt unshadowed.txt 
```

## JOHN --SINGLE

```
john --single --format=FORMAT file.txt
```

## ZIP2JOHN / RAR2JOHN / SSH2JOHN

how to crack zip password
```
zip2john zipfile.zip > output.txt
```

then 
```
john --wordlist=~/../../usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt output.txt 
``` 