nano routing_week6
by Jeremy Nalbone

<!-- # Routing Between Networks - Week 6

## Session 1: Configuration and Initial Attempt

### Router VM Configuration
- Management interface (ens18): 192.168.10.2/24
- New network interface (ens19): 10.0.10.1/24
- IP forwarding enabled: [Yes/No]
- IP forwarding persistent: [Yes/No]

### DHCP Gateway Update
- Old gateway option: dhcp-option=3,10.0.10.10
- New gateway option: dhcp-option=3,10.0.10.1
- dnsmasq restarted: [Yes/No]

### Jump Box Route
- Route added: 10.0.10.0/24 via 192.168.10.2
- ip route get result: [paste output]

### Initial Ping Attempt
- Test VM IP: [from lease file]
- Ping result: [Fail]
- Expected: Fail (return path not yet fixed)

### Notes
- The Test VM's lease still points at the old gateway
- Will retry after lease renewal -->

Information shown below:

student@student-oswd-ai:~$ ssh student@192.168.1010
ssh: connect to host 192.168.3.242 port 22: No route to host
student@student-oswd-ai:~$ ssh student@192.168.10.10
student@192.168.10.10's password: 
Welcome to Ubuntu 24.04.3 LTS (GNU/Linux 6.8.0-146-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Fri Nov 13 02:34:34 PM UTC 2026

  System load:  0.0                Processes:              117
  Usage of /:   41.8% of 14.66GB   Users logged in:        1
  Memory usage: 10%                IPv4 address for ens18: 192.168.1.40
  Swap usage:   0%

 * Canonical Workshop gives developers fast, composable, reproducible, and
   secure developer environments that are perfect for agentic workflows.

   https://ubuntu.com/workshop

Expanded Security Maintenance for Applications is not enabled.

84 updates can be applied immediately.
8 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


*** System restart required ***
Last login: Sun Nov  1 19:13:54 2026 from 192.168.10.50
student@oswdubuntuserv:~$ hostname
oswdubuntuserv
student@oswdubuntuserv:~$ ip a
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
4: ens20: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether bc:24:11:ba:64:6e brd ff:ff:ff:ff:ff:ff
    altname enp0s20
    inet 10.0.10.10/24 brd 10.0.10.255 scope global ens20
       valid_lft forever preferred_lft forever
    inet6 fe80::be24:11ff:feba:646e/64 scope link 
       valid_lft forever preferred_lft forever
student@oswdubuntuserv:~$ ls /etc/netplan/
 50-cloud-init.yaml      '5-=cloud-init.yam'
 50-cloud-init.yml.back   5--cloud-init.yaml
student@oswdubuntuserv:~$ sudo /etc/netplan/ 99-ens20.yaml
[sudo] password for student: 
Sorry, try again.
[sudo] password for student: 
Sorry, try again.
[sudo] password for student: 
sudo: /etc/netplan/: command not found
student@oswdubuntuserv:~$ sudo /etc/netplan/99-ens20.yaml
sudo: /etc/netplan/99-ens20.yaml: command not found
student@oswdubuntuserv:~$ sudo nano /etc/netplan/99-ens20.yaml
student@oswdubuntuserv:~$ sudo netplan try

** (process:131253): WARNING **: 14:41:07.305: Permissions for /etc/netplan/5--cloud-init.yaml are too open. Netplan configuration should NOT be accessible by others.

** (process:131253): WARNING **: 14:41:07.305: Permissions for /etc/netplan/99-ens20.yaml are too open. Netplan configuration should NOT be accessible by others.

** (generate:131255): WARNING **: 14:41:07.309: Permissions for /etc/netplan/5--cloud-init.yaml are too open. Netplan configuration should NOT be accessible by others.

** (generate:131255): WARNING **: 14:41:07.309: Permissions for /etc/netplan/99-ens20.yaml are too open. Netplan configuration should NOT be accessible by others.

** (process:131253): WARNING **: 14:41:07.540: Permissions for /etc/netplan/5--cloud-init.yaml are too open. Netplan configuration should NOT be accessible by others.

** (process:131253): WARNING **: 14:41:07.540: Permissions for /etc/netplan/99-ens20.yaml are too open. Netplan configuration should NOT be accessible by others.

** (process:131253): WARNING **: 14:41:07.619: Permissions for /etc/netplan/5--cloud-init.yaml are too open. Netplan configuration should NOT be accessible by others.

** (process:131253): WARNING **: 14:41:07.619: Permissions for /etc/netplan/99-ens20.yaml are too open. Netplan configuration should NOT be accessible by others.
Do you want to keep these settings?


Press ENTER before the timeout to accept the new configuration


Changes will revert in 100 seconds
Configuration accepted.
student@oswdubuntuserv:~$ ip a
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
    inet 10.0.10.1/24 brd 10.0.10.255 scope global ens19
       valid_lft forever preferred_lft forever
    inet6 fe80::be24:11ff:feda:1211/64 scope link 
       valid_lft forever preferred_lft forever
4: ens20: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether bc:24:11:ba:64:6e brd ff:ff:ff:ff:ff:ff
    altname enp0s20
    inet 10.0.10.10/24 brd 10.0.10.255 scope global ens20
       valid_lft forever preferred_lft forever
    inet6 fe80::be24:11ff:feba:646e/64 scope link 
       valid_lft forever preferred_lft forever
student@oswdubuntuserv:~$ ping -c 3 10.0.10.10
PING 10.0.10.10 (10.0.10.10) 56(84) bytes of data.
64 bytes from 10.0.10.10: icmp_seq=1 ttl=64 time=0.012 ms
64 bytes from 10.0.10.10: icmp_seq=2 ttl=64 time=0.008 ms
64 bytes from 10.0.10.10: icmp_seq=3 ttl=64 time=0.014 ms

--- 10.0.10.10 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2037ms
rtt min/avg/max/mdev = 0.008/0.011/0.014/0.002 ms
student@oswdubuntuserv:~$ cat /proc/sys/net/ipv4/ip_forward
0
student@oswdubuntuserv:~$ sudo sysctl -w net.ipv4.ip_forward=1
net.ipv4.ip_forward = 1
student@oswdubuntuserv:~$ sudo nano /etc/sysctl.conf
student@oswdubuntuserv:~$ sudo sysctl -p
net.ipv4.ip_forward = 1
student@oswdubuntuserv:~$ ip route show
default via 192.168.1.1 dev ens18 proto static 
10.0.10.0/24 dev ens19 proto kernel scope link src 10.0.10.1 
10.0.10.0/24 dev ens20 proto kernel scope link src 10.0.10.10 
192.168.1.0/24 dev ens18 proto kernel scope link src 192.168.1.40 
192.168.10.0/24 dev ens19 proto kernel scope link src 192.168.10.10 
student@student-oswd-ai:~$ sudo ip route add 10.0.10.0/24 via 192.168.10.2
[sudo] password for student:            
student@student-oswd-ai:~$ ip route show
default via 192.168.1.1 dev ens18 proto static metric 100 
default via 192.168.10.1 dev ens19 proto static metric 20101 
10.0.10.0/24 via 192.168.10.2 dev ens19 
192.168.1.0/24 dev ens18 proto kernel scope link src 192.168.1.50 metric 100 
192.168.10.0/24 dev ens19 proto kernel scope link src 192.168.10.50 metric 101 
student@student-oswd-ai:~$ ip route get 10.0.10.100
10.0.10.100 via 192.168.10.2 dev ens19 src 192.168.10.50 uid 1000 
    cache 
student@student-oswd-ai:~$ cat /var/lib/misc/dnsmasq.leases
cat: /var/lib/misc/dnsmasq.leases: No such file or directory
student@student-oswd-ai:~$ ping -c 3 10.0.10.100
PING 10.0.10.100 (10.0.10.100) 56(84) bytes of data.

--- 10.0.10.100 ping statistics ---
3 packets transmitted, 0 received, 100% packet loss, time 2061ms



