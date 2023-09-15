# ACCESS CONTROLS
# WHY FILTER TRAFFIC
- block malicious traffic
- decrease load on network infrastructure
- ensure data flows in an efficient manner
- ensure data gets to intended recipients and only intended recipients
- obfuscate network internals
# PRACTICAL APPLICATIONS FOR FILTERING
- Network Traffic - allow or block traffic to/from remote locations.
- Email addresses - to block unwanted email to reduce risk or increase productivity
- Computer applications in an organization environment - for security from vulnerable software
- MAC filtering - also for security to allow only specific computers access to a network


# DEFAULT POLICIES
- explicit - precisely and clearly expressed
- implicit - implied or understood
# BLOCK-LISTING VS ALLOW-LISTING
Block-Listing (Formerly Black-List):
- Implicit ACCEPT
- Explicit DENY
Allow-Listing (Formerly White-List):
- Implicit DENY
- Explicit ACCEPT
# DISCUSS FILTERING DEVICE TYPES
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/b13eb364-331f-4511-a4b6-64e4c016ad9e)
mostly done on routers
# OPERATION MODES
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/de875760-3813-4192-bdee-b6e72e378eb4)
# FIREWALL FILTERING METHODS
- Stateless (Packet) Filtering (L3+4)
- Stateful Inspection (L4) - think does it have an established connection, 3 way handshake type beat
- Circuit-Level (L5)
- Application Layer (L7)
- Next Generation (NGFW) (L7)
# SOFTWARE VS HARDWARE VS CLOUD FIREWALLS
- software - typically host-base
- Hardware - typically network-based
- Cloud -provided as a service
# TRAFIC DIRECTIONS
A -> B:
Traffic originating from the localhost to the remote-host
- You (the client) are the client sending traffic to the server.
Return traffic from that remote-host back to the localhost.
- The server is responding back to you (the client).
B -> A:
Traffic originating from the remote-host to the localhost.
- A client is trying to connect to you (the server)
Return traffic from the localhost back to the remote-host.
- You (the server) are responding back to the client.

# HOST BASED FILTERING
# WINDOWS, LINUX, OR MAC
- Windows - Norton, Mcafee, ZoneAlarm, Avast, etc.
- Linux - iptables, nftables, pfSense, OPNsense, etc.
- MAC - Little Snitch, LuLu, Vallum, etc.
# NETFILTER FRAMEWORK
made to provide:
- packet filtering
- stateless/stateful Firewalls
- network address and port translation (NAT and PAT)
- other packet manipulation
# NETFILTER HOOKS - > CHAIN
- NF_IP_PRE_ROUTING → PREROUTING
- NF_IP_LOCAL_IN → INPUT
- NF_IP_FORWARD → FORWARD
- NF_IP_LOCAL_OUT → OUTPUT
- NF_IP_POST_ROUTING → POSTROUTING
# NETFILTER PARADIGM
- tables - contain chains
- chains - contain rules
- rules - dictate what to match and what actions to perform on packets when packets match a rule
# SEPARATE APPLICATIONS
Netfilter created several (separate) applications to filter on different layer 2 or layer 3+ protocols.
- iptables - IPv4 packet administration
- ip6tables - IPv6 packet administration
- ebtables - Ethernet Bridge frame table administration
- arptables - arp packet administration

