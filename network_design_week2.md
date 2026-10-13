Module 2 Lab
By Jeremy Nalbone

SCENARIO: [Small office with Guest Access]

REQUIREMENTS ANALYSIS:
- Connectivity needs:
  - The 5 staff workstations need access to the practice management server.
  - Dentists, dental assistants, and the office manager need access to shared business resources.
  - The practice management server needs network connectivity to the staff computers.
  - Patients need internet access through WiFi, but must not be able to access any business systems.
  - All users share one internet connection.
- Security requirements:
  - The practice management server contains Protected Health Information (PHI) and must be strongly protected.
  - Patient WiFi must be completely isolated from the staff/business network.
  - The firewall should block unauthorized traffic between the patient and business networks.
  - Only authorized staff devices should be able to access the practice management server.
  - Wireless access should use strong authentication and encryption.
- Performance considerations:
  - The practice has relatively low bandwidth requirements.
  - Practice management applications and patient records generally do not require extremely high bandwidth.
  - Approximately 10 patients may use WiFi simultaneously, so the wireless network should provide adequate internet bandwidth without affecting staff operations.
  - Network latency should be low and reliable for staff accessing the practice management server.
- Scalability factors:
  - The design should allow additional staff computers, printers, phones, or medical/dental devices to be added later.
  - A managed switch with additional ports provides room for expansion.
  - Additional wireless access points could be added if the practice grows or coverage becomes an issue.
- Compliance constraints:
  - Because the practice stores patient health information, the network should support HIPAA security requirements.
  - PHI should be protected from unauthorized access.
  - Network segmentation, firewall controls, strong authentication, and access controls should be used to reduce the risk of unauthorized access.

  PROPOSED TOPOLOGY
  Network segments:
1. Staff/Business VLAN
   - 5 staff workstations
   - Practice management server
   - Business printers and other trusted devices
   - Used for all normal business operations.
2. Patient WiFi VLAN
   - Patient smartphones, tablets, and laptops.
   - Internet access only.
   - No access to the Staff/Business VLAN or practice management server.
Components needed:
- 1 business-class router/firewall capable of VLANs and firewall rules
- 1 managed Ethernet switch
- 1 wireless access point supporting multiple SSIDs/VLANs
- 1 practice management server
- Existing Ethernet/cable runs to treatment rooms
- Staff computers
Traffic flow: (I got the drawing help below from AI due to not adding it in to the md and make it appealing, I asked AI to draw it up for me as I described the topology to it)
                    INTERNET
                       |
                 ISP Connection
                       |
                Router / Firewall
                  /           \
                 /             \
        Staff VLAN            Patient VLAN
             |                     |
      Managed Switch          WiFi Access Point
        /        \                  |
       /          \            Patient Devices
 Staff PCs      Server          Internet Only

- Staff computers connect to the managed switch.
- The practice management server connects to the managed switch.
- Staff traffic remains on the Staff VLAN.
- Patient devices connect to the separate Patient WiFi VLAN.
- The firewall allows the Patient VLAN to reach the internet but blocks it from reaching the Staff VLAN.
- Staff devices are allowed to reach the practice management server.
- The firewall acts as the central security boundary between the networks.

DESIGN JUSTIFICATION
Why this topology meets the requirements:
- A two-VLAN design provides the minimum viable network architecture while still addressing the practice's major security requirements.
- The Staff VLAN allows the five workstations to communicate with the practice management server. The Patient WiFi VLAN provides patients with internet access without giving them access to the internal business network.
- A managed switch and VLAN-capable firewall avoid the need for multiple physical networks and keep the design relatively simple for a small practice with no dedicated IT staff.
Security benefits of this design:
- The most important security benefit is network segmentation.
- If a patient's device becomes infected or a patient attempts to access internal resources, firewall rules prevent that device from communicating with the Staff VLAN.
- The practice management server can also be restricted so that only devices on the Staff VLAN can access it.
Additional protections should include:
- Strong firewall rules
- WPA2/WPA3 encryption for WiFi
- Separate SSIDs for staff and patients
- Strong administrative passwords
- Regular software and firmware updates
- Restricted access to the server
- Regular backups of patient data
This design provides a much stronger security boundary than placing staff and patients on the same network.
Performance benefits of this design:
- The network is small, so a single managed switch and wireless access point should provide sufficient performance.
- Separating patient traffic also helps prevent unnecessary patient-device traffic from directly interacting with business devices.
- The firewall can prioritize business traffic if needed, ensuring that staff access to the practice management system remains reliable even when several patients are using the WiFi.

