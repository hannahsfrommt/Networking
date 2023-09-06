# Networking Fundamentals
```https://net.cybbh.io/public/networking/latest/lesson-1-fundamentals/fg.html```
# binary identification
01000010 - binary  
66 - decimal  
0x42 - hexidecimal  
QmFzZSA2NA== - base64  

0100 | 0010  
4    |   2  
= 0x42  

| # bits | representation |
|--------|----------------|
| 1      | bit/flag       |
| 4      | nibble         |
| 8      | byte/octet     |
| 16     | half word      |
| 32     | word           |
| 64     | very long word |
   
 
# encapsulation and decapsulation
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/28764f0d-324f-4aff-9ff9-8df5a5608a4c)

# OSI model
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/34050969-ed53-46d1-9fe5-a7ff5d57c0c0)

{3 way handshake: syn, synack, ack}


| OSI layer        |  PDU             | Common Protocols                               |
|------------------|------------------|------------------------------------------------|
| 7. Application   | Data             | DNS, HTTp, Telnet                              |
| 6. Presentation  | Data             | ssl, tls, jpeg, gif                            |
| 5. Session       | Data             | netbios, pptp, rpc, nfs                        |
| 4. Transport     | Segment/Datagram | tcp, udp                                       |
| 3. Network       | Packet           | ip, icmp, igmp                                 |
| 2. Data Link     | Frames           | ppp, atm, 802.2/3 ethernet, frame relay        |
| 1. Physical      | Bits             | bluetooth, usb, 802.11 (wifi), dsl, 1000Base-T |

## important organizations
IETF - mostly known for developing and publishing "white paper" standards knows as Request for Comment (RFC); ex: ipv4, ipv6, tcp, udp, http 1.1  
IANA - controls all internet numbers such as:mac oui numbers, ethertypes, ip addresses, multi-cast addresses, port numbers, protocol numbers, domain names  
IEEE - developed the standards for LAN's such as: 802.11 wireless LAN, 802.3 ethernet  

## physical layer
responsibilities:  
- hardware specifications
- encoding and signaling
- data transmision and reception
- physical network design

## data link layer
### responsibilities:  
- addressing schemes for LAN (physcial addressing)
- error notification
- flow control
- frame sequencing

### sublayers:  
- MAC (media access control)
- LLC (logical link control)

### ethernet header:  
"tmac smacks ethertype"  
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/b37ed25d-4cb9-4a05-95ab-374fec44548c)
#### common ethertypes  
- 0x8000 ipv4
- 0x0806 arp
- 0x86DD ipv6
- 0x8100 vlan tagging 802.1q

### 802.1Q header:  
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/5ae2b37f-8428-40fe-9261-f5d3eccdc94b)

### arp header:
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/197bfd6c-c3db-4685-afea-a0e90f98456a)
arp is a security concern: sending numerous arp requests masquerading itself as the router possibly...man in the middle attacks
arp attack, proxy arp attack

## netowrk layer
responsibilities:
- addressing schees for network (logical addressing)
- routing
- encapsulation
- ip fragmentation and reassembly
- error andling and diagnostics

### ipv4 header:
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/e6c4594b-8ab1-4d18-a312-459ba868170f)

#### dscp/ecn:   
dscp and ecn are in the same byte   
so dscp value is 10 the binary would be 001010|00  
but the value of the binary would be 0x28 or 40  
shortcut: take dscp and multiply by 4 if ecn is still 0 otherwise manually move bytes  

#### flags:  
0    0   0    
res  df  mf  
res - reserved, should always be 0, "evil bit"   
df - dont fragment  
mf - more fragments  
  
  
#### ttl - time to live:
linux - 64 
windows - 128

  
#### protocol:  
0x06 tcp  
0x11 udp  
  
options will be present if IHL > 5  
    
ipv4 header between 20-60 bytes  
  
### fragmentation process:  
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/253a5247-d2c6-47c3-94e6-ab784a4d00a9)  
MTU - maximum transmission unit
ethernet default MTU is 1500 bytes
(MTU - IHL(bytes)) / 8 = offset

### ipv6 header:
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/1ab7e004-934d-4ee6-8982-69732c6ce2e6)

ipv6 does not fragment bc routers dont support it  
source host will fragment its own packets vs the router doing it  

### fingerprinting
making a educated guess on the OS via the ttl  
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/a49c136d-7ad1-44ae-a361-37d6ee156bf9)

### icmp header
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/c0983a4a-3989-4d6e-8784-ac71c46ace6d)  
attacks:  
- fire-walking
- oversized ICMP informational messages
- ICMP drouter discovery messages
- smurf attack

### zero configuration
ipv4 auto configuration   
- apipa
- rfc 3927
ipv6 auto configuration   
- SLAAC (stateless address auto-configuration)
- RFC 4862

## transport layer
connection oriented (tcp-segments-unicast traffic)  
connection-less (udp-datagram-broadcast,multicast,unicast traffic)  

