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