ALTERNATIVES CONSIDERED:
Alternative 1: One flat network
All computers, the server, and patient WiFi would use the same network.
Rejected because:
- Patients could potentially reach business devices.
- Malware on a patient device could spread to business systems.
- The practice management server containing PHI would have less protection.
- It does not provide adequate network segmentation.
Alternative 2: Two completely separate physical networks
The practice could install separate switches, routers, and internet connections for staff and patients.
Rejected because:
- More expensive.
- More equipment to maintain.
- A second internet connection is unnecessary.
- Overkill for an 8-person dental practice.
And alternative 3: VLAN-based network
Use one managed switch and one firewall/router with separate VLANs for staff and patients.
Selected because:
- Provides strong network isolation.
- Uses the existing cable infrastructure.
- Requires relatively little equipment.
- Keeps costs low.
- Is easier to maintain than two completely separate physical networks.
- Provides room for future expansion.

COMPONENT SUMMARY:
Managed Switch - 1	Central network location; connects staff PCs, server, and AP
Router/Firewall - 1	Internet edge; provides routing, VLAN isolation, and firewall rules
Wireless Access Point - 1	Waiting room/central location; provides Staff and Patient WiFi
Practice Management Server - 1	Secure business network; stores practice data and PHI
Staff Workstations - 5	Reception and treatment stations; connected to Staff VLAN
Internet Connection - 1	Shared connection through the firewall


SCENARIO: [Westfield Engineering]

REQUIREMENTS ANALYSIS:
Connectivity needs: (Who needs to reach what?)
- Engineering Department
  - 25 engineers need access to their workstations and the Engineering file server.
  - Engineering users need internet access.
  - Engineering users should not access Finance or HR systems.
- Finance Department
  - 10 Finance employees need access to their workstations and the Finance database server.
  - Finance users need internet access.
  - Finance users should not access Engineering or HR systems.
- HR Department
  - 5 HR employees need access to their workstations and the HR server.
  - HR users need internet access.
  - HR users should not access Engineering or Finance systems.
- Executive Team
  - 5 executives need internet access.
  - Executives need access to all three department servers.
  - Executives should be able to access Engineering, Finance, and HR resources regardless of department.
Security requirements:
- Each department needs to be placed on a separate network segment.
- Engineering traffic must be isolated from Finance and HR.
- Finance traffic must be isolated from Engineering and HR.
- HR traffic must be isolated from Engineering and Finance.
- Department servers should only accept connections from:
  - Their own department
  - The Executive network
- The firewall/router must enforce these access restrictions.
- The network should prevent users from bypassing the segmentation.
- Financial, HR, and engineering project data should be protected from unauthorized departmental access.
Performance considerations:
- Engineering has the largest number of users, with 25 workstations, and will likely generate the most file-server traffic.
- The Engineering file server may require higher throughput because engineers could frequently transfer project files.
- Finance and HR have fewer users, so their network requirements will be lower.
- A gigabit or faster switched network should provide adequate performance.
- The firewall/router should have sufficient throughput to handle inter-VLAN routing without becoming a bottleneck.
- All departments share one internet connection, so the firewall should be capable of handling the organization's expected internet bandwidth.
Scalability factors:
- The company currently has 45 employees but may add employees in the future.
- The network should have additional switch ports available for expansion.
- VLANs make it easy to add additional computers to an existing department without redesigning the physical network.
- Additional access points, printers, VoIP phones, or servers can be added later.
- The firewall should support additional VLANs if the company adds departments or specialized networks.
Compliance constraints:
- Finance contains sensitive financial information.
- HR contains confidential employee records.
- Engineering contains potentially sensitive client and project information.
- Access should follow the principle of least privilege, meaning employees should only be able to access systems necessary for their jobs.
- If the company handles regulated financial, employee, or client information, additional industry-specific requirements may apply.

