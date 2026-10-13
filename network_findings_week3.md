# Network Findings - Week 3
by Jeremy Nalbone

## Desktop Configuration
- eth0 IP: [127.0.0.1/8]
- eth1 IP: [192.168.1.40/24]
- Assigned Subnet: [192.168.10.10/24]

## Server Configuration
- Server IP: [192.168.10.10]
- SSH Connection: Verified [Yes]
- Server Interfaces: [List what you found with ip a]
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: ens18: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether bc:24:11:77:c0:a6 brd ff:ff:ff:ff:ff:ff
    altname enp0s18
    inet 192.168.1.40/24 brd 192.168.1.255 scope global ens18
       valid_lft forever preferred_lft forever
    inet6 fe80::be24:11ff:fe77:c0a6/64 scope link 
       valid_lft forever preferred_lft forever
3: ens19: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether bc:24:11:da:12:11 brd ff:ff:ff:ff:ff:ff
    altname enp0s19
    inet 192.168.10.10/24 brd 192.168.10.255 scope global ens19
       valid_lft forever preferred_lft forever
    inet6 fe80::be24:11ff:feda:1211/64 scope link 
       valid_lft forever preferred_lft forever


## Connectivity Tests
- Ping to 8.8.8.8 from server: [Success]
- Ping to desktop from server: [Success]
- Traceroute to 8.8.8.8: [30]