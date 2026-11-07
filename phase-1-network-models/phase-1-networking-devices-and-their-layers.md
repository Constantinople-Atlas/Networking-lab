# Phase 1 — Networking Devices and Their Layers

## 1. Introduction

Computer networks use different devices to connect computers, forward data, and control communication.

Each device has a particular role. Understanding these roles helps network engineers design networks, identify traffic paths, and troubleshoot connectivity problems.

The OSI model provides a useful framework for understanding where networking devices commonly operate.

This document covers:
- Hubs and repeaters.
- Switches.
- Routers.
- Wireless access points.
- Firewalls.
- How networking devices work together.
- How to identify possible device-related problems.

---

## 2. The Physical Layer — Layer 1

The Physical layer is responsible for transmitting raw bits through a communication medium.

It deals with the physical characteristics of network communication, including:
- Electrical signals.
- Optical signals.
- Radio signals.
- Cables and connectors.
- Signal transmission and reception.

### 2.1 Hub

A hub is a basic networking device that operates at the Physical layer.

Its main responsibility is to repeat incoming signals to its other ports.

For example, if a hub receives a signal through one port, it repeats that signal through the remaining ports.

Important characteristics:
- Does not examine MAC addresses.
- Does not make intelligent forwarding decisions.
- Repeats signals to connected devices.
- All connected devices share the same collision domain.

Hubs are largely obsolete in modern Ethernet networks because switches provide more efficient communication.

### 2.2 Repeater

A repeater operates at Layer 1.

Its purpose is to regenerate or repeat signals so they can travel farther.

Important characteristics:
- Works with signals rather than IP addresses.
- Does not examine MAC addresses.
- Does not route packets.
- Helps extend the physical transmission distance within supported limits.

### 2.3 Physical Layer Troubleshooting

When investigating a possible Layer 1 problem, check:
1. Is the cable connected?
2. Is the cable damaged?
3. Are the device interfaces powered on?
4. Are the correct cable types and supported media being used?
5. Are interface link indicators showing a connection?

A device cannot communicate successfully over a physical link that is disconnected or faulty.

---

## 3. The Data Link Layer — Layer 2

The Data Link layer handles communication across a local network link.

Its responsibilities include:
- Ethernet framing.
- MAC addressing.
- Local frame delivery.
- Media access.
- Error detection at the frame level.

### 3.1 Network Switch

A typical Layer 2 Ethernet switch operates at the Data Link layer.

It forwards Ethernet frames using MAC address information.

#### How a switch learns MAC addresses
A switch learns by examining the source MAC address of incoming frames.

It records the relationship between a MAC address and the port on which that address was learned.

| MAC Address | Switch Port |
| :--- | :--- |
| `00AA.1111.2222` | `Fa0/1` |
| `00BB.3333.4444` | `Fa0/2` |
| `00CC.5555.6666` | `Fa0/3` |

This information is stored in the MAC address table.

#### How a switch forwards frames
When a switch receives a frame:
1. It examines the source MAC address and learns or updates its location.
2. It examines the destination MAC address.
3. If the destination is known, it forwards the frame through the appropriate port in the relevant VLAN.
4. If the destination is an unknown unicast address, it generally floods the frame through the other eligible ports in that VLAN, excluding the incoming port.
5. Broadcast frames are forwarded within the relevant VLAN, subject to normal forwarding rules.

#### Important switch characteristics
- Uses MAC addresses for Layer 2 forwarding.
- Connects devices within a local network.
- Separates collision domains per switch port in normal full-duplex Ethernet.
- A basic Layer 2 switch does not route traffic between IP networks.
- VLANs create separate Layer 2 broadcast domains.

### 3.2 VLANs

A Virtual Local Area Network (VLAN) divides a switched network into separate logical Layer 2 broadcast domains.

| VLAN | Connected Devices |
| :--- | :--- |
| **VLAN 10** | `PC0` |
| **VLAN 20** | `PC1` and `PC2` |

`PC0` belongs to a different broadcast domain from `PC1` and `PC2`.

Devices in different VLANs generally need Layer 3 routing to communicate.

### 3.3 Access Ports

An access port is normally assigned to one VLAN and connects an end device to a switch.

```text
interface fastEthernet 0/1
 switchport mode access
 switchport access vlan 10
```

This configuration assigns FastEthernet0/1 to VLAN 10.

### 3.4 Trunk Ports

A trunk port carries traffic for multiple VLANs across one connection.

IEEE 802.1Q is commonly used to identify VLANs on Ethernet trunk links by adding a VLAN tag to frames.

