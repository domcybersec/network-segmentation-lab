# Network Segmentation Lab

This project documents my CCNA-focused Cisco Packet Tracer lab environment.

The lab began as a basic router-and-switch topology and has been progressively expanded into a small enterprise-style network incorporating VLAN segmentation, inter-VLAN routing, access control, DHCP, secure management, NAT/PAT, Layer 2 security, redundancy, EtherChannel, and dynamic routing.

## Goals

- Learn and reinforce networking fundamentals
- Practice subnetting, switching, and routing
- Develop hands-on Cisco IOS experience
- Practice structured network troubleshooting
- Integrate networking technologies into a single evolving topology
- Build a documented networking portfolio using Git and GitHub

## Tools

- Cisco Packet Tracer
- Cisco IOS CLI
- Git
- GitHub
- VS Code

---

# Labs Completed

## Module 1 - Basic Topology

### Overview

Built the initial router, switch, and end-device topology and established basic IPv4 connectivity.

### Implemented

- Configured router and switch connectivity
- Assigned static IPv4 addresses
- Verified PC-to-PC connectivity
- Verified PC-to-router connectivity
- Troubleshot physical and logical interface issues
- Practiced Cisco IOS CLI navigation

### Lessons Learned

- Functional differences between routers and switches
- Difference between administratively down and operationally down interfaces
- Importance of proper IP addressing
- Basic physical and logical connectivity troubleshooting
- Cisco IOS configuration and verification workflow

---

## Module 2 - VLAN Segmentation

### Overview

Introduced logical network segmentation by separating HR, IT, and Guest devices into individual VLANs.

### Implemented

- Created VLANs for HR, IT, and Guest departments
- Assigned access ports to specific VLANs
- Configured separate IPv4 subnets
- Verified same-VLAN connectivity
- Verified isolation between different VLANs
- Used `show vlan brief` for validation

### VLAN Design

| VLAN | Department | Network |
|---|---|---|
| 10 | HR | `192.168.10.0/24` |
| 20 | IT | `192.168.20.0/24` |
| 30 | Guest | `192.168.30.0/24` |

### Lessons Learned

- VLANs provide logical segmentation on shared switching infrastructure
- Devices in different VLANs require Layer 3 routing to communicate
- Access ports assign end devices to specific VLANs
- VLAN segmentation improves organization and security
- VLAN design reduces unnecessary broadcast domains
- `show vlan brief` is useful for validating VLAN assignments

---

## Module 3 - VLAN Trunking

### Overview

Expanded the network to multiple switches and extended VLANs between them using 802.1Q trunk links.

### Implemented

- Configured trunk links between switches
- Extended VLANs across multiple switches
- Verified same-VLAN communication across switches
- Verified isolation between separate VLANs
- Used trunk verification commands for troubleshooting

### Verification

- HR devices successfully communicated across switches
- Devices in separate VLANs remained isolated before Layer 3 routing
- Verified VLAN assignments using `show vlan brief`
- Verified trunk operation using `show interfaces trunk`

### Lessons Learned

- Trunks allow multiple VLANs to traverse a single link
- Access ports connect endpoint devices
- Trunk ports commonly connect network infrastructure
- VLANs can span multiple switches
- Layer 2 switching alone cannot provide inter-VLAN communication

---

## Module 4 - Inter-VLAN Routing

### Overview

Implemented router-on-a-stick routing to provide Layer 3 connectivity between VLANs.

### Implemented

- Configured router-on-a-stick
- Created router subinterfaces for VLANs 10, 20, and 30
- Configured 802.1Q encapsulation
- Assigned default gateways to each VLAN
- Verified inter-VLAN communication
- Used `ping` and `tracert` for troubleshooting

### Gateway Design

| VLAN | Gateway |
|---|---|
| 10 | `192.168.10.1` |
| 20 | `192.168.20.1` |
| 30 | `192.168.30.1` |

### Lessons Learned

- Routers provide communication between separate VLANs
- Router subinterfaces can serve as VLAN default gateways
- 802.1Q allows multiple VLANs to share one physical router interface
- Correct default gateway configuration is required for routed communication
- `tracert` helps identify where traffic stops
- Endpoint configuration errors can cause failures even when infrastructure is working correctly

---

## Module 5 - ACL Segmentation and Security

### Overview

Implemented extended access control lists to enforce security boundaries between trusted and untrusted VLANs.

### Implemented

- Created the named extended ACL `GUEST-FILTER`
- Applied the ACL inbound on the Guest VLAN router subinterface
- Denied Guest access to HR
- Denied Guest access to IT
- Allowed trusted internal communication
- Allowed required ICMP return traffic
- Verified policies using successful and failed ping tests

### Security Policy

| Source | Destination | Result |
|---|---|---|
| Guest | HR | Denied |
| Guest | IT | Denied |
| HR | IT | Permitted |
| IT | HR | Permitted |
| HR / IT | Guest | Permitted |

### Lessons Learned

- Extended ACLs can filter by source and destination
- ACL entries are processed from top to bottom
- Processing stops at the first matching rule
- ACL placement affects efficiency and security
- Inbound ACLs filter packets as they enter an interface
- Named ACLs improve readability
- The implicit deny can unintentionally block traffic
- Traditional ACLs are stateless and may require explicit return-traffic rules

---

## Module 6 - DHCP Infrastructure Services

### Overview

Replaced static endpoint addressing with centralized DHCP services hosted on R1.

### Implemented

- Configured DHCP pools for HR, IT, and Guest VLANs
- Configured excluded addresses
- Distributed IPv4 addresses automatically
- Distributed subnet masks automatically
- Distributed default gateways automatically
- Distributed DNS configuration
- Converted endpoints from static addressing to DHCP
- Preserved existing ACL security policies

### DHCP Design

| VLAN | Network | Default Gateway |
|---|---|---|
| 10 | `192.168.10.0/24` | `192.168.10.1` |
| 20 | `192.168.20.0/24` | `192.168.20.1` |
| 30 | `192.168.30.0/24` | `192.168.30.1` |

### Troubleshooting

During testing, `tracert` showed that traffic reached the router but ICMP replies returning from the Guest VLAN were blocked by the existing ACL.

The `GUEST-FILTER` ACL was updated to explicitly permit required ICMP echo replies before the Guest-to-internal deny statements.

### Lessons Learned

- DHCP simplifies endpoint configuration
- Excluded addresses protect infrastructure IP assignments
- ACLs can unintentionally affect return traffic
- ACL rule ordering is critical
- `ping` and `tracert` are useful for isolating routing and filtering issues
- New infrastructure services should always be tested against existing security controls
- Centralized DHCP improves consistency and scalability across VLANs

---

## Module 7 - Secure Management and SSH

### Overview

Implemented encrypted remote CLI management for the router and switches.

### Implemented

- Configured local administrator accounts
- Configured SSH Version 2
- Generated RSA keys
- Configured VTY authentication
- Restricted VTY access to SSH
- Disabled Telnet remote access
- Configured Management VLAN 99
- Successfully connected from PC1 to R1, SW1, and SW2 using SSH

### Management Addresses

| Device | Address |
|---|---|
| R1 | `192.168.99.1` |
| SW1 | `192.168.99.2` |
| SW2 | `192.168.99.3` |

### Troubleshooting

Packet Tracer failed when 2048-bit RSA keys were used.

RSA keys were regenerated using 1024 bits due to simulator limitations, after which SSH connectivity succeeded.

### Lessons Learned

- SSH provides encrypted device administration
- RSA keys are required for SSH operation
- VTY lines control remote CLI access
- Local authentication improves management security
- Telnet transmits management traffic insecurely
- Simulator limitations can differ from physical Cisco hardware
- Troubleshooting should distinguish configuration problems from platform limitations

---

## Module 8 - NAT/PAT and Simulated Internet Connectivity

### Overview

Expanded the topology with an ISP router and external server to simulate Internet connectivity.

