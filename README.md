# Security-Operations-Home-Lab

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

### Network Segmentation (VLANs)
The environment is segmented into three distinct zones to mimic a corporate infrastructure:

VLAN 40 (Attack LAN): 10.1.4.0/24 — Kali Linux instance for penetration testing and telemetry generation.

VLAN 20 (Target Environment - E Corp): 10.1.2.0/24 — Windows Domain Controller (DC-1) and Windows 11 endpoints (Win-11-1, Win-11-2).

VLAN 30 (Security Operations - Viper MSSP): 10.1.3.0/24 — Centralized monitoring using Wazuh (SIEM/XDR) and Velociraptor for endpoint visibility and forensics.

Ref 2:Cisco Network Appliances
![Cisco](https://github.com/user-attachments/assets/84d89835-2331-4280-b87a-faed162b5fe8)

Ref 3: Dell Type 1 Hyperviser Server Proxmox VE
![Dell Poweredge r620](https://github.com/user-attachments/assets/58cd2abe-7440-4460-88e3-4bbdd8c5f419)

### Tools Used

Proxmox VE: Managed virtualization for all server and client nodes.

Ref 4:Proxmox Node with Operating Systems Installed via ISO Images

<img width="1887" height="927" alt="Setup SOC Lab Infrastructure" src="https://github.com/user-attachments/assets/31afdf4a-6d21-4669-9e0c-c9368f990e19" />



### pfSense: 
Installed ISO Image and will come back to setup later in series after we add OpenVSwtich (OVS)


### Installed SPICE 
(Simple Protocol for Independent Computing Environments):
Integrated to provide high-performance, low-latency remote console access to the virtualized endpoints. This allowed for seamless interaction with the Windows 11 targets and Kali Linux attack node, supporting multi-monitor setups and shared clipboards between the lab and the host machine. 

Ref 5: SPICE

<img width="1895" height="252" alt="Setup Spice" src="https://github.com/user-attachments/assets/48cddf28-9273-40dc-9699-e0b765f79e05" />


### Open vSwitch (OVS) Integration:
Replaced standard Linux bridges with Open vSwitch within the Proxmox hypervisor. This upgrade provides a more robust, SDN-ready (Software Defined Networking) foundation, enabling advanced features like VLAN tagging at the virtual switch level and providing the infrastructure necessary for deep packet inspection and network telemetry.

Ref 6: OVS

<img width="1134" height="654" alt="Setup OVS" src="https://github.com/user-attachments/assets/8555952f-cda6-4458-86fd-c5260f5f0f23" />

### pfSense: 
Provided granular firewall rules between the Attack, Victim, and SOC networks.

Ref 7-11: PfSense
<img width="1310" height="498" alt="Firewall Rules 1" src="https://github.com/user-attachments/assets/e172bc8b-613a-4b89-8a70-0a1be214d3b5" />

<img width="1241" height="472" alt="Firewall Rules 2" src="https://github.com/user-attachments/assets/a418c7e0-d2e9-4dcf-9a38-482821c862a8" />

<img width="1188" height="462" alt="Firewall Rules 3" src="https://github.com/user-attachments/assets/192fa142-74ed-482a-bcea-00fa5d3cad7b" />

<img width="1213" height="411" alt="Firewall Rules 4" src="https://github.com/user-attachments/assets/dd37e768-ac09-4a4d-9338-8a7e60dbce8d" />

<img width="1323" height="829" alt="Setup pfSense" src="https://github.com/user-attachments/assets/340be815-d313-4a4c-98bd-3c3a74199aad" />

### Deployment of Active Directory Domain Services (AD DS)
With the network infrastructure and gateways established, we deployed a Windows Server instance to serve as the backbone of the "E Corp" corporate environment. This step was essential for establishing centralized identity management and providing a realistic target for SOC monitoring and attack simulations.

Key Implementation Details:

Domain Controller Promotion: Configured the primary server as a Domain Controller (DC-1) for the ecorp.local forest.

DNS & DHCP Integration: Set up integrated DNS to handle internal name resolution for all domain-joined assets, ensuring seamless communication across the 10.1.2.0/24 subnet.

Organizational Unit (OU) Structure: Created a logical hierarchy within AD to manage users, groups, and workstations, mirroring a standard business environment.

Group Policy Objects (GPOs): Initialized baseline security policies and audit logging configurations. This is a critical step for the SOC lab, as it ensures the Windows endpoints generate the necessary event logs (like Process Creation and PowerShell logging) for Wazuh to ingest.

Snapshot Strategy: Prior to promotion, a Proxmox snapshot was taken to establish a "clean slate" recovery point for the domain environment.

Ref 12: Active Directory

<img width="960" height="725" alt="Setup Users and Groups" src="https://github.com/user-attachments/assets/f97fc61f-852d-48ff-8fc5-0767a12e5c15" />

### Intentional Deactivation of Windows Defender
During the "E Corp" domain setup, Windows Defender was intentionally disabled across the Windows endpoints and the Domain Controller. While counterintuitive in a production environment, this was done for two specific technical reasons:

Telemetry Isolation: By removing the "noise" and automated blocking of Windows Defender, we ensured that the Wazuh and Velociraptor agents were the primary sources of telemetry. This allows us to see exactly how these specific SOC tools detect and alert on raw, unblocked malicious activity.

Attack Simulation Accuracy: In a learning environment, Defender often "kills" exploits before they can execute. Disabling it allows us to study the full lifecycle of an attack—from initial execution to lateral movement—providing a much richer dataset for log analysis and behavioral detection tuning.

Performance Optimization: Reducing background resource consumption on the virtualized nodes ensures the Dell R620 can maintain high performance during heavy simulation testing.

Ref 13-14: Disabled Windows Security

<img width="1890" height="996" alt="Setup Disable Windows Defender" src="https://github.com/user-attachments/assets/eca3e7dc-1883-4936-8b22-6cebe05ff2fb" />

<img width="1854" height="986" alt="Start UP Enable Disable GP MS Defender" src="https://github.com/user-attachments/assets/ac226cdc-b06d-4172-8bbb-816ec8efc630" />


### User Management & Domain Integration

Once the Domain Controller was stabilized, we populated the environment with users and configured the necessary policies to ensure the endpoints were ready for active monitoring.

Key Implementation Details:

User & Group Creation: Established a set of "Victim" user accounts and administrative groups within Active Directory to simulate a real-world workforce.

Domain Joins: Successfully joined the Win-11-1 and Win-11-2 workstations to the ecorp.local domain, verifying that the DNS settings through pfSense and the DC were correctly resolving.

Security & Audit Policies:

Configured Group Policy Objects (GPOs) to enable "Audit Process Creation" and "Include Command Line in Process Creation Events" (Event ID 4688).

Enabled PowerShell Script Block Logging to ensure that any malicious scripts executed during testing would be captured in the logs for the SIEM to ingest.

Verification: Logged into the workstations using the newly created domain credentials to confirm profile synchronization and policy application.

Ref 15: User Group and Policies

<img width="960" height="725" alt="Setup Users and Groups" src="https://github.com/user-attachments/assets/be8e7398-b450-40af-84d0-5d23c98d348b" />

Ref 16-17: Joined Users to Domain

<img width="1883" height="1010" alt="Setup User Joined Domain confirm" src="https://github.com/user-attachments/assets/5112f93b-2541-4721-adce-d6bca113524a" />

<img width="1908" height="942" alt="Setup, Join endpoint to Domain" src="https://github.com/user-attachments/assets/68cd5bcb-a1d3-4390-9d8c-fa7bd5745610" />

### Deployment of Microsoft Sysmon
To augment the standard Windows Event Logs, we deployed Microsoft Sysmon across all "E Corp" endpoints. This provides the high-fidelity telemetry required for advanced threat hunting and behavioral analysis.

Implementation Highlights:

Granular Visibility: Configured Sysmon to capture critical events that standard logging often misses, such as Process Creation (Event ID 1) with full command-line arguments, Network Connections (Event ID 3), and File Creation (Event ID 11).

Configuration Schema: Utilized a customized configuration file (based on industry standards like SwiftOnSecurity) to filter out "noise" while ensuring that suspicious activity—such as Rundll32 executing code or unusual PowerShell behavior—is strictly logged.

SIEM Integration: Established the foundation for Wazuh to ingest these Sysmon logs, allowing us to build detection rules based on specific sub-processes and hash values (SHA256) captured by the tool.

Verification: Confirmed successful installation via the Windows Event Viewer (Applications and Services Logs > Microsoft > Windows > Sysmon > Operational), ensuring logs were being generated in real-time.

Ref 18-22: Sysmon

<img width="1889" height="850" alt="Setup Sysmon 1" src="https://github.com/user-attachments/assets/de0c89e1-8186-4f5d-8e46-e8fa6c0a612d" />

<img width="1889" height="963" alt="Setup Sysmon 2 Github Modular" src="https://github.com/user-attachments/assets/ecd11372-694b-4428-99f3-4db0ddf2e4ae" />

<img width="1900" height="978" alt="Setup Sysmon 3 Modular Cut Paste to Notepad" src="https://github.com/user-attachments/assets/144c7d3b-d195-4925-8b2d-58792e2c9f90" />

<img width="1519" height="808" alt="Setup Sysmon 4 CLI Command" src="https://github.com/user-attachments/assets/290f424e-3291-40e7-a9c5-24870872817f" />

<img width="1689" height="956" alt="Setup Sysmon Event Viewer" src="https://github.com/user-attachments/assets/a69ab2a7-258b-4a50-9208-285f9c17d499" />






Telemetry & Testing:

Executed initial projects (each have their own post here:)




### Skills Learned

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.