```text
interface gigabitEthernet 0/1
 switchport mode trunk
```

A trunk is useful when connecting switches or connecting a VLAN-aware switch to a router.

### 3.5 Data Link Layer Troubleshooting

When investigating a Layer 2 problem, check:
1. Is the device connected to the correct switch port?
2. Is the port assigned to the intended VLAN?
3. Is the MAC address table learning devices correctly?
4. Is the trunk operational?
5. Are the required VLANs allowed and active on the trunk?

Useful Cisco switch commands include:
```text
show mac address-table
show vlan brief
show interfaces status
show interfaces trunk
```

---

## 4. The Network Layer — Layer 3

The Network layer handles logical addressing and routing between networks.

Its responsibilities include:
- IP addressing.
- Packet forwarding.
- Routing.
- Selecting paths toward destination networks.

### 4.1 Router

A router is a Layer 3 networking device.

It connects different IP networks and forwards packets based on IP addressing and routing information.

For example:
- Network A: `192.168.1.0/24`
- Network B: `192.168.2.0/24`

A router can forward traffic between these networks when appropriate interfaces and routes are configured.

#### How a router forwards packets
A simplified process:
1. The router receives a frame.
2. It processes the Layer 2 header and obtains the IP packet.
3. It examines the destination IP address.
4. It checks its routing information.
5. It selects an outgoing interface or next hop.
6. It creates a new Layer 2 frame appropriate for the outgoing link.
7. It forwards the packet.

The Layer 2 frame is replaced for the next link. The IP source and destination addresses normally remain unchanged unless a feature such as Network Address Translation modifies them.

The IPv4 TTL is reduced by at least one at each router hop.

### 4.2 Default Gateway

A default gateway is the router address a host uses to reach destinations outside its local IP subnet.

| Device | IP Address | Subnet Mask | Gateway |
| :--- | :--- | :--- | :--- |
| `PC0` | `192.168.1.10` | `255.255.255.0` | `192.168.1.1` |

If `PC0` wants to communicate with a destination outside `192.168.1.0/24`, it sends the traffic toward `192.168.1.1`.

A wrong default gateway can prevent communication with remote networks even when local communication works.

### 4.3 Router-on-a-Stick

Router-on-a-Stick allows one physical router interface to route traffic between multiple VLANs using logical subinterfaces.

| Router Subinterface | VLAN | IP Address |
| :--- | :--- | :--- |
| `G0/0.10` | 10 | `192.168.1.1/26` |
| `G0/0.20` | 20 | `192.168.1.65/26` |

Each subinterface is associated with a VLAN tag.

Example configuration:
```text
interface gigabitEthernet 0/0.10
 encapsulation dot1Q 10
 ip address 192.168.1.1 255.255.255.192

interface gigabitEthernet 0/0.20
 encapsulation dot1Q 20
 ip address 192.168.1.65 255.255.255.192
```

The switch-to-router link must be configured as a trunk so it can carry the VLAN traffic.

### 4.4 Network Layer Troubleshooting

Check:
1. Does the host have the correct IP address?
2. Is the subnet mask correct?
3. Is the default gateway correct?
4. Are the router interfaces up?
5. Are the router subinterfaces configured with the correct VLAN tags?
6. Does the routing table contain a route to the destination?

Useful router commands include:
```text
show ip interface brief
show ip route
show running-config
```

---

## 5. Wireless Access Points

A wireless access point (AP) allows wireless devices to connect to a network.

In a common bridged wireless LAN design, an access point operates primarily at Layer 2.

Its responsibilities can include:
- Providing wireless connectivity.
- Connecting wireless clients to a wired LAN.
- Bridging traffic between wireless and wired network segments.
- Managing wireless association and authentication.

An access point is not necessarily a router. Some home devices combine an access point, router, switch, and firewall in one physical unit.

---

## 6. Firewalls

A firewall enforces security policies by allowing or blocking network traffic.

Its operation depends on the type and implementation.

Examples include:
- Stateless packet-filtering firewalls.
- Stateful inspection firewalls.
- Application-aware firewalls.
- Next-generation firewalls.

A firewall may examine information from several network layers, including IP addresses, transport protocols, port numbers, connection state, and application data.

Therefore, it is not accurate to classify every firewall as operating exclusively at one OSI layer.

---

## 7. Comparison of Common Networking Devices

