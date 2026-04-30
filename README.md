## Project Overview

This project simulates a corporate network environment for a company with 
four departments — Finance, HR, IT and Guest. The network was designed and 
built in two phases, starting with a core foundation and expanding to include 
dedicated infrastructure servers and enhanced security controls.

The goal was to demonstrate practical networking skills relevant to 
real-world enterprise environments while following Cisco best practices 
throughout.

---

## Technologies Used

- Cisco Packet Tracer
- VLANs (IEEE 802.1Q)
- Router-on-a-Stick (Inter-VLAN Routing)
- DHCP (Dedicated Server)
- Extended Access Control Lists (ACL)
- SSH v2 Remote Management
- Port Security (Sticky MAC)
- NTP (Network Time Protocol)
- Syslog (Centralised Logging)
- VLAN 99 Management Network

---

## Network Topology

### Phase 1 — Core Network
![Phase 1 Topology](topology/phase1-diagram.png)

### Phase 2 — Expanded Network
![Phase 2 Topology](topology/phase2-diagram.png)

---

## VLAN Design

| VLAN ID | Name | Department | Subnet |
|---|---|---|---|
| 10 | FINANCE | Finance | 192.168.10.0/24 |
| 20 | HR | Human Resources | 192.168.20.0/24 |
| 30 | IT | Information Technology | 192.168.30.0/24 |
| 40 | GUEST | Guest | 192.168.40.0/24 |
| 99 | MANAGEMENT | Network Management | 192.168.99.0/24 |

---

## Key Features

### Phase 1

- Four department VLANs with full traffic segmentation
- Router-on-a-Stick inter-VLAN routing via 802.1Q subinterfaces
- DHCP configured on router with per-department pools
- Extended ACL blocking Guest department from accessing IT resources
- SSH v2 remote management restricted to IT department only
- Dedicated management VLAN 99 isolating administrative traffic
- Basic device hardening on router and switch

### Phase 2

- DHCP migrated to dedicated server (SRV-DHCP-01)
- Router reconfigured as DHCP relay agent using ip helper-address
- NTP server (SRV-NTP-01) deployed for network time synchronisation
- Syslog server (SRV-LOG-01) deployed for centralised log management
- Port security with sticky MAC configured on all active access ports
- Unused switch ports administratively disabled
- Service password encryption enabled on all devices
- All infrastructure servers placed in IT VLAN with static addressing

---

## Security Implementation

| Control | Layer | Implementation |
|---|---|---|
| VLAN Segmentation | Layer 2 | Traffic isolation per department |
| ACL | Layer 3 | Guest blocked from IT subnet |
| Port Security | Layer 2 | Sticky MAC, violation shutdown |
| SSH v2 | Management | IT access only, Telnet disabled |
| Management VLAN | Layer 2 | Admin traffic isolated on VLAN 99 |
| Unused Ports | Layer 1 | Administratively shutdown |
| Password Encryption | Device | service password-encryption enabled |
| Syslog Monitoring | Monitoring | All security events logged centrally |

---

## Infrastructure Servers

| Device | Hostname | IP Address | Purpose |
|---|---|---|---|
| DHCP Server | SRV-DHCP-01 | 192.168.30.2 | IP address assignment |
| NTP Server | SRV-NTP-01 | 192.168.30.3 | Time synchronisation |
| Syslog Server | SRV-LOG-01 | 192.168.30.4 | Centralised logging |

---

## Repository Structure

cisco-enterprise-network-lab/
│
├── README.md
├── topology/
│   ├── phase1-network.pkt
│   ├── phase1-diagram.png
│   ├── phase2-network.pkt
│   └── phase2-diagram.png
├── documentation/
│   └── network-documentation.pdf
└── configs/
├── phase1-router-config.txt
├── phase1-switch-config.txt
├── phase2-router-config.txt
└── phase2-switch-config.txt

---

## Testing and Verification

| Test | Description | Result |
|---|---|---|
| DHCP Assignment | All PCs received correct IP per VLAN | ✅ Pass |
| Gateway Ping | Each PC reached its default gateway | ✅ Pass |
| Inter-VLAN Routing | Departments can communicate | ✅ Pass |
| ACL Enforcement | Guest cannot reach IT subnet | ✅ Pass |
| SSH Access | PC-IT successfully SSH into switch | ✅ Pass |
| SSH Blocked | Non-IT departments cannot SSH | ✅ Pass |
| Port Security | Rogue device triggers err-disabled | ✅ Pass |
| Syslog | Security events logged on SRV-LOG-01 | ✅ Pass |

---

## Design Decisions

**VLANs over Physical Separation**
VLANs were chosen over dedicated switches per department to reflect real-world cost efficiency and scalability. A single managed switch provides the same logical traffic separation at a fraction of the 
hardware cost.

**Router-on-a-Stick**
Implemented to demonstrate subinterface and 802.1Q trunk configuration. In a production environment a Layer 3 switch using SVIs would be preferred for better performance and scalability.

**Dedicated DHCP Server**
DHCP was migrated from the router to a dedicated server in Phase 2 to eliminate a single point of failure and separate service roles following enterprise best practice.

**Management VLAN 99**
A dedicated management VLAN was created rather than using the default VLAN 1, isolating administrative traffic and following Cisco security best practice.

**Port Speed Allocation**
GigabitEthernet ports were assigned to IT and Finance due to bandwidth-intensive workloads. HR and Guest were assigned FastEthernet ports as their requirements are less demanding.

---

## What I Would Do Differently

- Implement a Layer 3 switch to handle inter-VLAN routing via SVIs, removing the Router-on-a-Stick bottleneck
- Add a redundant router and switch with Hot Standby Router Protocol(HSRP) for high availability
- Deploy a dedicated internal DNS server rather than using 8.8.8.8
- Configure NTP authentication using MD5 keys to prevent NTP spoofing
- Implement a firewall between the router and external network
- Further restrict the Guest VLAN to internet access only
- Use a fully featured DHCP server supporting granular lease time configuration per pool, the Guest pool would use a 4 hour lease to prevent address exhaustion

---

## What I Learned

This project taught me the importance of planning a network design before configuration. Building the network in two phases demonstrated how a core foundation can be progressively hardened and expanded to meet enterprise requirements. I've developed a deeper understanding of how security controls work together at different layers — VLANs for segmentation, ACLs for traffic filtering, port security for physical access control, and SSH for secure management. I also gained practical experience identifying the gap between lab environments and production networks, particularly around dedicated server roles, redundancy and enterprise grade hardware.

---

## Requirements

- Cisco Packet Tracer 8.0 or later recommended
- Open .pkt files directly in Packet Tracer

---
