# Getting reverse shell HOWTOs &nbsp; <img src="https://raw.githubusercontent.com/syriusbughunt/Getting_Reverse_Shell/master/img/shell1.png" width="30"/>
After having success with a remote command execution vulnerability, you'll probably wish to get an interactive shell. You will see that your target cannot execute any reverse shell scripting languages; it's limited to the current vulnerable application language.  
  
Before starting, you will need netcat installed on your listener host.
  
**Ubuntu/Debian distrib:** *sudo apt-get install nc -y*  
**Other Linux distros (CentOS, RedHat, ...):** *yum install nc -y*  
Now that netcat is successfully installed on your listener host, type: *nc -l -v 4444*
  
You will find below different ways in obtaining a reverse shell in multiple langs.  
  
## **Bash** &nbsp; <img src="https://raw.githubusercontent.com/syriusbughunt/Getting_Reverse_Shell/master/img/bash.jpg" width="20"/>
