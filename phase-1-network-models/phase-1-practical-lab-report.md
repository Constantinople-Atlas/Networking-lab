# Phase 1 --- Practical Network Models Lab Report

## 1. Lab Overview

**Project:** Networking Engineering\
**Phase:** 1 --- Network Models and Encapsulation\
**Lab:** VLAN Configuration and Inter-VLAN Routing\
**Simulation Software:** Cisco Packet Tracer

### Objective

Build and test a small network containing multiple VLANs and IP subnets.
Configure a switch trunk and router subinterfaces to enable
communication between VLANs.

## 2. Network Topology

The lab contained:

-   Three PCs: PC0, PC1, and PC2.
-   One Cisco Catalyst 2960 switch.
-   One Cisco 2911 router.
-   A trunk connection between the switch and router.

### Switch Port Assignments

  Switch Port   Connected Device   Configuration
  ------------- ------------------ ----------------
  Fa0/1         PC0                Access VLAN 10
  Fa0/2         PC1                Access VLAN 20
  Fa0/3         PC2                Access VLAN 20
  Gi0/1         Router G0/0        802.1Q trunk

## 3. IP Addressing Plan

The lab used two IPv4 subnets.

  Device   IP Address     Subnet Mask       Default Gateway
  -------- -------------- ----------------- -----------------
  PC0      192.168.1.10   255.255.255.192   192.168.1.1
  PC1      192.168.1.70   255.255.255.192   192.168.1.65
  PC2      192.168.1.71   255.255.255.192   192.168.1.65

### Subnet Details

**VLAN 10**

-   Network: `192.168.1.0/26`
-   Usable addresses: `192.168.1.1–192.168.1.62`
-   Broadcast: `192.168.1.63`
-   Router gateway: `192.168.1.1`

**VLAN 20**

-   Network: `192.168.1.64/26`
-   Usable addresses: `192.168.1.65–192.168.1.126`
-   Broadcast: `192.168.1.127`
-   Router gateway: `192.168.1.65`

## 4. Switch Configuration

Two VLANs were created:

  VLAN ID   VLAN Name        Assigned Ports
  --------- ---------------- ----------------
  10        vlan10-PC0       Fa0/1
  20        vlan20-PC1-PC2   Fa0/2--Fa0/3

The switch's GigabitEthernet0/1 interface was configured as a trunk
using IEEE 802.1Q.

### Actual Verification Evidence

The `show interfaces trunk` command produced the following relevant
output:

``` text
Port        Mode         Encapsulation  Status        Native vlan
Gig0/1      on           802.1q         trunking      1

Port        Vlans allowed on trunk
Gig0/1      1-1005

Port        Vlans allowed and active in management domain
Gig0/1      1,10,20

Port        Vlans in spanning tree forwarding state and not pruned
Gig0/1      1,10,20
```

**Interpretation:** The switch-to-router connection was operating as an
802.1Q trunk. VLANs 10 and 20 were active and forwarding across the
trunk.

## 5. Router-on-a-Stick Configuration

The router's physical interface GigabitEthernet0/0 was used to create
two logical subinterfaces.

### Subinterface Configuration

  Subinterface   VLAN   IP Address     Subnet Mask
  -------------- ------ -------------- -----------------
  G0/0.10        10     192.168.1.1    255.255.255.192
  G0/0.20        20     192.168.1.65   255.255.255.192

The subinterfaces used these encapsulation settings:

``` text
encapsulation dot1Q 10
encapsulation dot1Q 20
```

### Actual Verification Evidence

The `show ip interface brief` command produced:

``` text
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     unassigned      YES unset  up                    up
GigabitEthernet0/0.10  192.168.1.1     YES manual up                    up
GigabitEthernet0/0.20  192.168.1.65    YES manual up                    up
GigabitEthernet0/1     unassigned      YES unset  administratively down down
GigabitEthernet0/2     unassigned      YES unset  administratively down down
Vlan1                  unassigned      YES unset  administratively down down
```

**Interpretation:** Both configured router subinterfaces were up/up and
had the intended gateway addresses. The unused physical interfaces being
administratively down did not affect this lab.

## 6. Connectivity Tests

The following ping tests were performed:

  Source   Destination   Result
  -------- ------------- --------------------------
  PC0      PC1           3 of 4 replies; 25% loss
  PC1      PC0           4 of 4 replies; 0% loss
  PC2      PC0           4 of 4 replies; 0% loss
  PC1      PC2           4 of 4 replies; 0% loss

### Interpretation

-   PC0 successfully communicated with a device in VLAN 20 after the
    first ping attempt.
-   PC1 and PC2 successfully communicated with PC0 across VLANs.
-   PC1 and PC2 communicated within VLAN 20.
-   The first PC0-to-PC1 test lost its first packet, while the following
    three succeeded. This can be consistent with initial address
    resolution, such as ARP, but the exact cause was not independently
    established.

The successful tests demonstrated that inter-VLAN routing and same-VLAN
communication were functioning.

## 7. Important Networking Concepts Demonstrated

### VLANs

VLANs divide a switched network into separate Layer 2 broadcast domains.

### Access Ports

Access ports carry traffic for their assigned VLAN and connect end
devices to the switch.

### Trunk Ports

Trunk ports carry traffic from multiple VLANs over one link using VLAN
tags.

### IEEE 802.1Q

802.1Q identifies VLAN membership by adding a VLAN tag to Ethernet
frames on trunk links.

### Router-on-a-Stick

Router-on-a-Stick uses multiple logical router subinterfaces over one
physical interface to route traffic between VLANs.

### Default Gateways

A host sends traffic destined for another IP subnet to its configured
default gateway.

## 8. Lessons Learned

This practical exercise demonstrated that:

1.  VLAN membership determines the Layer 2 broadcast domain of a switch
    port.
2.  Different VLANs normally require Layer 3 routing to communicate.
3.  A trunk connection can transport multiple VLANs between network
    devices.
4.  Router subinterfaces must use the correct VLAN IDs.
5.  IP addresses, subnet masks, and default gateways must be consistent
    with the network design.
6.  Verification commands and connectivity tests are essential when
    troubleshooting.

## 9. Conclusion

The Phase 1 practical lab successfully demonstrated VLAN segmentation,
802.1Q trunking, subnet addressing, and inter-VLAN routing using
Router-on-a-Stick.

The successful connectivity tests confirmed that devices in separate
VLANs could communicate through the router while devices in the same
VLAN could communicate through the switch.

**Lab Status:** Completed.
