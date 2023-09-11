# Network Reconnaissance
: active, passive, external, internal
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/b146555d-0e1a-4cde-a71f-356c43237164)
Active External: ping scans, port scans
Active Internal: ARP requests, DNS queries, ping scans, port scans
Passive External: dns queries, whois queries, shodan, google-fu
passive internal: active sniffing, situational awareness

## situational awareness
where am i? uname -n, hostname
who am i? whoami, id
who else is on here? who, w
what am i allowed to do? sudo -l
what ports are open? netstat -anlptu, ss -nlptu (sudo to see process list if have permission)
are there any special services running? ps -elf
what interfaces do I have? ifconfig, ip addr (ip a)
what other hosts does this box know? arp -a, ip neigh (ip n)
what routes/networks does this host know? route, ip route (ip r)


router is usally first or last ip address in your network

## Passive Recon
- gather info about targets without direct interation
- not as straight forward and requires more time than active recon
- lower risk of discovery
- involved identifying
    - ip addr and subdomains
    - external and 3rd party sites
    - people and tech
    - vulnerabilities
- Possible tools for gathering:
    - WHOIS queries
    - Job site listings
    - Phone Numbers
    - Google searches
Passive OS fingerprinting
### Passive External
- info gathered outside of the network using passive methods
- allows for more efficient attacks and plans
#### DNS
- resolves hostnames to IP addresses
- RFC 3912
- whois queries
#### DIG
- typically between primary and secondary DNS servers
- if allowed to transfer externally hostnames, IPs, and IP blocks can be determined
#### Zone Transfers
- returns DNS info
- supplements base queries
#### Host History
- netcraft
- wayback machine.... archive.org
#### Google Searches
- subdomains
- tech
#### Shodan
- reveals info about tech, remote, access services, imporoperly configured services, and network infrastructure
- when selected can give additional info and applicable vulnerabilities
- shodan.io



#Demo
whois google.com
dig google.com
dig google.com MX
dig google.com SOA
dig gooel.com AAAA
dig google.com TXT
dig axfr @nsztm1.digi.ninja zonetransfer.me
in google type: site:*ccboe.net "Powered by"




### network scanning
- scanning strategy
    - remote to local
    - local to remote
    - local to local
    - remote to remote
- scanning approach
    - aim
        - wide range target scan
        - target specific scan
    - method
        - single source scan
        - distributed scan
- broadcast ping and ping sweep
- arp scan
- syn scan (aka tcp half open scan)
- full connect scan
- null scan (idle scan)
- fin scan
- xmas tree scan (illegal combo of flags)
- udp scan
- ack/window scan
- rpc scan
- ftp scan
- decoy scan
- os fingerprinting scan
- version scan
- protocol ping
- discovery probes
```
nmap [options] [target ip/subnet]
nc [options] [target ip] [target port]
```
demo:
on router:
ssh vyos@172.16.20.1
w
whoami
which sudo
sudo -l
ip a
ip n
ip r
show int
show config
exit

nmap (lists all options)
nmap 172.16.82.106
nmap 172.16.82.106 -Pn (skip host disvoery treat every host as online)
nmap 172.16.20.1 -p 3000-3099 -T4 --min-rate 10000 -A -vvvv(send 10000 packets a minute, t4 means agressively can see everything dont care about being seen, seeing if that port range is open, -A means all, -vvvvv more verbose not sitting waiting forever)
can give ip a range or cidr in nmap scan
172.16.20.1-10 or 172.16.20.1/29 or 172.16.20.1,172.16.20.2
ports aswell range 
1-1000 or 1-1000,2552 or '1-65535 is same as 1-' or -1000 or -p- (looks at all ports)


ctrl+shift+v



nc <options> <ip> <port>
curl <URL or IP:[port]>
wget<URL or IP:[port]>/<filename>
eog <filename>
