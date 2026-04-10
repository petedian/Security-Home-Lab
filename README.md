# Security-Home-Lab

## Lab Architecture & Technical Overview

This home lab is hosted on a Dell PowerEdge R620 server running Proxmox VE as the Type-1 hypervisor. The physical networking is isolated from the main ISP network using a dedicated Cisco Enterprise Router, Switch, and Firewall stack, ensuring a "sandbox" environment for malware and attack simulation.

The Detection Lab project aimed to establish a controlled environment for simulating and detecting cyber attacks. The primary focus was to ingest and analyze logs within a Security Information and Event Management (SIEM) system, generating test telemetry to mimic real-world attack scenarios. This hands-on experience was designed to deepen understanding of network security, attack patterns, and defensive strategies.


Hardware Stack
Server: Dell PowerEdge R620 (Dual Intel Xeon, 128GB+ RAM recommended)

Hypervisor: Proxmox VE

Networking: Cisco Enterprise Router & Catalyst Switch (Hardware Isolation)

Virtual Firewall: pfSense (Internal routing and inter-VLAN control)

Ref 1: Network Diagram
<img width="617" height="792" alt="Infrastructure Diagram" src="https://github.com/user-attachments/assets/943587c6-17b6-4c6f-a88c-bb2dfdccd4a4" />

Network Segmentation (VLANs)
The environment is segmented into three distinct zones to mimic a corporate infrastructure:

VLAN 40 (Attack LAN): 10.1.4.0/24 — Kali Linux instance for penetration testing and telemetry generation.

VLAN 20 (Target Environment - E Corp): 10.1.2.0/24 — Windows Domain Controller (DC-1) and Windows 11 endpoints (Win-11-1, Win-11-2).

VLAN 30 (Security Operations - Viper MSSP): 10.1.3.0/24 — Centralized monitoring using Wazuh (SIEM/XDR) and Velociraptor for endpoint visibility and forensics.

Ref 2:Cisco Network Appliances
![Cisco](https://github.com/user-attachments/assets/84d89835-2331-4280-b87a-faed162b5fe8)

Ref 3: Dell Type 1 Hyperviser Server Proxmox VE
![Dell Poweredge r620](https://github.com/user-attachments/assets/58cd2abe-7440-4460-88e3-4bbdd8c5f419)

Tools Used
Proxmox VE: Managed virtualization for all server and client nodes.
Ref 4:Proxmox Node with Operating Systems Installed via ISO Images
<img width="1887" height="927" alt="Setup SOC Lab Infrastructure" src="https://github.com/user-attachments/assets/31afdf4a-6d21-4669-9e0c-c9368f990e19" />



pfSense: Provided granular firewall rules between the Attack, Victim, and SOC networks.

Ref 5: PfSense
<img width="1310" height="498" alt="Firewall Rules 1" src="https://github.com/user-attachments/assets/e172bc8b-613a-4b89-8a70-0a1be214d3b5" />

<img width="1241" height="472" alt="Firewall Rules 2" src="https://github.com/user-attachments/assets/a418c7e0-d2e9-4dcf-9a38-482821c862a8" />

<img width="1188" height="462" alt="Firewall Rules 3" src="https://github.com/user-attachments/assets/192fa142-74ed-482a-bcea-00fa5d3cad7b" />

<img width="1213" height="411" alt="Firewall Rules 4" src="https://github.com/user-attachments/assets/dd37e768-ac09-4a4d-9338-8a7e60dbce8d" />

<img width="1323" height="829" alt="Setup pfSense" src="https://github.com/user-attachments/assets/340be815-d313-4a4c-98bd-3c3a74199aad" />


Wazuh: Used for log ingestion, file integrity monitoring (FIM), and active response.

Velociraptor: Employed for advanced endpoint monitoring and digital forensics.

Kali Linux: Utilized for generating attack traffic (Metasploit, Nmap, etc.) to test detection rules.

Active Directory (Windows Server): Established a realistic corporate identity and access management target.

Implementation Steps
Physical Networking & Isolation:

Configured the Cisco hardware stack to sit behind the ISP router, creating a physical "air-gap" or isolated management segment.

Cabled the Dell R620 to the Cisco switch to trunk multiple VLANs into the Proxmox environment.

Virtual Infrastructure Setup:

Installed Proxmox VE on the Dell R620 and configured Linux Bridges for VLAN awareness.

Deployed a pfSense VM as the primary gateway, defining the interfaces for VLANs 20, 30, and 40.

Endpoint & Domain Deployment:

Built the "E Corp" domain by promoting a Windows Server instance to Domain Controller.

Joined Windows 11 client VMs to the domain and verified connectivity.

SOC Integration:

Deployed the Viper MSSP stack: Ubuntu-based Wazuh Server and Velociraptor.

Installed Wazuh and Velociraptor agents on all Windows and Kali endpoints.

Telemetry & Testing:

Executed initial projects (each have their own post here:)




### Skills Learned

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.