### Implemented

- Added ISP-R1
- Added an external server network
- Configured a static default route toward the ISP
- Configured NAT inside and outside interfaces
- Configured a standard ACL for NAT classification
- Configured PAT using NAT overload
- Provided external connectivity to internal VLANs
- Verified NAT translations and statistics
- Confirmed Guest VLAN ACL restrictions remained operational

### External Networks

| Network | Purpose |
|---|---|
| `203.0.113.0/29` | WAN / transit network |
| `198.51.100.0/24` | Simulated external server network |

The WAN originally used `203.0.113.0/30` and was later expanded to `/29` during the OSPF branch expansion.

### Key Commands

```text
ip route 0.0.0.0 0.0.0.0 203.0.113.1

ip nat inside
ip nat outside

ip nat inside source list 1 interface g0/0/1 overload

show ip nat translations
show ip nat statistics
```

### Lessons Learned

- NAT translates addresses between network boundaries
- PAT allows multiple internal hosts to share one outside address
- NAT ACLs identify which source networks should be translated
- Default routing provides a path toward unknown external destinations
- Security policies should be regression-tested after Internet connectivity is introduced

---

## Module 9 - Port Security and Switch Hardening

### Overview

Implemented Layer 2 access-port security and hardened unused switch interfaces.

### Implemented

- Configured Port Security on endpoint-facing ports
- Limited endpoint ports to one secure MAC address
- Enabled sticky MAC learning
- Configured shutdown violation mode
- Simulated an unauthorized-device violation
- Recovered a secure-shutdown access port
- Created VLAN 999 as a black-hole VLAN
- Assigned unused interfaces to VLAN 999
- Administratively shut down unused ports

### Key Commands

```text
switchport port-security
switchport port-security maximum 1
switchport port-security violation shutdown
switchport port-security mac-address sticky

show port-security
show port-security address
show port-security interface fa0/1
```

### Lessons Learned

- Port Security can limit devices allowed on an access port
- Sticky learning dynamically records secure MAC addresses
- Shutdown mode disables a port after a violation
- Secure-shutdown interfaces can be recovered administratively
- Unused ports should be disabled and separated from production VLANs
- Layer 2 security complements Layer 3 ACL controls

---

## Module 10 - STP / RSTP and Layer 2 Redundancy

### Overview

Expanded the switching topology to three switches and implemented redundant Layer 2 paths using Spanning Tree Protocol.

### Implemented

- Added SW3
- Created a redundant triangular switch topology
- Observed automatic STP root bridge election
- Identified Root, Designated, and Alternate port roles
- Analyzed STP path cost
- Configured SW1 as primary root
- Configured SW2 as secondary root
- Tested redundant-link failover
- Migrated to Rapid-PVST+
- Tested faster convergence
- Configured PortFast
- Configured BPDU Guard
- Simulated a rogue-switch BPDU Guard violation
- Verified connectivity after topology changes

### Root Bridge Design

SW1 was configured as the primary root and SW2 as the secondary root for:

- VLAN 10
- VLAN 20
- VLAN 30
- VLAN 99

### Redundancy Testing

A redundant path was deliberately disabled to observe STP reconvergence.

Rapid-PVST+ successfully selected an alternate forwarding path and restored connectivity.

### PortFast and BPDU Guard

PortFast was enabled only on endpoint-facing interfaces.

BPDU Guard was configured to protect those interfaces from unexpected switches.

A temporary rogue switch was connected to a protected access port and BPDU Guard successfully placed the port into a secure shutdown state.

### Lessons Learned

- STP prevents Layer 2 switching loops
- Root bridge election depends on bridge priority and MAC address
- STP chooses paths using cumulative cost
- Redundant links may remain physically up while being logically blocked
- Rapid-PVST+ converges faster than traditional STP
- PortFast accelerates endpoint port forwarding
- BPDU Guard protects endpoint-facing ports from unexpected switching devices
- CLI verification is more reliable than Packet Tracer link colors when analyzing STP state

---

## Module 11 - EtherChannel / LACP

### Overview

Implemented an IEEE LACP EtherChannel between SW1 and SW2 to provide link aggregation and physical-link redundancy.

Two FastEthernet links were combined into logical interface `Port-channel1` and configured as an 802.1Q trunk.

### Physical Design

```text
SW1 Fa0/21 <--> SW2 Fa0/21
SW1 Fa0/22 <--> SW2 Fa0/22
```

Logical interface:

```text
Port-channel1
```

LACP mode:

- SW1 - Active
- SW2 - Active

### VLANs Carried

- VLAN 10 - HR
- VLAN 20 - IT
- VLAN 30 - Guest
- VLAN 99 - Management

VLAN 999 was intentionally excluded because it is used as the black-hole VLAN for unused ports.

### Configuration

#### SW1

```text
interface range FastEthernet0/21 - 22
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,99
 channel-group 1 mode active
 no shutdown

interface Port-channel1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,99
```

#### SW2

```text
interface range FastEthernet0/21 - 22
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,99
 channel-group 1 mode active
 no shutdown

interface Port-channel1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,99
```

### Verification

Verified using:

```text
show etherchannel summary
show interfaces port-channel 1
show interfaces trunk
```

Healthy EtherChannel:

```text
Po1(SU)   LACP   Fa0/21(P) Fa0/22(P)
```

This confirmed:

- Po1 was operating as a Layer 2 EtherChannel
- The Port-channel was in use
- Both member links were successfully bundled using LACP

### Spanning Tree Integration

Rapid-PVST initially preferred the existing GigabitEthernet0/2 link because it had a lower STP cost.

```text
Gi0/2   Root FWD   Cost 4
Po1     Altn BLK   Cost 12
```

After G0/2 was disabled, Rapid-PVST selected the EtherChannel.

```text
Po1     Root FWD   Cost 12
```

This demonstrated that spanning tree treats an EtherChannel as one logical interface.

### Redundancy Test

SW2 Fa0/21 was deliberately shut down.

EtherChannel then showed:

```text
Po1(SU)   LACP   Fa0/21(D) Fa0/22(P)
```

Despite losing one physical member:

- Port-channel1 remained operational
- Rapid-PVST continued using Po1
- PC2 continued communicating with PC1
- End-to-end connectivity was maintained

### Troubleshooting

Packet Tracer initially produced inconsistent Rapid-PVST behavior after the original G0/2 path was disabled.

The EtherChannel configuration was removed, cleaned, and rebuilt.

After rebuilding the LACP bundle:

- Po1 formed successfully
- STP selected Po1 correctly
- End-to-end host connectivity succeeded
- Member-link redundancy operated as expected

### Skills Demonstrated

- EtherChannel configuration
- LACP negotiation
- Layer 2 link aggregation
- 802.1Q trunking
- Rapid-PVST integration
- STP cost analysis
- Physical-link redundancy
- Member-link failure testing
- Cisco IOS troubleshooting

---

## Module 12 - OSPF Dynamic Routing: HQ and Branch Connectivity

### Overview

Expanded the enterprise topology by adding a simulated branch location and implementing single-area OSPFv2 between the HQ and branch routers.

The branch was dynamically integrated with the existing VLAN, ACL, NAT/PAT, and simulated Internet infrastructure.

### Added Devices

- R2 - Branch router
- SW4 - Branch access switch
- Branch-PC
- WAN-SW - Shared Layer 2 transit switch

### Network Design

#### HQ Networks

| Network | Purpose |
|---|---|
| `192.168.10.0/24` | HR |
| `192.168.20.0/24` | IT |
| `192.168.30.0/24` | Guest |
| `192.168.99.0/24` | Management |

#### Branch Network

| Device / Network | Address |
|---|---|
| Branch LAN | `192.168.40.0/24` |
| R2 Branch Gateway | `192.168.40.1` |
| Branch-PC | `192.168.40.10` |

#### WAN / Transit Network