PROPOSED TOPOLOGY:
Network segments:
I would create four primary VLANs:
1. Engineering VLAN
- 25 engineering workstations
- Engineering file server
- Engineering printers/devices
- Internet access
- Access to Finance and HR VLANs: Denied
2. Finance VLAN
- 10 Finance workstations
- Finance database server
- Finance printers/devices
- Internet access
- Access to Engineering and HR VLANs: Denied
3. HR VLAN
- 5 HR workstations
- HR employee-record server
- HR printers/devices
- Internet access
- Access to Engineering and Finance VLANs: Denied
4. Executive VLAN
- 5 executive workstations
- Internet access
- Access to Engineering server
- Access to Finance server
- Access to HR server
The Executive VLAN would not necessarily need unrestricted access to every workstation. The firewall can specifically permit executive computers to reach the required servers while blocking unnecessary access to individual department workstations.
Components needed:
- 1 business-class firewall/router capable of VLANs and inter-VLAN firewall rules
- 2 managed switches, depending on port requirements and physical layout
- 1 Engineering file server
- 1 Finance database server
- 1 HR server
- 45 employee workstations
- Existing Ethernet cabling
- Internet connection
- Optional UPS for network equipment and servers
Using two switches provides enough ports for the 45 employees, servers, network infrastructure, and future expansion.
Traffic flow: (I got the drawing help below from AI due to not adding it in to the md and make it appealing, I asked AI to draw it up for me as I described the topology to it)
                         INTERNET
                            |
                     ┌──────▼──────┐
                     │   FIREWALL  │
                     │   / ROUTER  │
                     └──────┬──────┘
                            │
                     ┌──────▼──────┐
                     │ CORE/MAIN   │
                     │   SWITCH    │
                     └──────┬──────┘
                            │
          ┌─────────────────┼─────────────────┐
          │                 │                 │
    ┌─────▼─────┐     ┌─────▼─────┐     ┌─────▼─────┐
    │ ENGINEERING│     │  FINANCE  │     │    HR     │
    │   VLAN 10  │     │  VLAN 20  │     │  VLAN 30  │
    │            │     │           │     │           │
    │ 25 PCs     │     │ 10 PCs    │     │ 5 PCs     │
    │ File Server│     │ DB Server │     │ HR Server │
    └────────────┘     └───────────┘     └───────────┘
          ▲                 ▲                 ▲
          │                 │                 │
          └─────────────────┼─────────────────┘
                            │
                     ┌──────▼──────┐
                     │ EXECUTIVE   │
                     │   VLAN 40   │
                     │             │
                     │ 5 PCs       │
                     └─────────────┘

DESIGN JUSTIFICATION:
Why this topology meets the requirements:
The VLAN-based topology directly addresses Westfield Engineering's requirement for department-level isolation.
Each department receives its own VLAN, which creates separate logical networks even though the company is using the same physical Ethernet infrastructure.
The firewall controls traffic between those VLANs.
SECURITY BENEFITS:
The biggest benefit is network segmentation.
Without segmentation, an employee's computer could potentially communicate with servers belonging to another department.
With this design:
- Engineering cannot access Finance systems.
- Engineering cannot access HR systems.
- Finance cannot access Engineering systems.
- Finance cannot access HR systems.
- HR cannot access Engineering systems.
- HR cannot access Finance systems.
- Executives can access the necessary department servers.
This significantly reduces the possibility of unauthorized access or lateral movement if one employee workstation becomes compromised.
The design also follows the principle of least privilege. Users receive access based on their role rather than being placed on one large network where everything is potentially reachable.
Performance benefits of this design:
The use of managed switches provides high-speed local communication within each department.
This is particularly important for Engineering because the department has 25 employees and a file server that could handle large project files.
Most Engineering file traffic can remain within the Engineering VLAN instead of unnecessarily crossing the firewall.
The firewall only needs to process traffic that crosses between VLANs or goes to the internet.
The design also provides enough switch capacity to allow future expansion without replacing the entire network.

ALTERNATIVES CONSIDERED:
Alternative 1: One flat network
All 45 employees and all servers would be placed on one network.
Rejected because:
- Engineering could potentially access Finance systems.
- Finance could potentially access HR systems.
- HR could potentially access Engineering systems.
- A compromised workstation could potentially move laterally throughout the network.
- It does not meet the department isolation requirements.
This would be the simplest design, but it would provide insufficient security.
Alternative 2: Separate physical networks for every department
Each department could have its own physical switch and router/network.
Rejected because:
- More expensive.
- More hardware to maintain.
- More complicated physical infrastructure.
- Doesn't provide a significant advantage over VLANs for this environment.
- The existing Ethernet cabling can support a VLAN-based solution.
COMPONENTS SUMMARY:
Managed Switches - 2Central network area; connect employee workstations and servers
Router/Firewall - 1	Internet gateway and inter-VLAN security
Engineering Server - 1	Engineering VLAN; stores project files
Finance Server - 1	Finance VLAN; stores financial records
HR Server - 1	HR VLAN; stores employee records
Engineering Workstations - 25	Engineering VLAN
Finance Workstations - 10	Finance VLAN
HR Workstations - 5	HR VLAN
Executive Workstations - 5	Executive VLAN
Internet Connection - 1	Shared by all VLANs through firewall
UPS	1+	Protects firewall, switches, and servers from power interruptions