| Device | Common OSI Association | Main Function |
| :--- | :--- | :--- |
| **Hub** | Layer 1 | Repeats signals |
| **Repeater** | Layer 1 | Regenerates signals |
| **Layer 2 switch** | Layer 2 | Forwards frames using MAC addresses |
| **Router** | Layer 3 | Forwards packets between IP networks |
| **Wireless access point** | Primarily Layer 2 in a bridged WLAN | Connects wireless clients to a LAN |
| **Firewall** | Depends on implementation | Enforces traffic security policies |

> **Important Note:** The OSI layer association is a learning aid. Modern devices can perform functions across multiple layers. For example, a multilayer switch can perform both Layer 2 switching and Layer 3 routing.

---

## 8. How Networking Devices Work Together

Consider a simple network:
1. A PC sends data through an Ethernet cable.
2. A switch receives the Ethernet frame.
3. The switch examines the destination MAC address.
4. If the destination is on another IP network, the PC sends the frame to its default gateway.
5. The router receives the frame and examines the IP packet.
6. The router selects a route toward the destination.
7. The router creates a new frame for the outgoing link.
8. The next network device receives and forwards the traffic.

Each device performs a different part of the communication process.

---

## 9. Applying the Concepts to the Phase 1 Lab

The practical Packet Tracer lab used:
- Three PCs.
- A Cisco Catalyst 2960 switch.
- A Cisco 2911 router.
- VLAN 10 and VLAN 20.
- An 802.1Q trunk.
- Router-on-a-Stick inter-VLAN routing.

### Device Roles in the Lab
* **PCs:** Generated and received network traffic.
* **Cisco Catalyst 2960 switch:** Forwarded Ethernet frames within VLANs using MAC address information.
* **Cisco 2911 router:** Routed IP packets between VLAN 10 and VLAN 20 using subinterfaces.
* **Trunk link:** Carried traffic for multiple VLANs between the switch and router.

### Observed Verification
The switch trunk verification showed:
- GigabitEthernet0/1 operating as a trunk.
- 802.1Q encapsulation.
- VLANs 1, 10, and 20 active and forwarding.

The router interface verification showed:
- GigabitEthernet0/0 up/up.
- GigabitEthernet0/0.10 up/up.
- GigabitEthernet0/0.20 up/up.

Connectivity tests demonstrated successful communication between devices in the same VLAN and between devices in different VLANs.

---

## 10. Troubleshooting by Layer

| Layer | Possible Problem | Example Check |
| :--- | :--- | :--- |
| **Physical** | Disconnected or faulty cable | Check cable and interface link status |
| **Data Link** | Incorrect VLAN assignment | `show vlan brief` |
| **Data Link** | Trunk not operational | `show interfaces trunk` |
| **Network** | Incorrect IP address or mask | Check host IP configuration |
| **Network** | Incorrect default gateway | Check host gateway configuration |
| **Network** | Router interface down | `show ip interface brief` |
| **Network** | Missing route | `show ip route` |
| **Transport** | Service or port unavailable | Check service and firewall rules |
| **Application** | Application or DNS issue | Test the relevant service and name resolution |

A good troubleshooting process begins with the symptoms and tests the relevant layers systematically. Avoid changing several settings at once because that makes it harder to identify the actual cause.

---

## 11. Knowledge Review

1. Which layer is associated with a traditional Ethernet hub?
2. Which address type does a typical Layer 2 switch use when forwarding frames?
3. What is the primary responsibility of a router?
4. What is the purpose of a default gateway?
5. What is the difference between an access port and a trunk port?
6. What does a router do to the Layer 2 frame when forwarding a packet over another Ethernet link?
7. Why can a firewall not always be assigned to only one OSI layer?
8. What is the purpose of a wireless access point?

### Answer Key
1. Layer 1 — Physical.
2. MAC addresses.
3. Forwarding IP packets between networks.
4. Providing a next hop for destinations outside the local IP subnet.
5. An access port normally carries one assigned VLAN for an end device; a trunk carries traffic for multiple VLANs.
6. It removes the incoming Layer 2 frame and creates a new frame appropriate for the outgoing link.
7. Modern firewalls can inspect and enforce policies using information from multiple layers.
8. Connecting wireless clients to a network, commonly by bridging wireless traffic to a wired LAN.

---

## Conclusion

Networking devices perform complementary functions.

Hubs and repeaters work with physical signals. Switches forward local Ethernet frames. Routers forward IP packets between networks. Wireless access points connect wireless clients to a LAN, while firewalls enforce security policies.

Understanding these responsibilities makes it easier to design networks, interpret traffic flow, and troubleshoot connectivity problems.

* **Phase:** 1 — Network Models and Encapsulation
* **Document:** Networking Devices and Their Layers
* **Status:** Completed
