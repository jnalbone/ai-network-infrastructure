# DNS and DHCP Configuration - Week 5
by Jeremy Nalbone

## DNS Server Configuration
- Server IP: 192.168.10.10
- DNS Software: dnsmasq
- Domain: ai.local
- Bound interface: ens20
- Upstream DNS: 8.8.8.8

## systemd-resolved Conflict Resolution
- DNSStubListener disabled: [Yes]
- resolv.conf symlink updated: [Yes]
- IGNORE_RESOLVCONF set: [Yes]

## DNS Records Created
| Hostname | IP Address | Purpose |
|----------|------------|---------|
| server1.ai.local | 192.168.10.10 | Ubuntu Server |
| ws1.ai.local | 192.168.10.50 | Admin Workstation |

ov  1 19:13:28 dnsmasq-dhcp[85851]: 932223988 sent size:  4 option: 58 T1  12h
Nov  1 19:13:28 dnsmasq-dhcp[85851]: 932223988 sent size:  4 option: 59 T2  21h
Nov  1 19:13:28 dnsmasq-dhcp[85851]: 932223988 sent size:  4 option:  1 netmask  255.255.255.0
Nov  1 19:13:28 dnsmasq-dhcp[85851]: 932223988 sent size:  4 option: 28 broadcast  10.0.10.255
Nov  1 19:13:28 dnsmasq-dhcp[85851]: 932223988 sent size: 17 option: 12 hostname  oswdubuntutest-vm
Nov  1 19:13:28 dnsmasq-dhcp[85851]: 932223988 sent size:  8 option: 15 domain-name  ai.local
Nov  1 19:13:28 dnsmasq-dhcp[85851]: 932223988 sent size:  4 option:  6 dns-server  10.0.10.10
Nov  1 19:13:28 dnsmasq-dhcp[85851]: 932223988 sent size:  4 option:  3 router  10.0.10.10
Nov  1 19:13:28 dnsmasq[85851]: query[A] ntp.ubuntu.com from 10.0.10.242
Nov  1 19:13:28 dnsmasq[85851]: forwarded ntp.ubuntu.com to 8.8.8.8
Nov  1 19:13:28 dnsmasq[85851]: forwarded ntp.ubuntu.com to 8.8.8.8
Nov  1 19:13:28 dnsmasq[85851]: reply ntp.ubuntu.com is 91.189.91.157
Nov  1 19:13:28 dnsmasq[85851]: reply ntp.ubuntu.com is 185.125.190.58
Nov  1 19:13:28 dnsmasq[85851]: reply ntp.ubuntu.com is 185.125.190.56
Nov  1 19:13:28 dnsmasq[85851]: reply ntp.ubuntu.com is 185.125.190.57
Nov  1 19:13:28 dnsmasq[85851]: query[AAAA] ntp.ubuntu.com from 10.0.10.242
Nov  1 19:13:28 dnsmasq[85851]: forwarded ntp.ubuntu.com to 8.8.8.8
Nov  1 19:13:28 dnsmasq[85851]: reply ntp.ubuntu.com is 2620:2d:4000:1::3f
Nov  1 19:13:28 dnsmasq[85851]: reply ntp.ubuntu.com is 2620:2d:4000:1::40
Nov  1 19:13:28 dnsmasq[85851]: reply ntp.ubuntu.com is 2620:2d:4000:1::41


## DHCP Configuration
- DHCP Pool: 10.0.10.100 - 10.0.10.254
- Lease Time: 24 hours
- Gateway: 10.0.10.10
- DNS Server: 10.0.10.10
- Domain: ai.local

## Verification
- nslookup server1.ai.local (loopback): [Success]
- nslookup ws1.ai.local (loopback): [Success]
- nslookup google.com (loopback): [Success]
- DHCP lease for test VM: [Not Found]
- DORA process in logs: [Confirmed]
- Ping to test VM: [Success]