| Device | Address |
|---|---|
| ISP-R1 | `203.0.113.1` |
| R1 | `203.0.113.2` |
| R2 | `203.0.113.3` |

Transit subnet:

```text
203.0.113.0/29
```

### OSPF Design

Configured single-area OSPFv2 using Area 0.

Router IDs:

- R1 - `1.1.1.1`
- R2 - `2.2.2.2`

R1 advertises:

- `192.168.10.0/24`
- `192.168.20.0/24`
- `192.168.30.0/24`
- `192.168.99.0/24`
- `203.0.113.0/29`

R2 advertises:

- `192.168.40.0/24`
- `203.0.113.0/29`

### R1 OSPF Configuration

```text
router ospf 1
 router-id 1.1.1.1
 network 192.168.10.0 0.0.0.255 area 0
 network 192.168.20.0 0.0.0.255 area 0
 network 192.168.30.0 0.0.0.255 area 0
 network 192.168.99.0 0.0.0.255 area 0
 network 203.0.113.0 0.0.0.7 area 0
 default-information originate
```

### R2 OSPF Configuration

```text
router ospf 1
 router-id 2.2.2.2
 network 203.0.113.0 0.0.0.7 area 0
 network 192.168.40.0 0.0.0.255 area 0
```

### OSPF Verification

R1 and R2 successfully formed a FULL OSPF adjacency.

Example:

```text
Neighbor ID     State       Address
2.2.2.2         FULL/BDR    203.0.113.3
```

R2 dynamically learned the HQ networks:

```text
O 192.168.10.0/24 via 203.0.113.2
O 192.168.20.0/24 via 203.0.113.2
O 192.168.30.0/24 via 203.0.113.2
O 192.168.99.0/24 via 203.0.113.2
```

R1 dynamically learned the branch network:

```text
O 192.168.40.0/24 via 203.0.113.3
```

### OSPF Routing Information

Example R2 route:

```text
O 192.168.10.0/24 [110/2] via 203.0.113.2
```

Where:

- `O` = OSPF-learned route
- `110` = OSPF administrative distance
- `2` = OSPF cost
- `203.0.113.2` = next-hop router

### Default Route Advertisement

R1 maintains a static default route toward ISP-R1:

```text
S* 0.0.0.0/0 via 203.0.113.1
```

R1 was configured with:

```text
default-information originate
```

R2 then dynamically learned:

```text
O*E2 0.0.0.0/0 via 203.0.113.2
```

This allows unknown destinations from the branch to be forwarded toward R1.

### Branch NAT/PAT

R2 performs PAT for the branch LAN using its WAN interface.

R2 NAT configuration:

```text
interface g0/0/1
 ip nat inside

interface g0/0/0
 ip nat outside

access-list 1 permit 192.168.40.0 0.0.0.255

ip nat inside source list 1 interface g0/0/0 overload
```

Branch traffic is translated from:

```text
192.168.40.10
```

to:

```text
203.0.113.3
```

This allows the Branch-PC to communicate with the simulated external network.

### Connectivity Verification

Successfully verified:

- R1-to-R2 OSPF adjacency
- Branch-to-HQ communication
- HQ-to-branch routing
- Dynamic HQ route learning on R2
- Dynamic branch route learning on R1
- OSPF default-route propagation
- Branch Internet connectivity
- NAT/PAT operation
- End-to-end routed path using traceroute

Branch-PC successfully reached the external server:

```text
198.51.100.10
```

Traceroute showed:

```text
Branch-PC
    |
    v
R2 - 192.168.40.1
    |
    v
R1 - 203.0.113.2
    |
    v
ISP-R1 - 203.0.113.1
    |
    v
External Server - 198.51.100.10
```

### Routing vs NAT

OSPF and NAT/PAT perform different functions.

OSPF determines:

```text
Where should this packet be sent to reach another network?
```

NAT/PAT determines:

```text
How should private addresses be translated when communicating through an outside network?
```

OSPF provided dynamic HQ-to-branch routing, while PAT allowed the private Branch-PC address to communicate with the simulated external network.

### Layer 2 vs Layer 3 Path Selection

R1, R2, and ISP-R1 share the same Layer 2 WAN switch.

Even though R2 and ISP-R1 are physically connected to the same switch, R2 follows its routing table.

R2 learned:

```text
O*E2 0.0.0.0/0 via 203.0.113.2
```

Therefore, R2 sends unknown destinations to R1.

The WAN switch forwards frames toward the selected next-hop MAC address but does not make Layer 3 routing decisions.

### Centralized vs Local Internet Breakout

The lab currently uses a centralized routing path:

```text
Branch -> R2 -> R1 -> ISP -> External Network
```

A real organization may use centralized Internet routing so branch traffic passes through shared security infrastructure such as:

- Firewalls
- IDS/IPS
- Web filtering
- Logging
- Security monitoring

An alternative design is local Internet breakout:

```text
Branch -> Branch Router -> Local ISP -> Internet
```

Dynamic routing can still be used for corporate HQ-to-branch traffic while Internet traffic follows the local ISP.

### Skills Demonstrated

- OSPFv2 configuration
- Single-area OSPF
- Router ID configuration
- OSPF neighbor adjacency
- Wildcard masks
- Dynamic route propagation
- Routing table interpretation
- Administrative distance
- OSPF cost
- DR/BDR concepts
- Default route advertisement
- OSPF external routes
- HQ/branch network integration
- NAT/PAT integration
- Layer 2 vs Layer 3 path analysis
- Ping and traceroute verification

---

# Current Network Technologies

The project currently incorporates:

- IPv4 addressing and subnetting
- VLAN segmentation
- 802.1Q trunking
- Router-on-a-stick
- Inter-VLAN routing
- Extended ACLs
- DHCP
- Management VLANs
- SSH
- NAT/PAT
- Static default routing
- Port Security
- Black-hole VLANs
- STP
- Rapid-PVST+
- PortFast
- BPDU Guard
- EtherChannel
- LACP
- OSPFv2
- Dynamic route propagation
- OSPF default-route advertisement

## Module 13 - IPv6 and OSPFv3

### Overview

Extended the existing IPv4 enterprise topology into a dual-stack IPv4/IPv6 network. IPv6 was deployed across the HQ VLANs, branch network, WAN, and simulated external network while preserving the existing IPv4 configuration.

Dynamic IPv6 routing was implemented internally using OSPFv3, while static and default IPv6 routing were used at the simulated ISP edge.

### IPv6 Addressing

| Network | IPv6 Prefix | Gateway / Router |
|---|---|---|
| HR - VLAN 10 | `2001:DB8:10::/64` | `2001:DB8:10::1` |
| IT - VLAN 20 | `2001:DB8:20::/64` | `2001:DB8:20::1` |
| Guest - VLAN 30 | `2001:DB8:30::/64` | `2001:DB8:30::1` |
| Management - VLAN 99 | `2001:DB8:99::/64` | `2001:DB8:99::1` |
| Branch LAN | `2001:DB8:40::/64` | `2001:DB8:40::1` |
| WAN Transit | `2001:DB8:113::/64` | ISP-R1 `.1`, R1 `.2`, R2 `.3` |
| External Network | `2001:DB8:198::/64` | ISP-R1 `.1`, Server `.10` |

The `2001:DB8::/32` documentation prefix is used to simulate globally addressed IPv6 networks within Packet Tracer.

### Implemented

- Dual-stack IPv4/IPv6 operation
- IPv6 addressing across existing HQ VLANs
- SLAAC host configuration
- IPv6 link-local default gateways
- IPv6 router-on-a-stick
- IPv6 inter-VLAN routing
- IPv6 addressing across the shared WAN
- IPv6 Branch LAN
- Single-area OSPFv3 between R1 and R2
- Dynamic IPv6 route exchange
- OSPFv3 default route advertisement
- Static and default IPv6 routing toward ISP-R1
- External IPv6 server network
- End-to-end IPv6 connectivity without NAT/PAT
- IPv6 Guest ACL matching the existing IPv4 segmentation policy

