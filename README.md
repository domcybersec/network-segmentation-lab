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
#### DHCP Infrastructure Services Lab
- Configured DHCP services on the router for multiple VLANs
- Created separate DHCP pools for HR, IT, and Guest VLANs
- Configured excluded addresses to reserve default gateway IPs
- Configured automatic distribution of IP addresses, subnet masks, default gateways, and DNS settings
- Converted end devices from static addressing to DHCP assignment
- Verified successful automatic IP assignment across all VLANs
- Maintained ACL-based Guest VLAN restrictions while implementing DHCP services
- Used ping and tracert to troubleshoot DHCP and ACL-related connectivity issues

##### DHCP Pool Design

| VLAN | Network | Default Gateway |
|------|----------|----------------|
| 10 | 192.168.10.0/24 | 192.168.10.1 |
| 20 | 192.168.20.0/24 | 192.168.20.1 |
| 30 | 192.168.30.0/24 | 192.168.30.1 |

##### DHCP Infrastructure Services - Troubleshooting
- Verified DHCP clients were receiving valid IP configurations
- Used tracert to identify that traffic was reaching the router but failing due to ACL behavior
- Determined that ICMP echo replies from the Guest VLAN were being blocked by the extended ACL
- Modified the named ACL (`GUEST-FILTER`) to permit ICMP echo replies from the Guest VLAN to internal VLANs
- Adjusted ACL rule ordering to ensure permit statements were processed before deny entries
- Retested connectivity successfully after ACL modification

##### DHCP Infrastructure Services - Lessons Learned
- DHCP automates IP address assignment and simplifies network management
- Excluded addresses prevent critical infrastructure IPs from being assigned dynamically
- ACLs can unintentionally block return traffic if protocols are not explicitly permitted
- ACL rule order is critical because ACLs are processed top-down
- ICMP troubleshooting tools such as ping and tracert are useful for isolating routing and filtering issues
- Infrastructure services must be validated alongside existing security controls
- Centralized DHCP configuration improves scalability and consistency across VLANs
#### Secure Management & SSH Lab
- Configured secure remote management access using SSH
- Created local administrator usernames and passwords
- Configured VTY lines for remote authentication
- Generated RSA keys for SSH encryption
- Restricted remote management access to SSH only
- Successfully established SSH connections from PC1 to R1, SW1, and SW2
- Disabled insecure Telnet-based remote access

##### SSH Configuration Features
- Local user authentication
- RSA key generation
- SSH Version 2
- VTY line configuration
- Secure remote CLI management

##### Secure Management & SSH - Troubleshooting
- Initial SSH connectivity issues occurred when using 2048-bit RSA keys in Packet Tracer
- Determined the issue was related to emulator limitations rather than configuration errors
- Regenerated RSA keys using 1024-bit encryption, which resolved the issue
- Verified successful SSH connectivity to all managed network devices

##### Secure Management & SSH - Lessons Learned
- SSH provides encrypted remote management access for network devices
- RSA keys are required for SSH functionality
- VTY lines control remote administrative access
- Local authentication improves management security
- Telnet is insecure because traffic is transmitted in plaintext
- Emulator environments may have limitations that differ from real enterprise hardware
- Troubleshooting should isolate whether issues are configuration-related or platform-related
### NAT/PAT and Simulated Internet Connectivity

Expanded the enterprise topology with an ISP router and external server to simulate Internet connectivity.

Implemented:

- ISP-facing routed connection using 203.0.113.0/30
- Simulated external server network using 198.51.100.0/24
- Static default routing toward the ISP
- NAT inside/outside interface designation
- Standard ACL for identifying internal NAT traffic
- PAT/NAT overload using the router's outside interface
- Internet connectivity for HR, IT, Guest, and Management networks
- Verification of NAT translations and statistics
- Validation that existing Guest VLAN ACL segmentation remained intact

Key commands:

    ip route 0.0.0.0 0.0.0.0 203.0.113.1
    ip nat inside
    ip nat outside
    ip nat inside source list 1 interface g0/0/1 overload
    show ip nat translations
    show ip nat statistics
    ### Port Security & Switch Hardening

Implemented Layer 2 access-port security and additional switch hardening on SW1 and SW2.

Implemented:

- Port Security on user-facing access ports
- Maximum of one secure MAC address per endpoint port
- Sticky MAC address learning
- Shutdown violation mode
- Simulated unauthorized-device Port Security violation
- Administrative recovery of a secure-shutdown interface
- VLAN 999 as a black-hole VLAN for unused interfaces
- Administrative shutdown of unused switchports
- Verification of secure MAC addresses and Port Security status