SCENARIO: [Greenfield E-Commerce Startup]

REQUIREMENTS ANALYSIS:
Connectivity needs: (Who needs to reach what?)
- Internet customers
  - Need to access the public web application.
  - Should only be able to reach the web servers through HTTP/HTTPS.
  - Should not be able to directly reach application or database servers.
- Web servers
  - 2 web servers are required for redundancy.
  - They receive customer HTTP/HTTPS traffic.
  - They need to communicate with the application servers.
  - They should not have direct access to the database server.
- Application servers
  - 2 application servers are required for redundancy.
  - They process orders and business logic.
  - They receive requests from the web servers.
  - They need controlled access to the database server.
  - They should not be directly reachable from the public internet.
- Database server
  - Stores customer information, orders, and inventory.
  - Should only accept connections from authorized application servers.
  - Should not be reachable directly from the internet.
  - Should not accept direct connections from the web tier.
- Operations team
  - 3 administrators need to manage all servers.
  - Administrative access must go through a secure jump box/bastion host.
  - Administrators should not connect directly from their workstations to production servers.
Security requirements:
Security is the highest priority because the company stores customer and potentially payment-related information.
- Public internet traffic should terminate in the web/DMZ tier.
- Application servers must be isolated from the public internet.
- The database must be isolated from both the internet and web servers.
- Administrative access must use the jump box.
- Firewall rules should follow a default-deny approach.
- Only required ports and traffic should be permitted between network segments.
- Administrative connections should use secure protocols such as SSH or RDP over an encrypted management connection.
- Strong authentication, preferably MFA, should be used for administrative access.
- Production servers should be hardened and regularly patched.
- Logging and monitoring should be enabled.
- Database access should be limited to the application servers.
- Sensitive customer information should be encrypted both in transit and at rest.
- Backups should be protected and separated from the production environment.
Performance considerations:
- The web tier must handle potentially large numbers of simultaneous customer connections.
- Two web servers provide both redundancy and the ability to distribute traffic.
- Two application servers provide redundancy and prevent one failed application server from taking down the store.
- A load balancer should distribute incoming traffic between the two web servers.
- The application tier should distribute workload between the two application servers.
- The database server is a central component and should have sufficient CPU, memory, storage, and network performance to support both application servers.
- Low latency between the application and database tiers is important because order processing requires frequent database communication.
Scalability factors:
Greenfield is a startup, so the infrastructure should allow the company to grow.
The design should allow:
- Additional web servers to be added behind the load balancer.
- Additional application servers to be added.
- Increased database resources as transaction volume grows.
- Additional monitoring and security infrastructure.
- Expansion to a database cluster or replication solution later.
- Additional administrative users with controlled access.
The three-tier architecture makes scaling easier because each tier can be expanded independently.
Compliance constraints:
Greenfield must comply with PCI-DSS because it operates an e-commerce platform that handles payment card-related activity.
The design should therefore support:
- Strong network segmentation
- Restricted access to systems handling sensitive information
- Firewall controls
- Secure administrative access
- Encryption
- Authentication and access controls
- Logging and monitoring
- Vulnerability management
- Regular security testing
- Secure configuration and patching
- Protection of cardholder data
The exact PCI-DSS scope will depend on how Greenfield processes payments. If a third-party payment processor handles card data and Greenfield never stores or processes cardholder data directly, the scope may be reduced, but the architecture should still follow strong security practices.
PROPOSED TOPOLOGY:
Network segments:
I would use four primary security zones:
1. Public Web / DMZ
Contains:
- Web Server 1
- Web Server 2
- Public-facing load balancer
Purpose:
- Receives customer internet traffic.
- Allows HTTP/HTTPS traffic from the internet.
- Prevents customers from directly reaching internal application or database systems.
2. Application Network
Contains:
- Application Server 1
- Application Server 2
Purpose:
- Processes orders.
- Handles business logic.
- Receives requests from the web tier.
- Communicates with the database on approved ports.
The application servers have no direct inbound internet access.
3. Database Network
Contains:
- Database Server
Purpose:
- Stores customer information.
- Stores orders.
- Stores inventory.
- Accepts database connections only from authorized application servers.
This is the most restricted production network.
4. Management Network
Contains:
- Secure jump box/bastion host
- Administrative access
Purpose:
- Provides the only administrative entry point into the production server environment.
- Operations administrators connect to the jump box first.
- From the jump box, administrators can connect to the web, application, and database servers according to their permissions.
Components needed:
- 1 perimeter firewall
- 1 internal segmentation firewall or equivalent firewall functionality
- 1 public-facing load balancer
- 2 web servers
- 2 application servers
- 1 database server
- 1 secure jump box/bastion host
- Managed switches/network infrastructure
- Monitoring/logging system
- Backup infrastructure
- Secure administrative workstations
TRAFFIC FLOW: (I got the drawing help below from AI due to not adding it in to the md and make it appealing, I asked AI to draw it up for me as I described the topology to it)
                         INTERNET
                            |
                            |
                     HTTP / HTTPS ONLY
                            |
                    ┌───────▼────────┐
                    │   PERIMETER    │
                    │    FIREWALL    │
                    └───────┬────────┘
                            |
                            ▼
                  ┌───────────────────┐
                  │   LOAD BALANCER   │
                  └─────────┬─────────┘
                            |
                  ┌─────────┴─────────┐
                  │                   │
            ┌─────▼─────┐       ┌─────▼─────┐
            │ WEB SERVER│       │ WEB SERVER│
            │     1     │       │     2     │
            └─────┬─────┘       └─────┬─────┘
                  │                   │
                  └─────────┬─────────┘
                            │
                    APPROVED APP TRAFFIC
                            |
                    ┌───────▼────────┐
                    │  APPLICATION   │
                    │     TIER       │
                    └───────┬────────┘
                            |
                  ┌─────────┴─────────┐
                  │                   │
            ┌─────▼─────┐       ┌─────▼─────┐
            │    APP    │       │    APP    │
            │ SERVER 1  │       │ SERVER 2  │
            └─────┬─────┘       └─────┬─────┘
                  │                   │
                  └─────────┬─────────┘
                            │
                     DATABASE TRAFFIC
                      ONLY FROM APPS
                            |
                    ┌───────▼────────┐
                    │    DATABASE    │
                    │     SERVER     │
                    └────────────────┘


                    ADMINISTRATORS
                         |
                         | Secure connection
                         ▼
                  ┌─────────────────┐
                  │    JUMP BOX     │
                  │    / BASTION    │
                  └────────┬────────┘
                           |
              ┌────────────┼─────────────┐
              ▼            ▼             ▼
            WEB          APP          DATABASE
          SERVERS       SERVERS         SERVER