### tcp header
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/4ff7777e-f09e-46c7-8edc-7a6a1808eb24)

#### flags
1 byte  
cwr, ece, urg, ack, psh, rst, syn, fin  
0x12 synack  
some flag combinations are illegal (including no flags is illegal)  

### tcp states/connections
syn > synack > ack  
psh/ack to exchange data  
then 4 way termination
finack > ack+ finack > ack (closed)  
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/2ab019a4-d31e-4304-b922-b02e01fb00b9)

### udp headers
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/56c1ee28-8ea4-408b-beaf-2ca5a5e0a6e0)


## session layer
protocols:
- socks
- netbios
- pptp/l2tp
- rpc

socks 4/5 (tcp 1080) (5 is udp)
- uses various client/server exchange messages
    - client can provide authentication to server
    - client can request connections from server

smb/cifs (tcp 139/445 and udp 137/138)
*smb rides over netbios*
- netbios dgram service - udp 138
- netios session sevice - tcp 139
- samba and cifs are just flavors of smb

rcp (any port)
  rpc is a request/response protocol
  user application will
  1. sends a reuest for information to a external server
  2. receives the info from the external server
  3. display collected data to user


## presentation layer
responsibilities:
- translation
- formating
- encoding
- encryption (symmetric and asymmetric)
- compression


## application layer

### ftp (tcp 20/21)
- messages: ftp commands and ftp reply codes
- modes: active (default), passive

#### ftp active issues
-  nat and firewall traversal issues
-  complications w tunneling through ssh
-  passive ftp solves issues related to active mode ans is most often used in modern systems

### ssh (tcp 22)
#### messages provide:
- client/server authentication
- asymmetric or pki for key exchange
- symmetric for session
- user authentication
- data stream channeling

#### ssh architecture 
- server
- client
- session
- keys
    - user key - asymmetric public key used to identify the user to the server
    - host key - asymmetric public key used to identify the server to the user
    - session key - symmetric key created by the lient ans server to protect the sessions
- key generator
- known-hosts database - collection fo host keys that the client and server use fo mutual authentcation
- agent - sores keys as convenience for users (prevents constatnt passphrase entry prompt)
- signer -s signs the hos-based auhentication packes
- random seed - used for entropy in creating pseudorandom numbers
- configuration file - settings that exist on the client and server to dictate onfiguration of ssh and sshd respectively

40,0,0,196,160,46


40.0.0.196 and 160x256+46=41006 port


#### ssh implementation concerns
- using password authentication only
- key rotation
- key management
- implementation specification (libssh, sshtrangerthings)

### telnet (tcp 23)
messages: telnet commands, telnet options

### smtp (tcp 25)
messages: smtp commands, smtp responses

### TACACS (TCP 49) simple/extended

### HTTP(S) (TCP 80/443)
messages:
- methods: GET/HEAD/POST/PUT
- HTTP status codes: 100, 200, 300, 400

### POP (TCP 110)
messages
- pop commands
- pop replies
- pop capabilities

### IMAP (tcp 143)
messages:
- imap commands
- imap status response
- imap capibilities

### RDP (tcp 3389)
- compression or encrypion support
- desktop size and color depth
- keyboard mapping
- remote system control
- mouse-cursor color properties

### DNS (query/response) (tcp/udp 53)

### DHCP (UDP 67/68)
Discover, Offer, Request, Acknowledge
### TFTP (udp 69)
messages: tftp opcodes, tftp error codes

### NTP (udp 123)

### snmp (udp 161/162)
7 messages types: get request, set request, get next, get bulk, response, trap, inform

### radius (udp 1645/1646 and 1812/1813)

### rtp (udp and above 1023)







# layer 2 switching technologies (datalink layer)
## switch operation
- fast forward - only dst mac
- frag gree - first 64 bytes
- store and forward - entire frame and FCS
## CAM table
- learn - examining the source mac address
- forward - examining the dst mac addy

## VLAN and IEEE 802.1Q
*see slides*
## IEEE 802.1AD "Q-IN-Q"
*see slides*

dont want to have 1 switch for the netire building beacause single point of failure and the whole system goes down therefore we creates STP...
## spanning tree protocol (STP)
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/0a8003a0-b712-4c0e-ba4e-ee28d63df291)
root decision process
1. elect root bridge
2. identify the root ports on non-root bridge
3. identify the designated port for each segment
4. set alternate ports to blocking state

## layer 2 discovery protocols
- cisco discovery protocol (CDP)
- foundry discovery protcol (FDP)
- link layer discovery protocol (LLDP)

## DTP (dynamic trunking protocol)
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/7667e168-d4c3-4598-a879-b434b3ab742c)
## VTP (VLAN trunking protocol)
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/585bd2a3-8b28-4ce6-9ef0-02d151987e1f)

## port security
modes:
-shutdown
restric
protect


# layer 3 routing technologies (network layer)
routers primary functions are to determine he best path to send packets, forward packets toward theri destination

----catch up on notes later lazyyyy rn---------