### SLAAC and Dual Stack

IPv6 was added alongside the existing IPv4 configuration rather than replacing it.

HQ and Branch clients use SLAAC to automatically configure IPv6 addresses from Router Advertisements. IPv6 hosts use the router's link-local address as their default gateway.

For example, VLAN 10 operates with both protocols:

```text
IPv4 Network:  192.168.10.0/24
IPv4 Gateway:  192.168.10.1

IPv6 Network:  2001:DB8:10::/64
IPv6 Gateway:  2001:DB8:10::1
```

This allows the same VLAN, trunk, and router-on-a-stick infrastructure to carry both IPv4 and IPv6 traffic.

### OSPFv3

R1 and R2 form an OSPFv3 adjacency across the shared WAN.

```text
Neighbor ID     Pri   State       Interface
2.2.2.2           1   FULL/BDR    GigabitEthernet0/0/1
```

R2 dynamically learns the HQ IPv6 prefixes:

```text
O 2001:DB8:10::/64
O 2001:DB8:20::/64
O 2001:DB8:30::/64
O 2001:DB8:99::/64
```

R1 dynamically learns the Branch IPv6 prefix:

```text
O 2001:DB8:40::/64
```

OSPFv3 uses IPv6 link-local addresses as next hops. For example, R2 learned the HQ networks through R1's link-local address on the WAN rather than through R1's global IPv6 address.

### IPv6 Default Routing

R1 uses an IPv6 default route toward ISP-R1:

```text
ipv6 route ::/0 2001:DB8:113::1
```

R1 advertises this default route into OSPFv3:

```text
ipv6 router ospf 1
 default-information originate
```

R2 therefore learns an OSPF external IPv6 default route:

```text
OE2 ::/0 [110/1]
```

This allows the Branch network to forward unknown IPv6 destinations toward R1 while R1 forwards external traffic toward ISP-R1.

### IPv6 External Connectivity

The simulated ISP and external server were extended to IPv6.

```text
                    External Server
                    2001:DB8:198::10
                           |
                         ISP-R1
                    2001:DB8:198::1
                           |
                    2001:DB8:113::1
                           |
                         WAN-SW
                       /        \
                      /          \
       2001:DB8:113::2          2001:DB8:113::3
              R1                       R2
               |                        |
           HQ VLANs               Branch LAN
                                2001:DB8:40::/64
```

ISP-R1 remains outside the internal OSPFv3 domain. Static IPv6 routes provide return paths from ISP-R1 toward the internal networks.

HQ and Branch hosts successfully reached the external server at:

```text
2001:DB8:198::10
```

The Branch-to-external path is:

```text
Branch-PC
    |
   R2
    |
 OSPFv3 Default Route
    |
   R1
    |
 IPv6 Default Route
    |
 ISP-R1
    |
External Server
```

Unlike the IPv4 portion of the lab, IPv6 traffic is routed end-to-end without PAT.

### Dual-Stack Security

Testing revealed an important dual-stack security issue.

The existing IPv4 `GUEST-FILTER` ACL successfully prevented Guest VLAN hosts from initiating traffic toward HR and IT. However, IPv4 ACLs do not filter IPv6 traffic.

After IPv6 was enabled, testing from PC4 in the Guest VLAN showed:

```text
Guest -> HR over IPv4 = BLOCKED
Guest -> HR over IPv6 = ALLOWED
```

This created an unintended IPv6 path around the existing IPv4 segmentation policy.

An IPv6 ACL was therefore created:

```text
ipv6 access-list GUEST-FILTER-V6
 permit icmp 2001:DB8:30::/64 2001:DB8:20::/64 echo-reply
 permit icmp 2001:DB8:30::/64 2001:DB8:10::/64 echo-reply
 deny ipv6 2001:DB8:30::/64 2001:DB8:10::/64
 deny ipv6 2001:DB8:30::/64 2001:DB8:20::/64
 permit ipv6 any any
```

The ACL was applied inbound to the Guest VLAN subinterface:

```text
interface GigabitEthernet0/0/0.30
 ipv6 traffic-filter GUEST-FILTER-V6 in
```

Final Guest policy:

```text
Guest -> HR       = BLOCKED
Guest -> IT       = BLOCKED
HR/IT -> Guest    = ALLOWED
Guest -> External = ALLOWED
```

This demonstrates an important dual-stack security principle: IPv4 security controls do not automatically protect IPv6 traffic, so equivalent policies must be implemented for both protocols.

### Verification

Successfully verified:

- SLAAC IPv6 address assignment
- IPv6 link-local default gateways
- Local IPv6 host-to-gateway connectivity
- IPv6 inter-VLAN routing
- R1-to-R2 IPv6 WAN connectivity
- OSPFv3 `FULL` adjacency
- Dynamic IPv6 route learning
- Branch-to-HQ router connectivity
- Branch-to-HQ host-to-host connectivity
- HQ-to-external IPv6 connectivity
- Branch-to-external IPv6 connectivity
- OSPFv3 IPv6 default route propagation
- Guest-to-HR IPv6 blocking
- Guest-to-IT IPv6 blocking
- Continued permitted Guest external IPv6 connectivity

Key verification commands included:

```text
show ipv6 interface brief
show ipv6 route
show ipv6 route ospf
show ipv6 ospf neighbor
show ipv6 access-list
```

### Lessons Learned

- IPv4 and IPv6 can operate simultaneously using dual stack.
- SLAAC allows hosts to automatically configure IPv6 addresses using Router Advertisements.
- IPv6 hosts commonly use router link-local addresses as their default gateways.
- Link-local addresses are also commonly used as routing-protocol next hops.
- Existing router-on-a-stick infrastructure can support both IPv4 and IPv6.
- IPv6 addressing alone does not provide remote connectivity; routers still require routes to remote IPv6 prefixes.
- OSPFv3 provides dynamic routing for IPv6 networks.
- `::/0` is the IPv6 equivalent of the IPv4 `0.0.0.0/0` default route.
- OSPFv3 can advertise an IPv6 default route to downstream routers.
- Native IPv6 routing does not require the IPv4 PAT design used elsewhere in this lab.
- IPv4 ACLs do not filter IPv6 traffic.
- Dual-stack deployments require equivalent security controls for both IPv4 and IPv6.
- End-to-end host testing is necessary to validate both routing and security policy.

### Skills Demonstrated

- IPv6 addressing and `/64` subnetting
- Dual-stack IPv4/IPv6 networking
- SLAAC and Router Advertisements
- IPv6 link-local addressing
- IPv6 router-on-a-stick
- IPv6 inter-VLAN routing
- IPv6 WAN configuration
- OSPFv3 configuration
- OSPFv3 neighbor verification
- Dynamic IPv6 routing
- IPv6 static routing
- IPv6 default routing
- OSPFv3 default route advertisement
- Native end-to-end IPv6 connectivity
- IPv6 ACL configuration
- Dual-stack security validation
- IPv6 connectivity troubleshooting
## Module 14 - Network Time Protocol (NTP)

### Overview

Expanded the enterprise network with a dedicated infrastructure-services VLAN and centralized Network Time Protocol (NTP) server. Routers and switches were configured to use a common time source to provide consistent timestamps for future logging, monitoring, troubleshooting, and security analysis.

This module also required troubleshooting several supporting network services, including inter-VLAN routing, PAT, OSPF route installation, switch management connectivity, and Packet Tracer NTP behavior.

### Network Services VLAN

A dedicated server network was created to separate infrastructure services from network-device management.

| Component | Configuration |
|---|---|
| VLAN | 50 - SERVERS |
| Network | 192.168.50.0/24 |
| Default Gateway | 192.168.50.1 |
| Infrastructure Server | 192.168.50.10 |
| Server Access Port | SW1 Fa0/3 |
| NTP Transport | UDP/123 |

R1 provides routing for the server VLAN through a new router-on-a-stick subinterface:

