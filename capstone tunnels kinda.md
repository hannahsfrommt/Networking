
net4_comrade18@capstone-07:~$ ssh net4_student18@10.10.10.167 -p 404 -L 41804:127.0.0.1:404 -NT
student@internet-host-student-18:~$ ssh net4_comrade18@127.0.0.1 -p 41803 -L 41805:10.10.10.182:504 -NT
student@internet-host-student-18:~$ ssh net4_student18@127.0.0.1 -p 41805 -D 9050 -NT


student@internet-host-student-18:~$ ssh net4_student18@10.50.41.66 -p 7777 -L 41803:127.0.0.1:41899 -NT
student@internet-host-student-18:~$ ssh net4_comrade18@127.0.0.1 -p 41803 -D 9050 -NT


student@internet-host-student-18:~$ ssh net4_student18@10.50.41.66 -p 7777
net4_comrade18@capstone-07:~$ ssh net4_student18@10.2.2.6 -p 7777 -R 41899:127.0.0.1:2222 -NT
student@internet-host-student-18:~$ ssh net4_student18@10.50.41.66 -p 7777 -L 41802:10.2.2.7:23 -NT

