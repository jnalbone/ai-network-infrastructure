1. A description of each subnet and its purpose

Subnet ID	        CIDR	        Purpose
Transit-EDGE-INT	192.168.0.0/30	Point‑to‑point link between Edge router and Internal router
DMZ	                192.168.10.0/24	Hosts the public web server and DMZ-facing router interface
Private LAN	        192.168.20.0/24	Employee workstations, printers, switch management, internal router LAN
Guest WiFi	        192.168.30.0/24	Visitor devices via DHCP, isolated from private and DMZ networks

2. A table showing device assignments (device, interface, IP, subnet)


Device	                    Interface role	        IP address	        Subnet
Edge router	                Inside / LAN	        192.168.0.1	        192.168.0.0/30
Edge router	                Outside / WAN	        Public IP (ISP)	    Internet
Internal router	            Outside (to Edge)	    192.168.0.2	        192.168.0.0/30
Internal router	            DMZ interface	        192.168.10.1	    192.168.10.0/24
Internal router	            Private LAN interface	192.168.20.1	    192.168.20.0/24
Internal router	            Guest WiFi gateway	    192.168.30.1	    192.168.30.0/24
Web server(DMZ)	            NIC	                    192.168.10.10	    192.168.10.0/24
Switch 1	                Mgmt IP	                192.168.20.2	    192.168.20.0/24
Switch 2	                Mgmt IP	                192.168.20.3	    192.168.20.0/24
Switch 3	                Mgmt IP	                192.168.20.4	    192.168.20.0/24
Employee workstations 1–25	Static IPs	            192.168.20.50–74	192.168.20.0/24
Printers 1–10	            Static IPs	            192.168.20.20–29	192.168.20.0/24
Guest WiFi clients	        DHCP	                192.168.30.100–199	192.168.30.0/24

3. Subnet details (network, mask, usable range) for each subnet

- Transit-EDGE-INT
Network: 192.168.0.0/30
Mask: 255.255.255.252
Usable host range: 192.168.0.1 – 192.168.0.2

- DMZ
Network: 192.168.10.0/24
Mask: 255.255.255.0
Usable host range: 192.168.10.1 – 192.168.10.254

- Private LAN
Network: 192.168.20.0/24
Mask: 255.255.255.0
Usable host range: 192.168.20.1 – 192.168.20.254

- Guest WiFi
Network: 192.168.30.0/24
Mask: 255.255.255.0
Usable host range: 192.168.30.1 – 192.168.30.254

4. Your justification for the design

- Subnet sizing:
/24 for DMZ, Private LAN, and Guest WiFi gives up to 254 hosts each—simple, scalable, and easy to document.
/30 for the router transit link minimizes wasted addresses while cleanly separating routing infrastructure.

- Security & DMZ concept:
The web server sits in the DMZ (192.168.10.0/24), reachable from the internet via NAT and firewall rules on the edge router (e.g., TCP 80/443 to 192.168.10.10).
The internal router separates DMZ from the Private LAN (192.168.20.0/24), blocking unsolicited traffic from DMZ to internal devices.

- Protecting internal devices:
Employee PCs, printers, and switches are only in 192.168.20.0/24; no public NAT or port‑forwarding is configured for this subnet.
Internal router and edge router ACLs allow outbound internet access from the Private LAN but deny inbound sessions from the internet—so workstations and printers are not internet-accessible.

- Guest WiFi isolation:
Guests use 192.168.30.0/24 with DHCP (192.168.30.100–199).
Firewall/VLAN rules ensure guest traffic can only go to the internet, not to 192.168.20.0/24 (employees) or 192.168.10.0/24 (DMZ).
This cleanly enforces the requirement that Guest WiFi is isolated from the private network.