```text
interface GigabitEthernet0/0/0.50
 description SERVERS
 encapsulation dot1Q 50
 ip address 192.168.50.1 255.255.255.0
 ip nat inside
```

SW1 was updated to carry VLAN 50 between the switch and R1:

```text
interface GigabitEthernet0/1
 switchport trunk allowed vlan 10,20,30,50,99
```

### Centralized NTP

The infrastructure server at `192.168.50.10` was configured as the centralized NTP source for enterprise network devices.

Client configuration:

```text
ntp server 192.168.50.10
```

Successful synchronization was verified using:

```text
show ntp associations
show ntp status
show clock
```

Healthy clients selected the infrastructure server as their system peer and reported synchronized clocks.

Example:

```text
*~192.168.50.10
Clock is synchronized, stratum 2, reference is 192.168.50.10
```

### OSPF Integration

Because VLAN 50 was created after the original OSPF implementation, R1 was updated to advertise the new server network:

```text
router ospf 1
 network 192.168.50.0 0.0.0.255 area 0
```

R2 subsequently learned the server network dynamically:

```text
Routing entry for 192.168.50.0/24
Known via "ospf 1", distance 110, metric 2, type intra area
Last update from 203.0.113.2 on GigabitEthernet0/0/0
```

Branch-to-server connectivity was verified with a successful ping from R2 to `192.168.50.10`.

### Troubleshooting

Several issues were identified while implementing centralized NTP:

- SW3 initially lacked a management SVI, preventing routed communication with the NTP server. VLAN 99 management addressing was added using `192.168.99.4/24`.
- SW3's simulated clock was significantly different from the NTP server and required an initial manual clock adjustment before synchronization.
- R1's PAT configuration still referenced ACL 1, but the ACL itself was no longer present. The NAT ACL was restored and VLAN 50 was added as an eligible inside network.
- R2 maintained a FULL OSPF adjacency with R1 while failing to install learned IPv4 routes. Resetting the OSPF process forced a route recalculation and restored the expected OSPF routes.
- VLAN 50 was then explicitly added to R1's OSPF configuration so the branch network could reach the infrastructure server.
- Packet Tracer retained/reconstructed a previous R2 NTP association with R1 even though the running configuration specified only the new infrastructure server. The intended configuration and Layer 3 connectivity were independently verified and the simulator behavior was documented as a limitation.

### Verification

The completed implementation verified:

- VLAN 50 connectivity and inter-VLAN routing
- Infrastructure-server external connectivity through PAT
- NTP synchronization on R1 and enterprise switches
- Centralized NTP configuration using `192.168.50.10`
- OSPF advertisement of the new server network
- Branch-to-server routed connectivity
- Consistent enterprise time synchronization for future logging and monitoring services

### Lessons Learned

NTP depends on the underlying network infrastructure and can expose problems that initially appear unrelated to time synchronization. During this module, an NTP failure helped uncover missing management addressing, an OSPF route-installation issue, a missing OSPF network advertisement, and an incomplete PAT configuration.

A FULL OSPF adjacency does not by itself guarantee that expected routes have been installed in the routing table. Protocol state, routing tables, interface status, and end-to-end connectivity should all be verified independently during troubleshooting.

Centralized time synchronization establishes an important foundation for the next network-services stages because Syslog and monitoring data are significantly more useful when all infrastructure devices share a consistent time reference.

### Skills Demonstrated

- Network Time Protocol (NTP)
- Infrastructure server deployment
- Server VLAN design
- Router-on-a-stick
- Inter-VLAN routing
- OSPF route advertisement and troubleshooting
- NAT/PAT troubleshooting
- Switch management SVIs
- Trunk VLAN management
- Layer 3 connectivity testing
- Cisco IOS troubleshooting
- Network-service verification
- Technical documentation
## Module 14 - Network Services & Monitoring

### Overview

This module introduces centralized infrastructure services used to manage, monitor, and troubleshoot network devices.

A dedicated infrastructure server was added to VLAN 50 to provide services such as NTP and Syslog to routers and switches throughout both the headquarters and branch networks.

The infrastructure server uses:

| Device | VLAN | IPv4 Address | Default Gateway |
| --- | --- | --- | --- |
| Infra-Server | 50 - SERVERS | 192.168.50.10/24 | 192.168.50.1 |

The module also expanded branch management capabilities by assigning SW4 a management IP address and correcting a NAT policy issue discovered while testing Branch-to-HQ connectivity.

---

### NTP

#### Overview

Network Time Protocol (NTP) was implemented to provide a centralized time source for infrastructure devices.

Accurate and consistent device clocks are important for troubleshooting, log correlation, security investigations, and determining the sequence of events across multiple network devices.

The Infra-Server at `192.168.50.10` was configured as the network's NTP server.

#### Implemented

The following command was configured on the network infrastructure devices:

```text
ntp server 192.168.50.10
```

NTP was configured on:

- R1
- R2
- SW1
- SW2
- SW3
- SW4

During implementation, SW3 was found to be missing a management SVI. A management address was added in VLAN 99:

```text
interface vlan 99
 description MANAGEMENT
 ip address 192.168.99.4 255.255.255.0
 no shutdown

ip default-gateway 192.168.99.1
```

VLAN 50 was also added to R1's OSPF configuration so the branch network could learn a route to the infrastructure server:

```text
router ospf 1
 network 192.168.50.0 0.0.0.255 area 0
```

R2 subsequently learned `192.168.50.0/24` through OSPF via R1.

#### Verification

NTP configuration and synchronization were examined using:

```text
show ntp associations
show ntp status
show clock
```

R1, SW1, SW2, and SW3 successfully demonstrated centralized NTP operation.

Branch devices successfully obtained routed connectivity to the NTP server, although Packet Tracer exhibited inconsistent NTP association behavior on R2 and SW4.

#### Packet Tracer NTP Limitations

Packet Tracer produced stale or unexpected NTP associations during testing, including associations with `203.0.113.2` that persisted or reappeared despite the intended NTP server being `192.168.50.10`.

Large simulated clock differences also interfered with synchronization. Some devices initially displayed dates decades behind the NTP server and required their clocks to be manually moved closer to the server time.

The intended NTP architecture remained:

```text
R1  ─┐
R2  ─┤
SW1 ─┤
SW2 ─┼──> Infra-Server
SW3 ─┤     192.168.50.10
SW4 ─┘
```

These behaviors were documented as Packet Tracer simulation limitations rather than redesigning a correctly routed network around simulator-specific behavior.

---

### Centralized Syslog

#### Overview

Centralized Syslog was implemented to provide a single location for operational and infrastructure events generated throughout the network.

Instead of relying only on individual device consoles or local log buffers, routers and switches forward messages to the Infra-Server at `192.168.50.10`.

Syslog provides centralized visibility for:

- Interface state changes
- Configuration changes
- Operational events
- Troubleshooting
- Security monitoring
- Incident investigation

The Packet Tracer Syslog service uses UDP port 514.

#### Implemented

Remote logging was configured on the infrastructure devices using:

```text
logging 192.168.50.10
```

Centralized logging was configured on:

- R1
- R2
- SW1
- SW2
- SW3
- SW4

The Syslog server successfully received messages from all six devices.

| Device | Observed Syslog Source |
| --- | --- |
| R1 | 192.168.50.1 |
| R2 | 203.0.113.3 |
| SW1 | 192.168.99.2 |
| SW2 | 192.168.99.3 |
| SW3 | 192.168.99.4 |
| SW4 | 192.168.40.2 |

`show logging` confirmed that devices were forwarding messages to the centralized server using UDP port 514.

Example:

```text
Trap logging: level informational
Logging to 192.168.50.10 (udp port 514)
```

---

### Syslog Event Verification

A controlled interface-state change was generated on SW1 to verify that operational events were actually being delivered to the centralized server.

SW1 Fa0/4 was temporarily configured as an active access port and connected to the lab's disconnected test laptop.

