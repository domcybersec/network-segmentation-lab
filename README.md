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