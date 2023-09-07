# tcpdump demo

```tcpdump```
  command not found
```echo $PATH```
  located elsewhere
```sudo whereis tcpdump```
  using sudo to find the path because it is installed
```sudo tcpdump```
  using tcpdump
```sudo tcpdump -D```
  -D lists interface
```sudo tcpdump -i eth0 'not port 22'```
  -i to specify interface
```sudo tcpdump -i eth0 -n 'not port 22'```
  -n option gets rid of service - port # resolution and ip to domain name resolution which the computer discovers dueto the files below
```cat /etc/hosts```
```cat /etc/services```
```sudo tcpdump -i eth0 -XXn 'not port 22'```
  -XX shows hex and ascii dump
```sudo tcpdump -i eth0 -XXvvn 'not port 22'```
  -vv verbose giving more information
```sudo tcpdump -i eth0 -XXn 'not port 22' -w target.pcap```
  -w writing it to a file
  -r would read it
```sudo tcpdump -i eth0 -XXvvn 'not port 22 && not port 23'```
  can use logical operators to combine filters for example
  && || !
```sudo tcpdump -i eth0 -XXvvn 'src host 10.10.0.40 && not port 22'```
  specifying the source ip


# Berkley Packet Filters

tcpdump {A} [B:C] {D} {E} {F} {G}

A = Protocol (ether | arp | ip | ip6 | icmp | tcp | udp)
B = Header Byte offset
C = optional: Byte Length. Can be 1, 2 or 4 (default 1)
D = optional: Bitwise mask (&)
E = Operator (= | == | > | < | <= | >= | != | () | << | >>)
F = Result of Expression
G = optional: Logical Operator (&& ||) to bridge expressions

example:
```
tcpdump 'ether[12:2] = 0x0800 && (tcp[2:2] != 22 || tcp[2:2] != 23)'
```
0x0800 meaning ipv4
not port 22 or 23 going to 



# bitwise masking

ip[0] & 0x0F > 0x05
looking at the ip at index 0 then check at bitwise mask 0x0F is larger than 0x05


mask being 0x0F = 00001111
therefore only looking at the last 4 bits since they are turned on
so 0100|0101=5 doesnt work because it is equal to 0x05 and cant match
but if its persay 1111|0111 it would work
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/c53550d6-9133-45ce-a2f7-5b441a06e75d)

most exclusive:
tcp[13] = 0x11
tcp[13] & 0xFF = 0x11
at all bits must equal 0x11

less exclusive:
tcp[13] & 0x11 = 0x11
only match the flags your looking for, dont care about the other flags if they are on or not 

least exclusive:
tcp[13] & 0x11 !=0

![image](https://github.com/hannahsfrommt/Networking/assets/140441321/d5f34461-f816-4f79-a6dd-5dba63d94805)

more exlusive:
ip[12:4] & 0xFFFFFFFF = 0x0A0A0028
looking at ip at index 12 and looking at the next 4 bytes ... src ip
bitwise mask 0xFFFFFFFF looking at all the bits
looking at src ip of 10.10.0.40

less exclusive
ip[12:4] & 0xFFFFFF00 = 0x0A0A0028

ip[1] & 0xFC = 80
dscp and ecn field but only care about the dscp field so make a mask that only covers the one field
8 4 2 1 | 8 4 2 1 
dscp takes up 6 bits of the 8 in the 1 byte so =0xFC

ip[6] & E0 = 0x20
more fgragments coming, this is first fragment
(ip[6] & E0 = 0x20 || ip[6:2] & 0x1FFFF > 0)





ip[8] < 0x41 || ip6[7] < 0x41
ip[6] & 0x40 = 0x40
tcp[0:2] > 0x400 || udp[0:2] > 0x400
ip[9] = 0x11 || ip6[6] = 0x11
tcp[13] = 0x14 || tcp[13] = 0x11
ip[4:2] = 0xD5
ether[12:2] = 0x8100
tcp[0:2] = 53 || tcp[2:2] = 53 || udp[0:2] = 53 || udp[2:2] = 53
tcp[13] = 0x02
tcp[13] = 0x12
tcp[13] = 0x04
tcp[2:2] < 1024 || udp[2:2] < 1024
tcp[0:2] = 80 || tcp[2:2] = 80
tcp[0:2] = 23 || tcp[2:2] = 23
ether[12:2] = 0x0806
ip[6] & 0x80 = 0x80
ip[9] = 0x10
ip[1]>>2 = 37
(ip[9] =0x01 || ip[9] = 0x11) && ip[8] = 1 
tcp[13] & 0x20 = 0 && tcp[18:2] > 0
ip[16:4] = 0x0A0A0A0A && tcp[13] = 0
ether[12:4]&0xFFFF0FFF=0x81000001&&ether[16:4]&0xFFFF0FFF=0x8100000a

?? ether[20:2] = 0x0800 && ether[15] = 1 && ether[19] = 10
?? ether[12:2] = 0x8100 && ether[15] = 1 && ether[16:2] = 0x8100 && ether[19] = 10
