# FINGERPRINTING AND HOST IDENTIFICATION
- Variances in the RFC implementation for different OS’s and systems enables the capability for fingerprinting
- Tools used for fingerprinting and host identification can be used passively(sniffing/fingerprinting) or actively(scanning)
# P0F (PASSIVE OS FINGERPRINTING)
- Looks at variations in initial TTL, fragmentation flag, default IP header packet length, window size, and TCP options
- config stored in:  ```more /etc/p0f/p0f.fp```
```
sudo p0f -i eth0
sudo p0f -t
sudo p0f -r anaalysis-demo.pcap
sudo p0f -r anaalysis-demo.pcap -o /var/log/p0f.log
ls -l /var/log
sudo cat /var/log/p0f.log (inside see timestamps, and mod=**(ie syn, http request, synack), client and server ip's)
sudo cat /var/log/p0f.log | grep "mod=http"
```
# NETWORK TRAFFIC SNIFFING
-what makes traffic capture possible? Libpcap, WinPcaap (outdated), NPCAP
# NETWORK TRAFFIC BASELINING
- Snapshot of what the network looks like during a time frame
- No industry standard
- 7 days to establish the initial snapshot
- Prerequisite Information
# NETWORK BASELINE OBJECTIVE
- Determines the current state of your network
- Ascertain the current utilization of network resources
- Identify normal vs peak network traffic time frames
- Verify port / protocol usage
# ANALYZE NETWORK TRAFFIC STATISTICS
- protocol hierarchy
- conversations
- endpoints
- i/o graph
- ipv4 and ipv6 stats
- expert info
```
student@internet-host-student-18:/home$ sudo tcpdump -r activity_resources/pcaps/analysis-demo.pcap 'tcp[13]=0x02' 
student@internet-host-student-18:/home$ sudo tcpdump -r activity_resources/pcaps/analysis-demo.pcap 'tcp[13]=0x02' | awk '{print $3}'
student@internet-host-student-18:/home$ sudo tcpdump -r activity_resources/pcaps/analysis-demo.pcap 'tcp[13]=0x02' | awk '{print $3}'| cut -d. -f1,2,3,4
student@internet-host-student-18:/home$ sudo tcpdump -r activity_resources/pcaps/analysis-demo.pcap 'tcp[13]=0x02' | awk '{print $3}'| cut -d. -f1,2,3,4 | sort 
student@internet-host-student-18:/home$ sudo tcpdump -r activity_resources/pcaps/analysis-demo.pcap 'tcp[13]=0x02' | awk '{print $3}'| cut -d. -f1,2,3,4 | sort | uniq -c
```
# NETWORK DATA TYPES
- full packet capture data
- session data
     - sflow
     - NetFlow
- statistical data
- packet string data
- alert data
- log data
# DATA COLLECTION DEVICES
- sensors
    - in-line
    - passive
# METHODS OF DATA COLLECTION
- TAP
- SPAN
- AARP Spoofing (MitM)
# Anomoly Detection
- Indicator of Attack (IOA)
    - Proactive
    - a series of actions that are suspicious together
    - focus on intent
    - looks for what must happen
          - code execution.persistence, lateral movement, etc.
- Indiicator of Compromise (IOC)
    - reactive
    - forensic evidence
    - provides infomation that can change
          - malware, ip addresses, exploits, signatures
# INDICATORS
- .exe/executable files
- NOP sled
- repeated leters
- well known signatures
- mismatched protocols
- unusual traffic
- large amounts of traffic/ unusual times
# POTENTIAL INDICATORS OF ATTACK
- Destinations
- Ports
- Public Servers/DMZs
- Off-Hours
- Network Scans
- Alarm Events
- Malware Reinfection
- Remote logins
- High levels of email protocols
- DNS queries
# POTENTIAL INDICATORS OF COMPROMISE
- Unusual traffic outbound
- Anomalous user login or account use
- Size of responses for HTML
- High number of requests for the same files
- Using non-standard ports/ application-port mismatch
- Writing changes to the registry/system files
- DNS requests
- Unexpected/ unusual patching
- Unusual tasks
# TYPES OF MALWARE
Adware/Spyware:
- large amounts of traffic/ unusual traffic
- IOA: Destinations
- IOC: Unusual traffic outbound

Virus:
- phishing/ watering hole
- IOA: Alarm Events, Email protocols
- IOC: changes to the registry/ system files

Worm:
- phishing/ watering hole
- IOA: Alarm events
- IOC: changes to registry/ system files

Trojan:
- beaconing
- IOA: Destinations
- IOC: Unusual traffic outbound, unusual tasks, changes to registry/ system files

Rootkit:
- IOA: Malware reinfection
- IOC: Anomalous user login/ account use

Backdoor:
- IOA: Remote logins
- IOC: Anomalous user login/ account use

Botnets:
- large amounts of IPs
- IOA: Destinations, remote logins
- IOC: Unusual tasks, anomalous user login/ account use

Polymorphic and Metamorphic Malware:
- Depends on the malware type/class

Ransomware:
- IOA: Destinations, Ports, Malware reinfection
- IOC: Unusual traffic outbound, non-standard ports, unusual tasks

Mobile Code:
- IOA: Depends on the malware type/class

Information-Stealing Worms:
- phishing/ watering hole, large amounts of traffic/ unusual traffic
- IOA: Alarm events, Destinations
- IOC: changes to registry/ system files, Unusual traffic outbound

BIOS/ Firmware Malware:
- IOA: Malware reinfection
- IOC: depends on the malware type/class

# POTENTIAL METHODS OF DETECTION FOR IOAS AND IOCS
- Display Filters
- Follow Streams
- BPFs
- Color Coding
- Hex Outputs
# DECODING
- enca
- chardet
- iconv




  