The resulting interface event generated a Syslog message similar to:

```text
%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to up
```

The event appeared on the Infra-Server's centralized Syslog table.

This verified the complete logging path:

```text
Physical Interface Event
          |
          v
    SW1 Detects Change
          |
          v
   IOS Generates Syslog
          |
          v
       UDP 514
          |
          v
     Infra-Server
    192.168.50.10
```

After testing, Fa0/4 was returned to its hardened unused-port configuration:

```text
interface fa0/4
 description UNUSED
 switchport mode access
 switchport access vlan 999
 shutdown
```

---

### Branch Switch Management

SW4 originally operated only as an unmanaged Layer 2 access switch.

To integrate SW4 with centralized infrastructure services, a management SVI was added using the local Branch network.

A dedicated Branch VLAN was created:

```text
vlan 40
 name Branch
```

The R2 and Branch-PC interfaces were assigned to VLAN 40:

```text
interface fa0/1
 description R2-BRANCH-GATEWAY
 switchport mode access
 switchport access vlan 40

interface fa0/2
 description BRANCH-PC
 switchport mode access
 switchport access vlan 40
```

SW4 received the management address `192.168.40.2/24`:

```text
interface vlan 40
 description BRANCH-MANAGEMENT
 ip address 192.168.40.2 255.255.255.0
 no shutdown

ip default-gateway 192.168.40.1
```

This allowed SW4 to communicate with infrastructure services across the routed network without extending the headquarters VLAN 99 management network across the WAN.

Verification showed:

```text
Vlan40    192.168.40.2    up    up
```

---

### Branch NAT Troubleshooting

While integrating SW4 with the centralized NTP and Syslog server, SW4 could successfully reach its local gateway and R2's WAN interface but could not reach the Infra-Server.

The Branch-PC experienced the same failure.

Testing showed:

```text
SW4 -> 192.168.40.1    Success
SW4 -> 203.0.113.3     Success
SW4 -> 192.168.50.10   Failed

Branch-PC -> 192.168.50.10   Failed
```

OSPF routing was verified in both directions.

R2 correctly knew:

```text
192.168.50.0/24 via 203.0.113.2
```

R1 correctly knew:

```text
192.168.40.0/24 via 203.0.113.3
```

Inspection of R2's NAT translation table revealed that Branch-to-HQ traffic was being unnecessarily translated.

A Branch-PC ping to `192.168.50.10` produced translations similar to:

```text
Inside global: 203.0.113.3
Inside local:  192.168.40.10
Outside:       192.168.50.10
```

R2's original NAT ACL matched all traffic sourced from `192.168.40.0/24`, regardless of whether the destination was an internal HQ network or the simulated Internet.

---

### NAT Exemption

R2's NAT policy was modified to exempt internal HQ networks from PAT while continuing to translate Internet-bound Branch traffic.

The original standard NAT ACL was replaced with:

```text
ip access-list extended BRANCH-NAT
 deny ip 192.168.40.0 0.0.0.255 192.168.10.0 0.0.0.255
 deny ip 192.168.40.0 0.0.0.255 192.168.20.0 0.0.0.255
 deny ip 192.168.40.0 0.0.0.255 192.168.30.0 0.0.0.255
 deny ip 192.168.40.0 0.0.0.255 192.168.50.0 0.0.0.255
 deny ip 192.168.40.0 0.0.0.255 192.168.99.0 0.0.0.255
 permit ip 192.168.40.0 0.0.0.255 any
```

PAT was then configured using the new ACL:

```text
ip nat inside source list BRANCH-NAT interface GigabitEthernet0/0/0 overload
```

Because this ACL is being used to classify traffic for NAT, the `deny` statements mean that matching traffic is **not translated**. They do not function as packet-deny rules in this context.

The resulting behavior is:

| Traffic | NAT Behavior |
| --- | --- |
| Branch → VLAN 10 | No NAT |
| Branch → VLAN 20 | No NAT |
| Branch → VLAN 30 | No NAT |
| Branch → VLAN 50 | No NAT |
| Branch → VLAN 99 | No NAT |
| Branch → Simulated Internet | PAT |

After correcting the NAT policy:

```text
Branch-PC -> 192.168.50.10   Success
Branch-PC -> 198.51.100.10   Success
SW4       -> 192.168.50.10   Success
```

This confirmed that Branch-to-HQ traffic remains normally routed through OSPF while Internet-bound Branch traffic continues to use PAT.

---

### Verification

Centralized network services were verified using:

```text
show ntp associations
show ntp status
show clock
show logging
show ip interface brief
show vlan brief
show ip route
show ip nat translations
show ip nat statistics
show access-lists
```

The final network successfully demonstrated:

- Centralized NTP configuration
- Centralized Syslog collection
- Syslog forwarding from six infrastructure devices
- Operational event logging
- Branch switch management
- Routed Branch-to-HQ infrastructure access
- OSPF reachability between Branch and HQ
- NAT exemption for internal traffic
- PAT for simulated Internet traffic

---

### Lessons Learned

- NTP provides a consistent time source across network infrastructure and improves event correlation and troubleshooting.
- Syslog centralizes operational and security-related events generated by routers and switches.
- Cisco Syslog severity levels range from 0 through 7, with lower numbers representing more severe events.
- Syslog commonly uses UDP port 514.
- Controlled interface-state changes provide a simple method for verifying end-to-end centralized logging.
- Layer 2 switches require a management SVI and default gateway to communicate with management services outside their local subnet.
- A branch switch can use a local management SVI without extending an HQ management VLAN across a WAN.
- NAT policies must distinguish between internal routed traffic and traffic that actually requires address translation.
- ACL `deny` statements used for NAT classification can exempt traffic from translation without blocking the underlying traffic.
- Troubleshooting should verify local connectivity, routing, return routing, NAT, ACLs, and finally the application or service.
- Packet Tracer can demonstrate NTP and Syslog concepts but contains simulator-specific limitations that may not reflect production Cisco IOS behavior.

### Skills Demonstrated

- Network Time Protocol (NTP)
- Centralized Syslog
- Cisco IOS logging configuration
- Syslog severity interpretation
- UDP-based network services
- Network event monitoring
- Management SVI configuration
- Branch VLAN implementation
- Layer 2 switch management
- OSPF route verification
- Branch-to-HQ routing
- NAT/PAT troubleshooting
- NAT exemption
- Extended ACL configuration
- Infrastructure service integration
- Structured network troubleshooting
## Module 14 - Network Services & Monitoring

### Overview

This module introduces centralized infrastructure services used to manage, monitor, and troubleshoot network devices across the headquarters and branch networks.

A dedicated infrastructure server was added to VLAN 50 to provide centralized network services.

| Device | VLAN | IPv4 Address | Default Gateway |
| --- | --- | --- | --- |
| Infra-Server | 50 - SERVERS | 192.168.50.10/24 | 192.168.50.1 |

The module implements:

- Network Time Protocol (NTP)
- Centralized Syslog
- Simple Network Management Protocol (SNMP)
- Branch switch management
- Multi-site monitoring
- NAT exemption for routed HQ-to-Branch communication

The module also demonstrates how centralized services depend on correct routing and NAT policy across a multi-site network.

---

### NTP

#### Overview

Network Time Protocol (NTP) was implemented to provide a centralized time source for network infrastructure.

Accurate and consistent device clocks improve troubleshooting, monitoring, log correlation, and security investigations by allowing events from multiple devices to be placed on a common timeline.

The Infra-Server at `192.168.50.10` acts as the centralized NTP server.

#### Implemented

The infrastructure devices were configured with:

```text
ntp server 192.168.50.10
```

NTP was configured on:

- R1
- R2
- SW1
- SW2
- SW3
- SW4

During implementation, SW3 was found to be missing a management SVI.

A management address was added in VLAN 99:

