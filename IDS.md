# SNORT RULES
## SNORT IDS/IPS RULE - HEADER
[action] [protocol] [s.ip] [s.port] [direction] [d.ip] [d.port] ( match conditions ;)
Action - such as alert, log, pass, drop, reject
Protocol - includes TCP, UDP, ICMP and others
Source IP address - single address, CIDR notation, range, or any
Source Port - one, multiple, any, or range of ports
Direction - either inbound or in and outbound
Destination IP address - options mirror Source IP
Destination port - options mirror Source port

## SNORT IDS/IPS GENERAL RULE OPTIONS:
msg - specifies the human-readable alert message
reference - links to external source of the rule
sid - used to uniquely identify Snort rules
rev - uniquely identify revisions of Snort rules
Classtype - used to describe what a successful attack would do
priority - level of concern (1 - really bad, 2 - badish, 3 - informational)
metadata - allows a rule writer to embed additional information about the rule

## SNORT IDS/IPS PAYLOAD DETECTION OPTIONS:
content - looks for a string of text.
|binary data| - to look for a string of binary HEX
nocase - modified content, makes it case insensitive
depth - specify how many bytes into a packet Snort should search for the specified pattern
distance - how far into a packet Snort should ignore before starting to search for the specified pattern relative to the end of the previous pattern match
within - modifier that makes sure that at most N bytes are between pattern matches using the content keyword
offset - skips a certain number of bytes before searching (i.e. offset: 12)

## SNORT IDS/IPS NON-PAYLOAD DETECTION OPTIONS:
Flow - direction (to/from client and server) and state of connection (established, stateless, stream/no stream)
ttl - The ttl keyword is used to check the IP time-to-live value.
tos - The tos keyword is used to check the IP TOS field for a specific value.
ipopts - The ipopts keyword is used to check if a specific IP option is present
seq - check for a specific TCP sequence number
ack - check for a specific TCP acknowledge number.
flags - The flags keyword is used to check if specific TCP flag bits are present.
itype - The itype keyword is used to check for a specific ICMP type value.
icode - The icode keyword is used to check for a specific ICMP code value.
logto - The logto keyword tells Snort to log all packets that trigger this rule to a special output log file.
session - The session keyword is built to extract user data from TCP Sessions.
react - This keyword implements an ability for users to react to traffic that matches a Snort rule by closing connection and sending a notice.
tag - The tag keyword allow rules to log more than just the single packet that triggered the rule.
activates - This keyword allows the rule writer to specify a rule to add when a specific network event occurs.
activated_by - This keyword allows the rule writer to dynamically enable a rule when a specific activate rule is triggered.
count - Allows the rule writer to specify how many packets to leave the rule enabled for after it is activated.

## SNORT IDS/IPS THRESHOLDING AND SUPPRESSION OPTIONS:
type [limit | threshold | both]
limit - alerts on the 1st event during defined period then ignores the rest.
Threshold - alerts every [x] times during defined period.
Both - alerts once per time internal after seeing [x] amount of occurrences of event. It then ignores all other events during period.

## SNORT RULE EXAMPLE
Look for anonymous ftp traffic:
```alert tcp any any -> any 21 (msg:"Anonymous FTP Login"; content: "anonymous"; sid:2121; )```
This will cause the pattern matcher to start looking at byte 6 in the payload)
```alert tcp any any -> any 21 (msg:"Anonymous FTP Login"; content: "anonymous"; offset:5; sid:2121; )```
This will search the first 14 bytes of the packet looking for the word “anonymous”.
```alert tcp any any -> any 21 (msg:"Anonymous FTP Login"; content: "anonymous"; depth:14; sid:2121; )```
Deactivates the case sensitivity of a text search.
```alert tcp any any -> any 21 (msg:"Anonymous FTP Login"; content: "anonymous"; nocase; sid:2121; )```

## RULE HEADER
ICMP ping sweep
```alert icmp any any -> 10.1.0.2 any (msg: "NMAP ping sweep Scan"; dsize:0; sid:10000004; rev: 1; )```
Look for a specific set of Hex bits (NoOP sled)
```alert tcp any any -> any any (msg:"NoOp sled"; content: "|9090 9090 9090|"; sid:9090; rev: 1; )```
Incorrect telnet login attempt
```
alert tcp any 23 -> any any (msg:"TELNET login incorrect"; content:"Login incorrect";
flow:established,from_server; classtype:bad-unknown; sid:2323; rev:6; )
```

## SNORT DEMONSTRATION
```
ls -l /etc/snort
cat /etc/snort/snort.conf
cd /etc/snort/rules
ls
cat icmp.rules
sudo snort -D -l /var/log/snort/ -c /etc/snort/snort.conf
ps -elf | grep snort
ping 10.2.0.2 -c 3
cd /var/log/snort
ls -l
cat alert
sudo file snort.log.1695128611
sudo tcpdump -r snort.log.1695128611
sudo kill -9 <pid of the sudo snort -D... command>
pe -elf | grep snort
sudo snort -r snort.log.1695128611
sudo snort -r snort.log.1695128611 -c /etc/snort/snort.conf
sudo vim /etc/snort/rules/icmp.rules


```

## FAILED IDS/IPS
Fail open
Fail close

## ATTACKING & EVADING IDS/IPS
Based on Delta between devices
Insertion Attack
IDS accepts packet
Host will not accept packet
Evasion Attacking
IDS does not accept packet
Host will accept packet

## TECHNICAL ATTACKS ON IDS/IPS
packet sequence manipulation
fragmenting payload
overlapping fragments with different reassembly by devices
Manipulating TCP headers
Manipulating IP options
Sending data during the TCP connection setup

## NON-TECHNICAL ATTACKS AGAINST IDS/IPS
attacking during periods of low manning
Example - Ramadan 2012 Saudi Aramco attack
attacking during a surge in activity
Example - Target Corp. Point of Sale machines during the Thanksgiving-Christmas 2013 shopping season

## STRENGTHENING DEFENSIVE SYSTEMS
Linking IDS/IPS to other tools
Multiconfig
Tuning
HIDS and File Integrity