# CONFIGURE IPTABLES FILTERING RULES
# TABLES OF IPTABLES
- filter - default table. Provides packet filtering.
- nat - used to translate private ←→ public address and ports.
- mangle - provides special packet alteration. Can modify various fields header fields.
- raw - used to configure exemptions from connection tracking.
- security - used for Mandatory Access Control (MAC) networking rules.
# CHAINS OF IPTABLES
PREROUTING - packets entering NIC before routing
INPUT - packets to localhost after routing
FORWARD - packets routed from one NIC to another. (needs to be enabled)
OUTPUT - packets from localhost to be routed
POSTROUTING - packets leaving system after routing
# CHAINS ASSIGNED TO EACH TABLE
filter - INPUT, FORWARD, and OUTPUT
nat - PREROUTING, POSTROUTING, INPUT, and OUTPUT
mangle - All chains
raw - PREROUTING and OUTPUT
security - INPUT, FORWARD, and OUTPUT
# COMMON IPTABLE OPTIONS
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/1b6f1b9b-7a5e-46bf-a292-473db87ce824)
# IPTABLES SYNTAX
iptables -t [table] -A [chain] [rules] -j [action]
Table: filter*, nat, mangle
Chain: INPUT, OUTPUT, PREROUTING, POSTROUTING, FORWARD
# IPTABLES RULES SYNTAX
-i [ iface ] (inbound interface)
-o [ iface ] (outbound interface)
-s [ ip.add | network/CIDR ]
-d [ ip.add | network/CIDR ]
# IPTABLES RULES SYNTAX
-p icmp [ --icmp-type { type# | code# } ]
-p tcp [ --sport | --dport { port1 |  port1:port2 } ]
-p tcp [ --tcp-flags { SYN | ACK | PSH | RST | FIN | URG | ALL | NONE } ]
-p udp [ --sport | --dport { port1 | port1:port2 } ]
# IPTABLES RULES SYNTAX
-m to enable iptables extensions:
-m state --state [ NEW | ESTABLISHED | RELATED | UNTRACKED | INVALID ]
-m mac [ --mac-source | --mac-destination ] [mac]
-p [tcp|udp] -m multiport [ --dports | --sports | --ports { port1 | port1:port15 } ]
-m bpf --bytecode [ 'bytecode' ]
-m iprange [ --src-range | --dst-range { ip1-ip2 } ]
# IPTABLES ACTION SYNTAX
ACCEPT - Allow the packet
REJECT - Deny the packet (send an ICMP reponse)
DROP - Deny the packet (send no response)
-j [ ACCEPT | REJECT | DROP ]
# MODIFY IPTABLES
Flush table
iptables -t [table] -F
Change default policy
iptables -t [table] -P [chain] [action]
Lists rules with rule numbers
iptables -t [table] -L --line-numbers
Lists rules as commands interpreted by the system
iptables -t [table] -S
Inserts rule before Rule number
iptables -t [table] -I [chain] [rule num] [rules] -j [action]
Replaces rule at number
iptables -t [table] -R [chain] [rule num] [rules] -j [action]
Deletes rule at number
iptables -t [table] -D [chain] [rule num]
#DEMO
```
sudo iptables -L
sudo iptables -t nat -L (case sensitive)
sudo iptables -t mangle -L
sudo iptables -A INPUT -p  tcp --dport 22  -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport 22 -j ACCEPT
sudo iptables -P  INPUT DROP (changing default policy)
forgot to allow terminator session
sudo iptables -A INPUT -p tcp -m multiports --ports 6010,6011,6012 -j ACCEPT
sudo iptables -L
sudo iptables -A OUTPUT -p tcp -m multiports --ports 6010,6011,6012
sudo iptables -P OUTPUT DROP
still connected we good
sudo iptables -L
sudo iptables -I OUTPUT -d 172.16.82.106 -j DROP

sudo iptables-save > test
cat test
sudo iptables-restore < test

sudo iptables -P INPUT ACCEPT
sudo iptables -P OUTPUT ACCEPT
sudo iptables -L
sudo iptables -F (flush got no rules)
sudo iptables -L

sudo iptables -L --line-numbers
sudo iptables -D INPUT 1
sudo iptables -D OUTPUT 1
sudo iptables -L --line-numbers
```


# CONFIGURE NFTABLES FILTERING RULES
# NFTABLE FAMILIES
- ip - IPv4 packets
- ip6 - IPv6 packets
- inet - IPv4 and IPv6 packets
- arp - layer 2
- bridge - processing traffic/packets traversing bridges.
- netdev - allows for user classification of packets - nftables passes up to the networking stack (no counterpart in iptables)
# NFTABLE ENHANCEMENTS
One table command to replace:
- iptables
- ip6tables
- arptables
- ebtables
simpler, cleaner syntax
less code duplication resulting in faster execution
simultaneous configuration of IPv4 and IPv6
# NFTABLES HOOKS
- ingress - netdev only
- prerouting
- input
- forward
- output
- postrouting
# INTRODUCES CHAIN-TYPES
There are three chain types:
- filter - to filter packets - can be used with arp, bridge, ip, ip6, and inet families
- route - to reroute packets - can be used with ip and ipv6 families only
- nat - used for Network Address Translation - used with ip and ip6 table families only
# NFTABLES SYNTAX
## 1. CREATE THE TABLE
nft add table [family] [table]
[family] = ip*, ip6, inet, arp, bridge and netdev.
[table] = user provided name for the table.
## 2. CREATE THE BASE CHAIN
nft add chain [family] [table] [chain] { type [type] hook [hook]
    priority [priority] \; policy [policy] \;}
* [chain] = User defined name for the chain.
* [type] =  can be filter, route or nat.
* [hook] = prerouting, ingress, input, forward, output or
         postrouting.
* [priority] = user provided integer. Lower number = higher
             priority. default = 0. Use "--" before
             negative numbers.
* ; [policy] ; = set policy for the chain. Can be
              accept (default) or drop.
 Use "\" to escape the ";" in bash
## 3. CREATE A RULE IN THE CHAIN
nft add rule [family] [table] [chain] [matches (matches)] [statement]
* [matches] = typically protocol headers(i.e. ip, ip6, tcp,
            udp, icmp, ether, etc)
* (matches) = these are specific to the [matches] field.
* [statement] = action performed when packet is matched. Some
              examples are: log, accept, drop, reject,
              counter, nat (dnat, snat, masquerade)
# RULE MATCH OPTIONS
ip [ saddr | daddr { ip | ip1-ip2 | ip/CIDR | ip1, ip2, ip3 } ]
tcp flags { syn, ack, psh, rst, fin }
tcp [ sport | dport { port1 | port1-port2 | port1, port2, port3 } ]
udp [ sport| dport { port1 | port1-port2 | port1, port2, port3 } ]
icmp [ type | code { type# | code# } ]
# RULE MATCH OPTIONS
ct state { new, established, related, invalid, untracked }
iif [iface]
oif [iface]
# MODIFY NFTABLES
nft { list | flush } ruleset
nft { delete | list | flush } table [family] [table]
nft { delete | list | flush } chain [family] [table] [chain]
List table with handle numbers
nft list table [family] [table] [-a]
Adds after position
nft add rule [family] [table] [chain] [position <position>] [matches] [statement]
Inserts before position
nft insert rule [family] [table] [chain] [position <position>] [matches] [statement]
Replaces rule at handle
nft replace rule [family] [table] [chain] [handle <handle>] [matches] [statement]
Deletes rule at handle
nft delete rule [family] [table] [chain] [handle <handle>]
To change the current policy
nft add chain [family] [table] [chain] { \; policy [policy] \;}
# NFTables DEMO
```
sudo nft add table ip MyTable
sudo nft list table MyTable
sudo nft add chain ip MyTable Input { type filter hook input priority 0 \; policy accept \.}
sudo nft add chain ip MyTable output { type filter hook output priority 0 \; policy accept \.}
sudo nft add rule ip MyTable Input tcp sport 22 accept
sydo nft add rule ip MyTable Input tcp dport 22 accept
sudo nft add rule ip Table output tcp sport 22 accept
sudo nft add rule ip Table output tcp dport 22 accept
sudo nft list table MyTable
sudo nft add rule ip MyTable Input tcp dport { 6010,6011,6012 } accept
(do same as ssh and re list the table to see)
sudo nft delete rule ip MyTable Input 8
sudo nft add chain ip MyTable Input { \; policy accept \; }

```





# CONFIGURE IPTABLES NAT RULES
# NAT & PAT OPERATORS & CHAINS
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/587577b4-3a38-4379-a041-961dcc7aab7a)


look at slideshow bc this shit is way too much



