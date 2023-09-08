# Section 2: Networking: Packet Crafting and Socket Programming

## socket types
stream sockets - connecion oriented sequenced; methods for connection est and tear down. used w tcp, sctp, and bluetooth  
datagram sockets - connectionless; designed for quickly sending and receiving data. used w udp
raw sockets - direct sending and receiving of ip packets withour automatic protocol-specific formatting

## user vs kernel what sockets?
user space - stream/datagram sockets
kernel space - raw sockets

## socket creation and privilege level
- User Space Sockets
    - The most common sockets that do not require elevated privileges to perform actions on behalf of user applications.
- Kernel Space Sockets
    - Attempts to access hardware directly on behalf of a user application to either prevent encapsulation/decapsulation or to create packets from scratch, which requires elevated privileges.

## user space applications/sockets
- Using tcpdump or wireshark to read a file
- Using nmap with no switches
- Using netcat to connect to a listener
- Using netcat to create a listener above the well known port range(1024+)
- Using /dev/tcp or /dev/udp to transmit data

## kernel space applications/sockets
- Using tcpdump or wireshark to capture packets on the wire
- Using nmap for OS identification or to set specific flags when scanning
- Using netcat to create a listener in the well known port range (0 - 1023)
- Using Scapy to craft or modify a packet for transmission

## python terminology
libraries > modules > functions,exceptions,constants,objects,types


## netowrk programming w python3
- network sockets primarily use the python3 socket library and socket.socket function
```
import socket
  s = socket.socket(socket.FAMILY, socket.TYPE, socket.PROTOCOL)
```
- Inside the socket.socket. function, you have these arguments, in order:
```
socket.socket([*family*[,*type*[*proto*]]])
```
- family constants should be: AF_INET (default), AF_INET6, AF_UNIX
- type constants should be: SOCK_STREAM (default), SOCK_DGRAM, SOCK_RAW
- proto constants should be: 0 (default), IPPROTO_RAW

https://docs.python.org/3/library/socket.html
https://docs.python.org/3/library/struct.html
https://docs.python.org/3/library/sys.html
https://docs.python.org/3/tutorial/errors.html
https://docs.python.org/3/library/exceptions.html

## raw ipv4 sockets
- Raw Socket scripts must include the IP header and the next headers.
- Requires guidance from the "Request for Comments" (RFC)to follow header structure properly.
    - RFCs contain technical and organizational documents about the Internet, including specifications and policy documents.
- See RFC 791, Section 3 - Specification for details on how to construct an IPv4 header.
## raw socket use case
- Testing specific defense mechanisms - such as triggering and IDS for an effect, or filtering
- Avoiding defense mechanisms
- Obfuscating data during transfer
- Manually crafting a packet with the chosen data in header fields


how to check python script  


stream    
python3 <name.py>  
echo "I got your message" | nc -l -p 12345  


dgram  
python3 <name.py>  
echo "I got your message" | nc -l -p 54321 -u  


raw  
sudo python3 <name.py>  
sudo tcpdump -X ip[9]=0x10  
-filtering on chaos   