```text
interface vlan 99
 description MANAGEMENT
 ip address 192.168.99.4 255.255.255.0
 no shutdown

ip default-gateway 192.168.99.1
```

VLAN 50 was also added to R1's OSPF configuration so the Branch network could learn a route to the infrastructure server:

```text
router ospf 1
 network 192.168.50.0 0.0.0.255 area 0
```

R2 subsequently learned `192.168.50.0/24` through OSPF.

#### Verification

NTP was examined using:

```text
show ntp associations
show ntp status
show clock
```

Centralized NTP operation was successfully demonstrated across the network.

SW4 initially experienced synchronization problems. Later troubleshooting revealed that R1 was incorrectly applying PAT to NTP traffic traveling from the Infra-Server toward the Branch network.

After correcting the HQ NAT policy, SW4 reported:

```text
Clock is synchronized, stratum 2, reference is 192.168.50.10
```

and `192.168.50.10` appeared as the selected NTP peer.

This confirmed successful centralized NTP operation across the routed HQ and Branch networks.

Packet Tracer continued to display an additional stale association with `203.0.113.2` on SW4, but the correct Infra-Server was selected as the active NTP reference.

---

### Centralized Syslog

#### Overview

Centralized Syslog was implemented to provide a single location for operational and infrastructure events generated throughout the network.

Instead of relying only on individual device consoles or local log buffers, routers and switches forward messages to the Infra-Server at `192.168.50.10`.

Syslog provides centralized visibility for:

- Interface state changes
- Configuration changes
- Operational events
- Troubleshooting
- Security monitoring
- Incident investigation

Packet Tracer uses UDP port 514 for Syslog.

#### Implemented

Remote logging was configured using:

```text
logging 192.168.50.10
```

Centralized logging was configured on:

- R1
- R2
- SW1
- SW2
- SW3
- SW4

The Syslog server successfully received messages from all six devices.

| Device | Observed Syslog Source |
| --- | --- |
| R1 | 192.168.50.1 |
| R2 | 203.0.113.3 |
| SW1 | 192.168.99.2 |
| SW2 | 192.168.99.3 |
| SW3 | 192.168.99.4 |
| SW4 | 192.168.40.2 |

`show logging` confirmed that devices were forwarding messages to the centralized server using UDP port 514.

Example:

```text
Trap logging: level informational
Logging to 192.168.50.10 (udp port 514)
```

#### Syslog Event Verification

A controlled interface-state change was generated on SW1 to verify end-to-end logging.

SW1 Fa0/4 was temporarily configured as an active access port and connected to the lab's disconnected test laptop.

The resulting interface event generated a Syslog message similar to:

```text
%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to up
```

The event appeared on the Infra-Server's centralized Syslog table.

The test demonstrated:

```text
Physical Interface Event
          |
          v
    SW1 Detects Change
          |
          v
   IOS Generates Syslog
          |
          v
       UDP 514
          |
          v
     Infra-Server
    192.168.50.10
```

After testing, Fa0/4 was returned to its hardened unused-port configuration:

```text
interface fa0/4
 description UNUSED
 switchport mode access
 switchport access vlan 999
 shutdown
```

---

### Branch Switch Management

SW4 originally operated only as an unmanaged Layer 2 access switch.

To integrate SW4 with centralized infrastructure services, a management SVI was added using the local Branch network.

A dedicated Branch VLAN was created:

```text
vlan 40
 name Branch
```

The R2 and Branch-PC interfaces were assigned to VLAN 40:

```text
interface fa0/1
 description R2-BRANCH-GATEWAY
 switchport mode access
 switchport access vlan 40

interface fa0/2
 description BRANCH-PC
 switchport mode access
 switchport access vlan 40
```

SW4 received the management address `192.168.40.2/24`:

```text
interface vlan 40
 description BRANCH-MANAGEMENT
 ip address 192.168.40.2 255.255.255.0
 no shutdown

ip default-gateway 192.168.40.1
```

Verification showed:

```text
Vlan40    192.168.40.2    up    up
```

This allows SW4 to communicate with centralized infrastructure services through the routed Branch network without extending the headquarters VLAN 99 management network across the WAN.

---

### Branch-to-HQ NAT Exemption

While integrating SW4 with centralized services, SW4 could successfully reach its local gateway and R2's WAN interface but initially could not reach the Infra-Server.

The Branch-PC experienced the same failure.

Testing showed:

```text
SW4 -> 192.168.40.1    Success
SW4 -> 203.0.113.3     Success
SW4 -> 192.168.50.10   Failed

Branch-PC -> 192.168.50.10   Failed
```

OSPF routing was verified in both directions.

R2 correctly knew:

```text
192.168.50.0/24 via 203.0.113.2
```

R1 correctly knew:

```text
192.168.40.0/24 via 203.0.113.3
```

Inspection of R2's NAT translation table revealed that Branch-to-HQ traffic was being unnecessarily translated.

R2's original NAT policy matched all traffic sourced from `192.168.40.0/24` regardless of destination.

The standard NAT ACL was replaced with a destination-aware extended ACL:

```text
ip access-list extended BRANCH-NAT
 deny ip 192.168.40.0 0.0.0.255 192.168.10.0 0.0.0.255
 deny ip 192.168.40.0 0.0.0.255 192.168.20.0 0.0.0.255
 deny ip 192.168.40.0 0.0.0.255 192.168.30.0 0.0.0.255
 deny ip 192.168.40.0 0.0.0.255 192.168.50.0 0.0.0.255
 deny ip 192.168.40.0 0.0.0.255 192.168.99.0 0.0.0.255
 permit ip 192.168.40.0 0.0.0.255 any
```

PAT was then configured using:

```text
ip nat inside source list BRANCH-NAT interface GigabitEthernet0/0/0 overload
```

The ACL `deny` entries exempt internal HQ destinations from NAT. They do not block the underlying routed traffic.

After the change:

```text
Branch-PC -> 192.168.50.10   Success
Branch-PC -> 198.51.100.10   Success
SW4       -> 192.168.50.10   Success
```

This confirmed that Branch-to-HQ traffic remains normally routed while Internet-bound Branch traffic continues to use PAT.

---

### SNMP

#### Overview

Simple Network Management Protocol (SNMP) was implemented to demonstrate centralized infrastructure monitoring.

SNMP allows a Network Management System (NMS) to retrieve operational information from routers and switches without administrators manually logging into each device.

Typical SNMP monitoring can include:

- Device identity
- Device uptime
- Interface inventory
- Administrative interface state
- Operational interface state
- Interface counters
- Device availability
- Performance metrics

The Infra-Server's MIB Browser was used as the lab's SNMP manager.

Network devices operate as SNMP agents.

The monitoring architecture is:

```text
Infra-Server / NMS
192.168.50.10
        |
        | SNMP GET
        | UDP 161
        v
Routers and Switches
     SNMP Agents
```

#### Implemented

A read-only SNMP community was configured on all six infrastructure devices:

```text
snmp-server community DOM-NMS-RO ro
```

SNMP was configured on:

- R1
- R2
- SW1
- SW2
- SW3
- SW4

Read-only access was selected because the objective is monitoring rather than remote modification.

The Packet Tracer implementation exposed only simplified community-string-based SNMP configuration. Production environments should restrict SNMP access to authorized management systems and generally prefer SNMPv3 when supported.

---

### MIB and OID Queries

The Packet Tracer MIB Browser was used to perform SNMP GET operations.

A Management Information Base (MIB) organizes information exposed by an SNMP-enabled device.

An Object Identifier (OID) identifies a specific value within that hierarchy.

The first R1 query requested:

```text
sysName
```

OID:

```text
.1.3.6.1.2.1.1.5.0
```

The query returned:

```text
R1
```

This demonstrated successful SNMP communication between the Infra-Server and R1.

The NMS then queried:

```text
sysUpTime
```

OID:

```text
.1.3.6.1.2.1.1.3.0
```

The response reported approximately:

