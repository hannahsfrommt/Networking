
IH> telnet 10.50.40.12
net4_student18@data-collection-net-ssh-01:~$ ssh student@10.50.43.237 -R 41880:127.0.0.1:80 -NT 
IH> wget -r 127.0.0.1:41880
IH> eog <filename>




^Cnet4_student18@data-collection-net-ssh-01:~$ ssh student@10.50.43.237 -R 41880:127.0.0.1:22 -NT
student@internet-host-student-18:~$ ssh net4_student18@127.0.0.1 -p 41880 -D 9050 -NT
use proxychains to scan for 192.168.0.0/24 network

proxychains nmap 192.168.0.20 --script=banner
see ssh is at port 3333



student@internet-host-student-18:~$ ssh net4_student18@127.0.0.1 -p 41880 -L 41810:192.168.0.20:3333 -NT
student@internet-host-student-18:~$ ssh net4_student18@127.0.0.1 -p 41810 -D 9050 -NT
student@internet-host-student-18:~$ proxychains ssh net4_student18@192.168.0.20 -p 3333
see .50



^Cstudent@internet-host-student-18:~$ ssh net4_student18@127.0.0.1 -p 41880 -L 41811:192.168.0.40:5555 -NT
student@internet-host-student-18:~$ ssh net4_student18@127.0.0.1 -p 41811 -D 9050 -NT



student@internet-host-student-18:~$ ssh net4_student18@127.0.0.1 -p 41811 -L 41813:172.16.0.60:23 -NT
student@internet-host-student-18:~$ telnet 127.0.0.1 41813

net4_comrade18@data-collection-net-ssh-06:~$ ssh net4_student18@192.168.0.40 -p 5555 -R 41881:127.0.0.1:22 -NT
student@internet-host-student-18:~$ ssh net4_student18@127.0.0.1 -p 41811 -L 41814:127.0.0.1:41881 -NT
student@internet-host-student-18:~$ ssh net4_comrade18@127.0.0.1 -p 41814 -D 9050 -NT
student@internet-host-student-18:~$ ssh net4_comrade18@127.0.0.1 -p 41814
student@internet-host-student-18:~$ proxychains nmap 172.16.0.70 -p 1337 --script=banner




^Cnet4_student18@data-collection-net-ssh-01:~$ ssh student@10.50.43.237 -R 41880:127.0.0.1:22 -NT
^Cstudent@internet-host-student-18:~$ ssh net4_student18@127.0.0.1 -p 41880 -L 41811:192.168.0.40:5555 -NT
^Cstudent@internet-host-student-18:~$ ssh net4_comrade18@127.0.0.1 -p 41815 -D 9050 -NT
student@internet-host-student-18:~$ ssh net4_student18@127.0.0.1 -p 41811 -L 41813:172.16.0.60:23 -NT
net4_comrade18@data-collection-net-ssh-06:~$ ssh net4_student18@192.168.0.40 -p 5555 -R 41881:127.0.0.1:22 -NT
student@internet-host-student-18:~$ ssh net4_comrade18@127.0.0.1 -p 41814 -L 41815:172.16.0.90:2222 -NT
proxychains telnet....












