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

#### Basic Topology 
- Configured router and switch connectivity
- Assigned static IP Addresses
- Verified PC to PC and PC to Router connectivity via Ping
- Troubleshot interface issues
##### Basic Topology Lessons Learned
- Learned functional between switches and routers
- Learned the difference between Administratively Down and Up
- Learned why it is important to be careful with how you designate IP addresses
- Practiced troubleshooting logical and physical connectivity 
- Improved familiarity with Cisco CLI navigation
#### Vlan Segmentation Lab
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