DESIGN JUSTIFICATION:
Why this topology meets the requirements
This design follows a standard three-tier architecture:
Web → Application → Database
Each tier is placed into a separate security zone.
The web tier is the only tier exposed to the public internet. Customers can access the website, but their traffic cannot directly reach the application or database networks.
The application tier sits behind the web tier. It processes customer requests and communicates with the database.
The database is placed into the most restricted network and only accepts connections from the application servers.
Administrative access follows a separate path:
Administrator → Jump Box → Production Server
This prevents administrators from directly exposing management interfaces to the internet.
SECURITY BENEFITS:
The architecture provides several important security layers.
Internet isolation, Application isolation,database isolation, administrative security, and reduced attack surface.
PERFORMANCE BENEFITS:
Providing redundancy. If one web server fails, the load balancer can send customers to the remaining web server.
If one application server fails, traffic can be directed to the remaining application server.
This improves availability without requiring the database to be duplicated immediately.
The database remains on a dedicated network with high-speed connectivity to the application servers, minimizing unnecessary network traffic and latency.
ALTERNATIVES CONSIDERED:
Alternative 1: Put everything on one network
Rejected because:
- Extremely large attack surface.
- Internet-facing servers would be on the same network as the database.
- A compromised web server could potentially communicate directly with the database.
All servers would share the same network.
Alternative 2: Two-tier architecture
Rejected because:
- Provides less isolation.
- A compromised web server could have greater access to application resources.
The company could combine the web and application servers into one network and keep only the database separate.
COMPONENT SUMMARY:
Perimeter Firewall - 1	Internet edge; controls incoming/outgoing traffic
Load Balancer - 1	Distributes customer traffic between web servers
Web Servers - 2	Public-facing web tier/DMZ
Application Servers - 2	Internal application/business logic tier
Database Server - 1	Restricted database network
Jump Box/Bastion Host - 1	Secure administrative access to production
Managed Network Switches - As needed	Connect servers and network infrastructure
Monitoring/Logging System - 1	Security and operational monitoring
Backup System - 1	Protects application/database data
Internet Connection - 1	Public customer connectivity

