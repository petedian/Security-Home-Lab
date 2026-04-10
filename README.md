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

Ref 2:Cisco Network Appliances
![Cisco](https://github.com/user-attachments/assets/84d89835-2331-4280-b87a-faed162b5fe8)

Ref 3: Dell Type 1 Hyperviser Server Proxmox VE
![Dell Poweredge r620](https://github.com/user-attachments/assets/58cd2abe-7440-4460-88e3-4bbdd8c5f419)

### Skills Learned
[Bullet Points - Remove this afterwards]

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used
[Bullet Points - Remove this afterwards]

- Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.

## Steps

Ref 1: Network Diagram