Key commands:

    switchport port-security
    switchport port-security maximum 1
    switchport port-security violation shutdown
    switchport port-security mac-address sticky
    show port-security
    show port-security address
    show port-security interface fa0/1
### STP / RSTP & Layer 2 Redundancy

Expanded the network to a three-switch redundant topology and implemented Spanning Tree Protocol.

Implemented and tested:

- STP root bridge election
- Root, Designated, and Alternate port roles
- STP path-cost selection
- Intentional SW1 primary / SW2 secondary root design
- Redundant Layer 2 link failover
- Rapid PVST+ migration
- Rapid STP convergence testing
- PortFast on endpoint-facing interfaces
- BPDU Guard
- Simulated rogue-switch BPDU Guard violation
- Post-change connectivity and security verification

MODULE 11 - ETHERCHANNEL / LACP
===============================

OVERVIEW
--------

Implemented an IEEE LACP EtherChannel between SW1 and SW2 to provide link aggregation and physical-link redundancy.

Two FastEthernet links were bundled into the logical Port-channel1 interface and configured as an 802.1Q trunk.

This module also demonstrated the interaction between EtherChannel and Rapid-PVST and tested connectivity during a physical member-link failure.


IMPLEMENTATION
--------------

Physical EtherChannel members:

SW1 Fa0/21 <--> SW2 Fa0/21
SW1 Fa0/22 <--> SW2 Fa0/22

Logical interface:

Port-channel1 (Po1)

LACP mode:

SW1 - Active
SW2 - Active

Port-channel1 was configured as an 802.1Q trunk carrying:

- VLAN 10 - HR
- VLAN 20 - IT
- VLAN 30 - Guest
- VLAN 99 - Management

VLAN 999 was excluded because it is used as the blackhole/parking VLAN for unused switch ports.


CONFIGURATION
-------------

SW1:

interface range FastEthernet0/21 - 22
 switchport mode trunk
 channel-group 1 mode active
 no shutdown

interface Port-channel1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,99


SW2:

interface range FastEthernet0/21 - 22
 switchport mode trunk
 channel-group 1 mode active
 no shutdown

interface Port-channel1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,99


VERIFICATION
------------

Successful LACP negotiation was verified with:

show etherchannel summary

Healthy EtherChannel:

Po1(SU)   LACP   Fa0/21(P) Fa0/22(P)

This confirmed that:

- Po1 was operating as a Layer 2 EtherChannel.
- The Port-channel was in use.
- Fa0/21 and Fa0/22 were successfully bundled using LACP.

Trunk operation was verified with:

show interfaces trunk

Port-channel1 successfully carried VLANs 10,20,30,99.


SPANNING TREE INTEGRATION
-------------------------

Rapid-PVST initially preferred the existing GigabitEthernet0/2 link between SW1 and SW2 because its STP cost was lower than the FastEthernet EtherChannel.

SW2 initially showed:

Gi0/2   Root FWD   Cost 4
Po1     Altn BLK   Cost 12

After the direct GigabitEthernet0/2 link was disabled, Rapid-PVST selected Port-channel1 as SW2's root path:

Po1     Root FWD   Cost 12

This demonstrated that spanning tree treats the EtherChannel as a single logical interface.


REDUNDANCY TEST
---------------

A physical member-link failure was simulated by shutting down SW2 Fa0/21.

EtherChannel verification then showed:

Po1(SU)   LACP   Fa0/21(D) Fa0/22(P)

Port-channel1 remained operational through Fa0/22.

During the failure:

- Po1 remained up.
- Rapid-PVST continued using Po1 as the root path.
- PC2 successfully continued communicating with PC1.
- End-to-end connectivity survived the physical member-link failure.

This demonstrated the link redundancy provided by EtherChannel.


TROUBLESHOOTING
---------------

During initial testing, Packet Tracer produced inconsistent Rapid-PVST behavior after the original GigabitEthernet0/2 path was disabled.

The EtherChannel configuration was removed, cleaned, and rebuilt.

After rebuilding the LACP bundle, Port-channel1 correctly transitioned to the STP Root Forwarding state and host connectivity succeeded.

Commands used for configuration verification and troubleshooting included:

show etherchannel summary
show interfaces port-channel 1
show interfaces trunk
show spanning-tree vlan 10
show spanning-tree inconsistentports
show running-config


SKILLS DEMONSTRATED
-------------------

- EtherChannel configuration
- LACP negotiation
- Layer 2 link aggregation
- 802.1Q trunk configuration
- Rapid-PVST and EtherChannel integration
- STP path-cost analysis
- Physical link redundancy
- Member-link failure testing
- End-to-end connectivity verification
- Cisco IOS troubleshooting