SCENARIO: [Regional Hospital AI Initiative]

REQUIREMENTS ANALYSIS:
Connectivity needs: (Who needs to reach what?)
- Radiology Department
  - 12 radiologist workstations need access to the AI inference system.
  - Radiologists need to submit X-rays and CT scans for AI analysis.
  - Radiologists need to receive and review AI-generated results.
  - Radiologists should not have unrestricted access to the AI server or other infrastructure.
- AI Inference Server
  - The GPU server needs to receive analysis requests from authorized radiologist workstations.
  - It needs to retrieve medical images from the PACS server.
  - It needs to send AI analysis results to the integration server.
  - It should not be directly accessible from general hospital networks.
  - The GPU server needs high-speed, low-latency connectivity because it is an expensive resource that should remain highly utilized.
- PACS Server
  - Stores medical images such as X-rays and CT scans.
  - Must be accessible by the AI server for authorized image retrieval.
  - Should not be broadly accessible from the AI infrastructure network.
  - Patient data must remain inside the hospital's on-premises infrastructure.
- Integration Server
  - Receives AI results from the AI server.
  - Sends appropriate results to the hospital's EHR network.
  - Acts as the controlled communication point between the AI infrastructure and the separate hospital EHR network.
- IT Administrators
  - The three-person IT team needs to securely manage the AI infrastructure.
  - Administrative access should be restricted and authenticated.
  - Remote administration should use a secure VPN and/or jump box.
  - Administrators should be able to manage the AI server, PACS integration, and integration server without exposing management interfaces directly to users.