```text
20 hours 11 minutes 51 seconds
```

This demonstrated how an NMS can remotely determine device uptime and potentially identify unexpected device restarts.

---

### SNMP Interface Monitoring

R1's interface MIB was queried to examine interface information.

Relevant objects included:

```text
ifDescr
ifAdminStatus
ifOperStatus
```

`ifDescr` mapped SNMP interface indexes to recognizable IOS interfaces.

Example results included:

| Index | Interface |
| ---: | --- |
| 1 | GigabitEthernet0/0/0 |
| 2 | GigabitEthernet0/0/1 |
| 3 | GigabitEthernet0/0/2 |
| 4 | GigabitEthernet0/0/0.10 |
| 5 | GigabitEthernet0/0/0.20 |
| 6 | GigabitEthernet0/0/0.30 |

`ifAdminStatus` represents the administratively configured interface state.

`ifOperStatus` represents the actual operational interface state.

For R1 interface index 3:

```text
Interface:     GigabitEthernet0/0/2
Admin Status:  down
Oper Status:   down
```

This allows an NMS to distinguish an administratively disabled interface from an enabled interface that unexpectedly lost connectivity.

For example:

```text
Admin: up
Oper:  up
```

indicates an enabled and functioning interface.

```text
Admin: up
Oper:  down
```

can indicate an unexpected link failure.

```text
Admin: down
Oper:  down
```

indicates an administratively disabled interface.

This demonstrates how a monitoring platform can determine interface state without an administrator manually running:

```text
show ip interface brief
```

---

### SNMP Multi-Site Verification

SNMP polling was verified against SW1 using:

```text
Target: 192.168.99.2
Object: sysName
OID:    .1.3.6.1.2.1.1.5.0
```

The query successfully returned:

```text
SW1
```

The Infra-Server then attempted the same query against SW4:

```text
192.168.40.2
```

The initial query failed.

Testing showed that the Infra-Server could not ping either SW4 or the Branch-PC, proving that the problem was network reachability rather than SNMP configuration.

---

### HQ-to-Branch NAT Exemption

R1's NAT translation table was inspected while the Infra-Server attempted to communicate with the Branch.

The table showed that traffic from:

```text
192.168.50.10
```

was being translated to:

```text
203.0.113.2
```

even when its destination was the internal Branch network.

The NAT table also showed UDP port 123 traffic from the Infra-Server toward SW4 being translated, revealing that the same NAT policy was interfering with NTP communication.

R1's original standard NAT ACL was therefore replaced with a destination-aware extended ACL:

```text
ip access-list extended HQ-NAT
 deny ip 192.168.10.0 0.0.0.255 192.168.40.0 0.0.0.255
 deny ip 192.168.20.0 0.0.0.255 192.168.40.0 0.0.0.255
 deny ip 192.168.30.0 0.0.0.255 192.168.40.0 0.0.0.255
 deny ip 192.168.50.0 0.0.0.255 192.168.40.0 0.0.0.255
 deny ip 192.168.99.0 0.0.0.255 192.168.40.0 0.0.0.255
 permit ip 192.168.10.0 0.0.0.255 any
 permit ip 192.168.20.0 0.0.0.255 any
 permit ip 192.168.30.0 0.0.0.255 any
 permit ip 192.168.50.0 0.0.0.255 any
 permit ip 192.168.99.0 0.0.0.255 any
```

PAT was then configured using:

```text
ip nat inside source list HQ-NAT interface GigabitEthernet0/0/1 overload
```

The final NAT design is:

| Traffic | Behavior |
| --- | --- |
| HQ → Branch | Routed without NAT |
| Branch → HQ | Routed without NAT |
| HQ → Simulated Internet | PAT |
| Branch → Simulated Internet | PAT |

After correcting R1's NAT policy:

```text
Infra-Server -> Branch-PC       Success
Infra-Server -> SW4             Success
Infra-Server -> External Server Success
```

SNMP polling of SW4 then succeeded across the routed HQ-to-Branch network.

The NAT correction also allowed SW4 to successfully synchronize with the centralized NTP server.

---

### Final Monitoring Architecture

The completed centralized services architecture includes:

```text
                    Infra-Server
                   192.168.50.10
                  /       |       \
                 /        |        \
              NTP       Syslog     SNMP
            UDP 123     UDP 514   UDP 161
               |           |         |
               +-----------+---------+
                           |
                Network Infrastructure
                           |
        +------+------+----+----+------+------+
        |      |      |         |      |      |
       R1     R2     SW1       SW2    SW3    SW4
```

NTP provides centralized time synchronization.

Syslog provides centralized event collection.

SNMP provides centralized device and interface monitoring.

Together, these services provide a basic model of the management and monitoring functions used in production network environments.

---

### Verification

Network services and monitoring were verified using:

```text
show ntp associations
show ntp status
show clock
show logging
show running-config | include snmp
show ip interface brief
show vlan brief
show ip route
show ip nat translations
show ip nat statistics
show access-lists
```

The Packet Tracer MIB Browser was also used to perform SNMP GET operations for:

```text
sysName
sysUpTime
ifDescr
ifAdminStatus
ifOperStatus
```

The final network successfully demonstrated:

- Centralized NTP
- Centralized Syslog
- Read-only SNMP monitoring
- SNMP MIB/OID queries
- Device uptime monitoring
- Interface state monitoring
- Multi-site network monitoring
- Branch switch management
- Routed HQ-to-Branch infrastructure access
- OSPF reachability between sites
- Bidirectional NAT exemption for internal traffic
- PAT for simulated Internet traffic

---

### Lessons Learned

- NTP provides a consistent time source across network infrastructure and improves event correlation and troubleshooting.
- Syslog centralizes operational and security-related events generated by routers and switches.
- SNMP allows an NMS to retrieve operational information without administrators manually logging into each device.
- SNMP polling commonly uses UDP port 161, while Syslog commonly uses UDP port 514.
- MIBs organize management information while OIDs identify individual values within the MIB hierarchy.
- SNMP `sysName` can remotely identify a device.
- SNMP `sysUpTime` can help identify unexpected device restarts.
- `ifDescr`, `ifAdminStatus`, and `ifOperStatus` allow an NMS to identify interfaces and distinguish administrative shutdowns from operational failures.
- Read-only SNMP follows least-privilege principles better than read-write access when only monitoring is required.
- Community-string-based SNMP provides weaker security than SNMPv3 and should be carefully restricted in production environments.
- Layer 2 switches require a management SVI and default gateway to communicate with management systems outside their local subnet.
- A branch switch can use a local management SVI without extending an HQ management VLAN across a WAN.
- NAT policies must distinguish internal routed traffic from traffic that actually requires address translation.
- A NAT ACL `deny` can exempt traffic from translation without blocking the underlying routed traffic.
- NAT translation tables are valuable troubleshooting tools and can reveal unexpected behavior affecting ICMP and application protocols such as NTP and SNMP.
- Adding a second routed site can expose assumptions in NAT policies originally designed for a single-site Internet edge.
- Troubleshooting application failures should begin by verifying network reachability before changing application-layer configuration.
- NTP, Syslog, and SNMP complement one another by providing synchronized time, centralized events, and ongoing network-state monitoring.

### Skills Demonstrated

- Network Time Protocol (NTP)
- Centralized Syslog
- Simple Network Management Protocol (SNMP)
- SNMP manager/agent architecture
- SNMP GET operations
- MIB navigation
- OID interpretation
- Device uptime monitoring
- Interface monitoring
- `ifIndex` correlation
- Administrative vs. operational interface status
- Read-only SNMP configuration
- Cisco IOS logging configuration
- Syslog severity interpretation
- Management SVI configuration
- Branch VLAN implementation
- Multi-site infrastructure monitoring
- OSPF route verification
- NAT/PAT troubleshooting
- Bidirectional NAT exemption
- Extended ACL configuration
- Centralized infrastructure services
- Structured network troubleshooting