# Network Segmentation Lab

This project documents my CCNA-focused Packet Tracer Labs.

## Goals
- Learn networking fundamentals
- Practice subnetting and routing
- Build hands-on lab experience 

## Tools
- Cisco Packet Tracer
- Git & GitHub
- VS Code

## Labs Completed

#### Basic Topology Lab
- Configured router and switch connectivity
- Assigned static IP Addresses
- Verified PC to PC and PC to Router connectivity via Ping
- Troubleshot interface issues
##### Basic Topology Lab Lessons Learned
- Learned functional differences between switches and routers
- Learned the difference between administratively down and operational interfaces
- Learned why it is important to be careful with how you designate IP addresses
- Practiced troubleshooting logical and physical connectivity 
- Improved familiarity with Cisco CLI navigation
#### VLAN Segmentation Lab
- Created VLANs for HR, IT, and Guest departments
- Assigned switch access ports to specific VLANs
- Configured static IP addressing for each department subnet
- Verified same-VLAN connectivity using ping testing
- Verified VLAN isolation by testing failed communication between different VLANs
- Used show vlan brief to validate VLAN assignments
##### VLAN Segmentation - VLAN Design 
| VLAN | Department | Network |
|------|-------------|----------|
| 10 | HR | 192.168.10.0/24 |
| 20 | IT | 192.168.20.0/24 |
| 30 | Guest | 192.168.30.0/24 |
##### VLAN Segmentation - Lessons Learned
- VLANs provide logical segmentation even on the same physical switch
- Devices in different VLANs cannot communicate without Layer 3 routing
- Access ports assign end devices to specific VLANs
- VLAN segmentation improves organization and security
- Proper VLAN planning reduces unnecessary broadcast traffic
- show vlan brief is useful for verifying VLAN assignments and switch port membership
#### VLAN Trunking Lab
- Configured trunk links between multiple switches
- Extended VLANs across separate switches using trunk ports
- Verified same-VLAN communication across switches
- Verified VLAN isolation between separate VLANs
- Used trunk verification commands for troubleshooting and validation

##### VLAN Trunking - Connectivity Verification
- HR VLAN devices successfully communicated across switches through trunk links
- Devices in separate VLANs remained isolated without Layer 3 routing
- Verified VLAN assignments using `show vlan brief`
- Verified trunk operation using `show interfaces trunk`

##### VLAN Trunking - Lessons Learned
- Trunk ports allow multiple VLANs to traverse between switches
- Access ports are used for end devices while trunk ports connect networking devices
- VLANs can span multiple switches through trunk links
- Proper trunk configuration is critical for VLAN communication between switches
- Layer 2 switching alone does not allow communication between separate VLANs
#### Inter-VLAN Routing Lab
- Configured router-on-a-stick inter-VLAN routing
- Created router subinterfaces for VLAN 10, VLAN 20, and VLAN 30
- Configured 802.1Q encapsulation on router subinterfaces
- Assigned default gateways for each VLAN
- Verified successful communication between separate VLANs
- Used ping and tracert for routing verification and troubleshooting

##### Inter-VLAN Routing - Gateway Design

| VLAN | Gateway |
|------|----------|
| 10 | 192.168.10.1 |
| 20 | 192.168.20.1 |
| 30 | 192.168.30.1 |

##### Inter-VLAN Routing - Lessons Learned
- Routers enable communication between separate VLANs
- Router subinterfaces act as default gateways for VLANs
- 802.1Q encapsulation allows multiple VLANs to traverse a single physical interface
- Default gateway configuration is critical for inter-network communication
- tracert can help identify where communication failures occur in the network path
- Endpoint misconfigurations can prevent connectivity even when routing infrastructure is functioning properly
#### ACL Segmentation & Security Lab
- Configured extended ACLs to enforce VLAN security policies
- Created a named extended ACL (`GUEST-FILTER`) to restrict Guest VLAN access
- Applied ACL filtering inbound on the Guest VLAN router subinterface
- Denied Guest VLAN access to HR and IT networks
- Permitted authorized inter-VLAN communication between trusted departments
- Verified ACL functionality through successful and failed ping testing

##### ACL Security Policy

| Source VLAN | Destination VLAN | Result |
|-------------|------------------|--------|
| Guest | HR | Denied |
| Guest | IT | Denied |
| HR | IT | Permitted |
| IT | HR | Permitted |

##### ACL Segmentation - Lessons Learned
- Extended ACLs can filter traffic based on source and destination networks
- ACLs are processed top-down and stop at the first matching rule
- ACL placement is important for both efficiency and security
- Inbound ACLs filter traffic as it enters an interface
- Named ACLs improve readability and management compared to numbered ACLs
- The implicit deny statement at the end of ACLs can unintentionally block traffic if permit rules are not configured properly
- Applying ACLs close to the traffic source reduces unnecessary routed traffic