Security requirements:
Security is extremely important because this environment handles Protected Health Information (PHI).
- Patient data must remain on-premises.
- The AI infrastructure should be segmented from the general hospital network.
- The AI server should only accept authorized connections.
- PACS should only be accessible by approved systems and users.
- The integration server should only communicate with the required EHR services.
- Administrative interfaces should not be exposed directly to general hospital users or the internet.
- Remote IT access should use secure authentication and encryption.
- MFA should be used for administrative access where supported.
- All important access and administrative actions should be logged.
- Network traffic containing PHI should be encrypted where appropriate.
- Firewalls should use a default-deny approach between security zones.
- The proprietary AI models should be protected against unauthorized modification, copying, or access.
- Systems should be patched, monitored, and regularly reviewed.
Performance considerations:
Performance is particularly important because the AI inference server uses an expensive GPU.
- Medical imaging files can be large, particularly CT studies.
- The AI server requires high-bandwidth connectivity to the PACS server.
- Network latency between PACS and the AI server should be minimized.
- A high-speed connection such as 10 GbE between the AI server and core/storage infrastructure would be appropriate if supported by the hospital's existing infrastructure.
- Radiologist workstations do not necessarily need the same bandwidth as the AI server.
- The network should prevent unrelated hospital traffic from consuming resources needed for AI image processing.
- Quality-of-service policies could prioritize AI/PACS traffic if the existing hospital network becomes congested.
Scalability factors:
The hospital may expand the AI initiative beyond the initial deployment.
The architecture should allow:
- Additional GPU inference servers.
- Additional AI models.
- Additional radiology workstations.
- Additional imaging modalities.
- Additional integration services.
- Additional PACS capacity.
- Potential future AI applications in other departments.
The AI VLAN/subnet should have enough address space and switch capacity to add additional infrastructure without redesigning the entire network.
Compliance constraints:
HIPAA compliance is mandatory.
The design should support:
- Access controls
- User authentication
- Authorization
- Audit logging
- Encryption
- Network segmentation
- Protection of PHI
- Secure remote access
- Monitoring
- Incident detection
- Data backup and recovery
Because this system processes medical images and AI-generated medical information, access to patient data should be limited to authorized personnel and systems.
PROPOSED TOPOLOGY:
Network segments:
I would use four primary security zones for the AI initiative.
1. Radiology VLAN
Contains:
- 12 radiologist workstations
Purpose:
- Provides radiologists with controlled access to the AI system.
- Allows radiologists to submit imaging studies for analysis.
- Receives AI results.
Radiology users should not receive unrestricted access to the underlying AI server.
2. AI Infrastructure VLAN
Contains:
- AI GPU inference server
Purpose:
- Runs the medical imaging AI models.
- Receives authorized requests from Radiology.
- Retrieves images from PACS.
- Sends results to the integration server.
This should be a highly restricted network segment because it contains proprietary AI models.
3. Integration VLAN
Contains:
- Integration server
Purpose:
- Acts as the controlled bridge between the AI environment and the hospital EHR network.
- Receives AI results.
- Sends approved information to the EHR system.
- Prevents the AI server from having unrestricted access to the hospital's EHR network.
4. Management VLAN
Contains:
- Secure management/jump server
- Administrative management interfaces
Purpose:
- Provides the IT team with controlled administrative access.
- Keeps management traffic separate from normal clinical traffic.
- Allows centralized logging and access control.
Existing PACS/EHR networks
The hospital already has these networks, so they should remain separate.
The AI environment should communicate with them only through specific firewall rules and approved services.
COMPONENTS NEEDED:
- 1 or more enterprise firewalls capable of VLAN/zone-based segmentation
- Managed high-speed switches
- 1 high-performance GPU AI inference server
- 1 PACS server — existing hospital infrastructure
- 1 integration server
- 1 secure management/jump server
- Existing EHR infrastructure
- 12 radiologist workstations — existing
- VPN solution for secure remote administration
- Centralized logging/audit system
- UPS/power protection
- Backup/recovery infrastructure
- High-speed network connection between AI server and PACS/core infrastructure
TRAFFIC FLOW: (I got the drawing help below from AI due to not adding it in to the md and make it appealing, I asked AI to draw it up for me as I described the topology to it)
                         HOSPITAL NETWORK
                               |
                               |
                         ┌─────▼─────┐
                         │ FIREWALL  │
                         │ / ROUTER  │
                         └─────┬─────┘
                               |
              ┌────────────────┼────────────────┐
              │                │                │
              ▼                ▼                ▼
       ┌────────────┐   ┌────────────┐   ┌────────────┐
       │ RADIOLOGY  │   │ AI INFRA   │   │ MANAGEMENT │
       │   VLAN     │   │   VLAN     │   │   VLAN     │
       │            │   │            │   │            │
       │ 12 PCs     │   │ GPU Server │   │ Jump Box   │
       └─────┬──────┘   └─────┬──────┘   └─────┬──────┘
             │                │                │
             │                │                │
             │         ┌──────▼──────┐         │
             │         │    PACS     │         │
             │         │   SERVER    │         │
             │         └─────────────┘         │
             │                                  │
             │         ┌─────────────┐          │
             └────────►│ INTEGRATION │◄─────────┘
                       │   SERVER    │
                       └──────┬──────┘
                              │
                        Controlled
                        EHR Traffic
                              │
                       ┌──────▼──────┐
                       │ HOSPITAL EHR│
                       │   NETWORK   │
                       └─────────────┘

DESIGN JUSTIFICATION:
Why this topology meets the requirements
The proposed design creates a segmented AI infrastructure environment rather than placing the GPU server directly onto the general hospital network.
The basic architecture is:
Radiology → AI → PACS
and:
AI → Integration Server → EHR
while IT administration follows:
IT Administrator → Secure Remote Access/VPN → Management/Jump Server → Infrastructure
This provides controlled communication paths while preventing unnecessary connectivity.
The AI server can retrieve images from PACS without giving the AI infrastructure unrestricted access to the hospital network.
Likewise, the integration server provides a controlled communication point for sending AI results to the EHR.
SECURITY BENEFITS:
1. Protection of PHI
Patient images remain within the hospital's on-premises infrastructure.
The AI system does not need to send patient data to an external cloud service for inference.
This helps satisfy the hospital's requirement that patient data remain on-premises.
2. Network segmentation
The AI infrastructure is separated from general hospital systems.
If the AI server were compromised, an attacker would not automatically gain unrestricted access to the hospital's EHR or other systems.
3. Protection of proprietary AI models
The GPU server contains proprietary models.
Placing it in a restricted AI VLAN limits who can communicate with the server and reduces opportunities for unauthorized access or model theft.
4. Controlled EHR integration
The integration server acts as a controlled gateway between the AI environment and the EHR.
PERFORMANCE BENEFITS:
The AI GPU server is one of the most expensive components in the environment.
It therefore needs high-speed connectivity to PACS.
A recommended approach is to provide the AI server with a high-speed network connection, such as 10 GbE, assuming the existing hospital network infrastructure supports it.
This is particularly important because medical imaging studies can be large.
ALTERNATIVES CONSIDERED:
lternative 1: Put the AI server directly on the existing hospital network
The GPU server would simply receive an IP address on the existing hospital network.
Rejected because:
- Too much exposure.
- Difficult to enforce least-privilege access.
- A compromised AI server could potentially reach other hospital systems.
- Poor segmentation of PHI-related infrastructure.
Alternative 2: Put all AI infrastructure on one VLAN
The AI server, PACS integration, management systems, and radiologist workstations could all share one VLAN.
Rejected because:
- Provides insufficient internal segmentation.
- Radiologists could potentially reach management systems.
- A compromised workstation could potentially reach critical servers.
COMPONENT SUMMARY:
Enterprise Firewall - 1+	Controls traffic between hospital, AI, PACS, EHR, and management networks
Managed High-Speed Switches - 2+	Provides segmented and high-speed network connectivity
AI GPU Server - 1	Runs medical imaging AI models
PACS Server - 1 existing	Stores X-rays, CT scans, and other medical images
Integration Server - 1	Transfers AI results to the EHR
Jump/Management Server - 1	Secure administrative access
Radiologist Workstations - 12 existing	Submit studies and receive AI results
VPN - 1	Secure remote IT administration
Logging/Audit System - 1	HIPAA auditing, security monitoring, and event logging
UPS/Power Protection - As needed	Protects critical infrastructure
Backup Infrastructure - 1+	Disaster recovery and data protection


