pc1 -------{xll, forwarding, file transfer} ----> pc2
              [session key]
client                                           server  
client sends user key and server sends host key back to client
![image](https://github.com/hannahsfrommt/Networking/assets/140441321/4987043f-ed53-4770-9bbd-393452b6b633)



IH> ssh <user>@<ip> -L <bind_port>:<target_ip>:<target_port> -NT
---   ____________  --------------  _______________________
IH> ssh <user>@<ip> -R <bind_port>:<target_ip>:<target_port> -NT
IH> ssh <user>@<ip> -D 9050 -NT
--                     ----
-NT reduces problemems and means not interactive
-D default port is 9050

IH ------------------ Bob ---------------------------------James
                1.2.3.4   10.6.0.1/29                 10.6.0.5    172.16.10.2/29
        port 1111,80, 443                              21, 23, 22
        
files: 127.0.0.1/42200/ and 10.6.0.5/

IH> sudo nmap 1.2.3.4 (find ports open)
IH> sudo nmap 1.2.3.4 -A (see banners; 1111 is ssh)
IH> wget -r 1.2.3.4
IH> ssh user1@1.2.3.4 -p 1111
User1@Bob> "situational awareness"
User1@Bob> ip r, ip a, ip n
User1@Bob> ping network
need to create a tunnel to use nmap through bob
IH> user1@1.2.3.4 -p 1111 -D 9050 -NT
using -D because we are gonna scan a bunch of ports using nmap
IH>  proxychains nmap 10.6.0.5 -Pn -A -vvvv -T4
doing -Pn to skip host discovery to treat host like its up bc its only tcp
see ftp open 
IH> proxychains wget -r FTP://10.6.0.5
most dangerous part of module:
IH> proxychains telnet 10.6.0.5
user1@james>  (situational awareness ... find ssh is open on port 22)

{ 
intermission:
get traffic from Bobs 80 , obviously not secure so to make secure use ssh
IH> ssh user1@1.2.3.4 -p 1111 -L 42200:127.0.0.180 -NT
IH> wget -r 127.0.0.1:42200
}
user1@james> ssh user1@10.6.0.1 -p 1111 -R 42299:127.0.0.1:22 -NT
(how does James refer to its own port 22 : use loopback)
test to see if it works
Bob> nc 127.0.0.1 42299
>>> openssh
create a local port forward to bob to the port that lets us connect to james
ssh user1@1.2.3.4 -p 1111 -L 42200:127.0.0.1:42299 -NT
test again
IH> ssh user1@127.0.0.1 -p 42200
user1@James>

need to close the first/dynamic tunnel xxxx closes proxychains telnet so everything just closed xxxxx
forget proxychains telnet doesnt exist!!!

IH ------------------ Bob ---------------------------------James
                1.2.3.4   10.6.0.1/29                 10.6.0.5    172.16.10.2/29
        port 1111,80, 443                              21, 23, 22
        
 :42200 -> James:23   :42299 -> James:22    
 :9050 -> James:->*
files: 127.0.0.1/42200/ and 10.6.0.5/

authenticating to bob, bob talks to james through telnet
IH> ssh user1@1.2.3.4 -p 1111 -L 42200:10.6.0.5:23 -NT
test it
IH> telnet 127.0.0.1 42200
user1@james> ssh user1@10.6.0.1 -p 1111 -R 42299:127.0.0.1:42200 -NT
test it
Bob> ssh user1@127.0.0.1 -p 42299
James> 
yay it works
IH> ssh user1@1.2.3.4 -p 1111 -L 42201:127.0.0.1:42299 -NT
IH> ssh user1@127.0.0.1 -p 42201 
user1@James> 
back hwhere we started
IH> ssh user@127.0.0.1 -p 42201 -D 9050 -NT
IH> proxychains nmap 172.16.10.2/29 -Pn -A -T4 -vvvv
or 
IH> proxychains ./scan.sh (172.16.10; .1, .6; 21-23,80)

IH -------------------------------- Bob ------------------------------------------------James ---------------------------------------------- curtis                     inigo
                             1.2.3.4   10.6.0.1/29                              10.6.0.5    172.16.10.2/29                                  172.16.10.4, 172.16.10.5, 172.16.10.6
                            port 1111,80, 443                                      21, 23, 22                                                22,80        21, 80        21,23,80
        
 :42200 -> James:23           :42299 -> James:22    
 :9050 -> James:->*                                                              :42298 -> Inigo:80
 :42201 -> Bob:42299
 :42202 -> Curtis:22
 :42203 -> Inigo:23
 :42204 -> James:42298
files: 127.0.0.1/42200/ and 10.6.0.5/ and 172.16.10.4/ and 172.16.10.5/ and 172.16.10.6/ and 127.0.0.1:42204/

IH> proxychains wget -r 172.16.10.4 (do for .5 as well)
IH> proxychains wget -r FTP://172.16.10.5 (and .6)

build static tunnels just in case to leverage ports
IH> ssh user1@127.0.0.1 -p 42201 -L 42202:172.16.10.4:22 -L 42203:172.16.10.6:23 -NT
IH> nc 127.0.0.1 42202
>>> open ssh
IH> nc 127.0.0.1 42203
>>> ftp
IH> ssh user1@127.0.0.1 -p 42202
user1@Curtis

----------curtis--------------
172.16.0.4     10.4.0.16/27

change -D tunnel port
IH> ssh user@127.0.0.1 -p 42201 -D 9050 -NT
IH> telnet 127.0.0.1 42203
user1@Inigo>

----------inigo -----------
      port 80
user1@Inigo> ssh user1@172.16.10.2 -R 42298:127.0.0.1:80 -NT
James> nc 127.0.0.1 42298 (enter or type)
nginx (web server)
IH> ssh user@127.0.0.1 -p 42201 -L 42204:127.0.0.1:42298
IH> wget -r 127.0.0.1 42204
IH> proxychains nmap -Pn 10.4.0.16/27 -T4 -A -vvvv (can do this because of the dynamic tunnel)
keep going if hosts are found...

