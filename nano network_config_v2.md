# Network Configuration - Week 4

## Subnetting Practice Answers
1. A /24 subnet means the first 24 bits are the network portion, leaving 8 bits for hosts. With 8 host bits, you get 256 total IP addresses.
Network address: 192.168.x.0 — this is where all the host bits are set to 0
Broadcast address: 192.168.x.255 — all host bits set to 1
Usable host range: 192.168.x.1 to 192.168.x.254
Usable hosts: 254 total (since the network and broadcast addresses can’t be assigned)
2. You get 256 total addresses in a /24 because 2⁸ = 256. Out of those, 2 are reserved — one for the network address and one for the broadcast address. That leaves 254 usable host addresses you can actually assign.
3. No — they can’t talk to each other directly. With a /24 subnet, the first three octets define the network. Your network is 192.168.x.0, and theirs is 192.168.y.0. Since the third octet is different, they’re on completely separate networks. Devices on different networks need a router to pass traffic between them. That’s why each student’s lab environment is isolated — your packets never leave your own subnet unless routing is involved.
4. No — they can’t communicate directly. The two networks are using completely different address spaces: one is 192.168.x.0/24, and the other is 10.0.x.0/24. Since they’re separate networks with separate ranges, devices on one can’t reach devices on the other by default. Your server is the exception because it’s connected to both networks. It has an interface in each address space, which is known as multi‑homing. That lets the server talk to devices on both sides. 
5. Devices decide which interface to use based on the first octet of the IP address. Anything starting with 192.168 goes out through eth0 (your lab network), and anything starting with 10 goes through eth1 (your new network). This difference is intentional — using clearly separate address ranges makes it easy to tell networks apart at a glance. It’s common for organizations to use 10.x for one purpose and 192.168.x for another.

## Server Configuration
| Device | Interface | IP Address | Purpose |
|--------|-----------|------------|---------|
| Desktop | eth0 | DHCP (127.0.0.1/8) | Guacamole access - do not configure |
| Desktop | eth1 | 192.168.1.40/24  | Lab network (client) |
| Server | eth0 | 192.168.10.10/24 | Lab network (SSH target) |
| Server | eth1 | 10.0.10.10/24 | New network (multi-homing) |

## Verification
- Server eth0 working: [Yes]
- Server eth1 configured: [Yes]
- SSH to server after changes: [Yes]
- Netplan backup created: [Yes]

## IP Convention Table Applied
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