1. Which scenario was most challenging to design? Why?
The Regional Hospital AI Initiative was the most challenging scenario to design for me. It had the most complex requirements because it involved HIPAA compliance, patient health information, AI infrastructure, PACS, EHR integration, secure remote administration, and high availability. The hospital scenario also required consideration of performance because medical images can be very large, and the GPU server is an expensive resource that needs high-speed network connectivity. At the same time, security could not be sacrificed for performance.
2. What common patterns did you notice across all four scenarios?
The biggest common pattern was network segmentation. Each scenario had different groups of users or systems that needed different levels of access. VLANs, firewalls, and access-control rules were used to separate those groups. A major pattern was that the physical network does not necessarily need to be completely separate to provide logical separation. VLANs and firewall rules can create multiple secure network segments while still using shared physical infrastructure.
3. How did the design process differ from what you expected?
Before working through the scenarios, I expected network design to mainly involve choosing equipment such as switches, routers, and firewalls and then connecting everything together. I also learned that there is rarely one "correct" topology. The best design depends on the organization's size, budget, security requirements, performance needs, and regulatory requirements.

REFLECT QUESTIONS:
1. Compare your approach to the dental practice scenario versus the e-commerce scenario. How did the complexity differ? What additional considerations emerged?
The dental practice scenario was much simpler than the e-commerce scenario I feel. For the dental practice, the main goal was to separate the staff network from the patient WiFi network. A simple VLAN-based design with a firewall, managed switch, and wireless access point was enough to meet the requirements.
The e-commerce scenario required several layers of security because the web application had to be accessible from the public internet. Instead of just separating trusted and untrusted users, I had to separate the web, application, database, and management layers. I also had to consider redundancy because there were two web servers and two application servers.
2. The hospital AI scenario introduces integration with an existing network. How does this constraint change your design approach compared to greenfield deployments?
Working with an existing network makes the design more complicated because I cannot simply create the network from scratch. I have to understand the existing infrastructure and determine how the new AI environment can safely connect to it. I can design the network architecture from the beginning and decide where every VLAN, firewall, server, and connection should go. With the hospital, I have to work within existing systems such as the PACS and EHR networks.
3. Looking back at your designs, do you see patterns that repeat? What topology elements appear in multiple scenarios?
Yes. The biggest repeating pattern across all four scenarios was segmentation. Different users and systems had different access requirements, so separating them into VLANs or security zones was useful in almost every scenario.
4. If you had to explain network segmentation to a non-technical manager, how would you describe it based on what you learned this week?
I would describe network segmentation as putting different parts of a company into separate rooms with controlled doors between them. Imagine an office building where Finance, HR, Engineering, and Executives each have their own locked offices. Employees can enter their own department, but they cannot automatically enter everyone else's office. Executives may have keys to multiple offices because their job requires access to them.
5. How does the jump box concept from the lecture apply across all four scenarios? What variations did you need to consider?
The jump box provides a controlled doorway for administrators to reach sensitive systems. Instead of allowing administrators to directly connect to every server, they first authenticate to the jump box and then connect to the systems they are authorized to manage.
