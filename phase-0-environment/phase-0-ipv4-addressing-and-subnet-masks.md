# Phase 0 — IPv4 Addressing and Subnet Masks

## Learning Milestone

* **Status:** Theory and Packet Tracer practical completed
* **Lab file:** `phase-0-subnetting-practical.pkt`
* **Suggested folder:** `Documents\Networking-Lab\phase-0-environment\`
* **Track:** Networking Engineering

This document records the IPv4 addressing, binary conversion, subnet-mask, subnetting, and Packet Tracer work completed during this milestone.

---

## 1. IPv4 Addressing Fundamentals

An IPv4 address is a 32-bit logical address, normally written as four decimal octets separated by dots.

**Example:**
```text
192.168.1.10
```

Each octet contains 8 bits. An octet can represent values from 0 through 255.

An IP address has two important parts:
* **Network portion:** Identifies the IP network.
* **Host portion:** Identifies an interface or device within that network.

A subnet mask identifies which bits belong to the network portion and which belong to the host portion.

---

## 2. Binary and Decimal Conversion

The eight bit positions in one octet have these values:

| Bit position | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Value** | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |

To convert binary to decimal, add the values of the positions containing "1".

### Examples

| Binary octet | Calculation | Decimal |
| :--- | :--- | :--- |
| `10000000` | 128 | 128 |
| `11000000` | 128 + 64 | 192 |
| `11100000` | 128 + 64 + 32 | 224 |
| `11110000` | 128 + 64 + 32 + 16 | 240 |
| `11111000` | 128 + 64 + 32 + 16 + 8 | 248 |
| `11111100` | 128 + 64 + 32 + 16 + 8 + 4 | 252 |
| `11111110` | 128 + 64 + 32 + 16 + 8 + 4 + 2 | 254 |
| `11111111` | 128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 | 255 |
| `10101000` | 128 + 32 + 8 | 168 |

The reverse process converts decimal values into their binary representation.

---

## 3. CIDR Prefixes and Subnet Masks

CIDR notation uses a slash followed by the number of network bits.

For example, `/24` means that the first 24 of the 32 bits are network bits. The remaining 8 bits are host bits.

### Common subnet prefixes

| Prefix | Subnet mask | Host bits | Total addresses | Conventional usable hosts |
| :--- | :--- | :--- | :--- | :--- |
| `/24` | `255.255.255.0` | 8 | 256 | 254 |
| `/25` | `255.255.255.128` | 7 | 128 | 126 |
| `/26` | `255.255.255.192` | 6 | 64 | 62 |
| `/27` | `255.255.255.224` | 5 | 32 | 30 |
| `/28` | `255.255.255.240` | 4 | 16 | 14 |
| `/29` | `255.255.255.248` | 3 | 8 | 6 |

For these conventional IPv4 subnets, the network address and broadcast address are not assigned to ordinary hosts.

### Useful formulas
* **Host bits** = 32 − prefix length
* **Total addresses** = $2^{\text{host bits}}$
* **Conventional usable hosts** = total addresses − 2

> **Important:** These conventional usable-host calculations apply to ordinary IPv4 subnets, not every special-purpose prefix or point-to-point addressing arrangement.

---

## 4. The /24 Example

For:
```text
192.168.1.10/24
```

### The subnet details are:

| Property | Value |
| :--- | :--- |
| **Subnet mask** | `255.255.255.0` |
| **Network address** | `192.168.1.0` |
| **First usable address** | `192.168.1.1` |
| **Last usable address** | `192.168.1.254` |
| **Broadcast address** | `192.168.1.255` |
| **Host bits** | 8 |
| **Total addresses** | 256 |
| **Conventional usable host addresses** | 254 |

### Original Packet Tracer LAN
The original Packet Tracer LAN used:

| Device | IPv4 address | Subnet mask |
| :--- | :--- | :--- |
| **PC0** | `192.168.1.10` | `255.255.255.0` |
| **PC1** | `192.168.1.11` | `255.255.255.0` |
| **PC2** | `192.168.1.12` | `255.255.255.0` |

All three addresses belong to:
```text
192.168.1.0/24
```

---

## 5. Subnetting a /24 into /25 Networks

A `/25` has 25 network bits and 7 host bits.

| Property | Value |
| :--- | :--- |
| **Subnet mask** | `255.255.255.128` |
| **Binary mask** | `11111111.11111111.11111111.10000000` |
| **Total addresses per subnet** | 128 |
| **Conventional usable hosts per subnet** | 126 |

The two `/25` networks inside `192.168.1.0/24` are:

| Subnet | Network address | Usable host range | Broadcast |
| :--- | :--- | :--- | :--- |
| **First** | `192.168.1.0/25` | `192.168.1.1`–`192.168.1.126` | `192.168.1.127` |
| **Second** | `192.168.1.128/25` | `192.168.1.129`–`192.168.1.254` | `192.168.1.255` |

### Example
`192.168.1.130/25` belongs to the second subnet:
* **Network:** `192.168.1.128/25`
* **First usable host:** `192.168.1.129`
* **Broadcast:** `192.168.1.255`

---

## 6. Subnetting a /24 into /26 Networks

A `/26` has 26 network bits and 6 host bits.

| Property | Value |
| :--- | :--- |
| **Subnet mask** | `255.255.255.192` |
| **Binary mask** | `11111111.11111111.11111111.11000000` |
| **Total addresses per subnet** | 64 |
| **Conventional usable hosts per subnet** | 62 |

### Calculating the block size
The block size is calculated using:
$$\text{256} - \text{interesting octet}$$

For a `/26`, the interesting octet is 192:
$$\text{256} - \text{192} = 64$$

Therefore, the subnet boundaries in the final octet are `.0`, `.64`, `.128`, and `.192`.

### /26 subnet table

| Subnet | Network address | Usable host range | Broadcast |
| :--- | :--- | :--- | :--- |
| **1** | `192.168.1.0/26` | `.1`–`.62` | `.63` |
| **2** | `192.168.1.64/26` | `.65`–`.126` | `.127` |
| **3** | `192.168.1.128/26` | `.129`–`.190` | `.191` |
| **4** | `192.168.1.192/26` | `.193`–`.254` | `.255` |

### Examples practised
* `192.168.1.150/26` belongs to `192.168.1.128/26`; broadcast is `192.168.1.191`.
* `192.168.1.200/26` belongs to `192.168.1.192/26`.
* `192.168.1.200/26` and `192.168.1.220/26` are in the same subnet.
* `192.168.1.50/26` and `192.168.1.90/26` are in different subnets.

---

## 7. Subnetting a /24 into /27 Networks

A `/27` has 27 network bits and 5 host bits.

| Property | Value |
| :--- | :--- |
| **Subnet mask** | `255.255.255.224` |
| **Block size** | $256 - 224 = 32$ |
| **Total addresses** | 32 |
| **Conventional usable hosts** | 30 |

The subnet boundaries in the final octet are `.0`, `.32`, `.64`, `.96`, `.128`, `.160`, `.192`, `.224`.

### /27 examples

| Address | Network | Usable range | Broadcast |
| :--- | :--- | :--- | :--- |
| `192.168.1.75/27` | `192.168.1.64/27` | `.65`–`.94` | `.95` |
| `192.168.1.150/27` | `192.168.1.128/27` | `.129`–`.158` | `.159` |
| `192.168.1.200/27` | `192.168.1.192/27` | `.193`–`.222` | `.223` |

### Choosing a subnet size
* A requirement for at least 50 conventional host addresses fits a `/26`, which provides 62 usable addresses.
* A requirement for 25 conventional host addresses fits a `/27`, which provides 30 usable addresses.

The selected prefix must provide enough usable host addresses for the devices that need to use the subnet.

---

## 8. Practical Packet Tracer Lab

### Purpose
Use one Cisco 2960 Layer 2 switch to demonstrate:
1. Devices in the same IP subnet can communicate through the switch without a router.
2. Devices in different IP subnets need a Layer 3 routing device to communicate.
3. A Layer 2 switch does not route IP packets between different IP networks.

### Lab file and topology
A separate copy was saved as:
```text
Documents\Networking-Lab\phase-0-environment\phase-0-subnetting-practical.pkt
```

The topology contains:
* PC0
* PC1
* PC2
* Switch0 — Cisco 2960
* Copper Straight-Through Ethernet connections from each PC to the switch

The original `phase-0-first-lan.pkt` was kept separate and untouched.

### Addressing plan used
Subnet mask for all three PCs:
```text
255.255.255.192 (/26)
```

| Device | IPv4 address | Subnet | Default gateway |
| :--- | :--- | :--- | :--- |
| **PC0** | `192.168.1.10` | `192.168.1.0/26` | Blank |
| **PC1** | `192.168.1.70` | `192.168.1.64/26` | Blank |
| **PC2** | `192.168.1.71` | `192.168.1.64/26` | Blank |

### Subnet details

#### PC0 subnet — 192.168.1.0/26
* **Network:** `192.168.1.0`
* **Usable range:** `.1–.62`
* **Broadcast:** `.63`

#### PC1 and PC2 subnet — 192.168.1.64/26
* **Network:** `192.168.1.64`
* **Usable range:** `.65–.126`
* **Broadcast:** `.127`

---

### Test A — Same-subnet communication
From PC1:
```text
ping 192.168.1.71
```

**Observed result:**
* Packets sent: 4
* Packets received: 4
* Packets lost: 0
* Packet loss: 0%
* Minimum: 0 ms
* Maximum: 0 ms
* Average: 0 ms

**Conclusion:** PC1 and PC2 communicated successfully. Both belong to `192.168.1.64/26`, so they can exchange traffic through the Layer 2 switch without a router.

---

### Test B — Different-subnet communication
The first attempt contained a typing error:
```text
192.169.1.70
```
That attempt timed out because the destination address was incorrect. The destination was corrected before drawing a conclusion.

From PC0, the correct command was:
```text
ping 192.168.1.70
```

**Observed result:**
* Packets sent: 4
* Packets received: 0
* Packets lost: 4
* Packet loss: 100%

**Conclusion:** PC0 could not reach PC1 in this lab.
* PC0 belongs to `192.168.1.0/26`.
* PC1 belongs to `192.168.1.64/26`.
* No default gateway was configured.
* No router was present.

PC0 recognises that the destination is outside its local subnet. It needs a default gateway to forward the packet towards a router. The Cisco 2960 Layer 2 switch can forward Ethernet frames, but it does not perform IP routing between these subnets.

### Important topology limitation
All three PCs remained connected to the same physical switch and the same default VLAN (VLAN 1). This exercise demonstrates different IP subnets on a shared Layer 2 segment. It does not demonstrate VLAN-based isolation.

In a properly segmented network, separate subnets are commonly associated with separate VLANs and routed through a router or Layer 3 switch. No router or VLAN configuration was added as part of this exercise.

---

## 9. Key Concepts to Remember

* An IPv4 address is 32 bits, written as four decimal octets.
* A subnet mask separates network bits from host bits.
* The CIDR prefix tells how many bits are network bits.
* More network bits leave fewer host bits and therefore fewer host addresses per subnet.
* The block-size method helps identify subnet boundaries in the interesting octet.
* Network and broadcast addresses are not assigned to ordinary hosts in conventional IPv4 subnets.
* Hosts in the same IP subnet can communicate directly at Layer 2 when the local network is functioning.
* Hosts in different IP subnets need a router or another Layer 3 device to communicate.
* A normal Layer 2 switch forwards frames using MAC addresses; it does not route between IP subnets.
* Always check the destination IP carefully before diagnosing a failed ping.

---

## 10. Knowledge-Check Outcomes

The learner correctly answered the key checks covering:
* `/24`, `/25`, `/26`, and `/27` network and host bits.
* Binary-to-decimal conversion exercises.
* Network, usable host, and broadcast address identification.
* Identifying whether two IP addresses belong to the same subnet.
* Selecting `/26` for at least 50 hosts and `/27` for 25 hosts.
* Why two same-subnet PCs communicated while PCs in different subnets could not communicate without a router.

**Correction during practice:** A Layer 2 switch does not automatically route traffic between different IP subnets.

---

## 11. Milestone Status

### Completed
* [x] IPv4 address structure
* [x] Binary conversion practice
* [x] Subnet masks
* [x] CIDR prefixes
* [x] `/24`, `/25`, `/26`, and `/27` calculations
* [x] Subnet boundaries
* [x] Usable host ranges
* [x] Broadcast addresses
* [x] Packet Tracer same-subnet demonstration
* [x] Packet Tracer different-subnet demonstration
* [x] Practical knowledge checks

### Not covered by this practical
* VLAN configuration
* Inter-VLAN routing
* Router configuration
* VLSM design
* A full subnetting assessment beyond the exercises completed

*(These topics belong in later lessons.)*

---

## 12. Portfolio and Privacy Reminder

Before publishing this work to GitHub:
* Include the `.pkt` file and this Markdown report if desired.
* Review screenshots and outputs for personal information.
* Do not publish home-network credentials, private keys, physical MAC addresses, or other secrets.
* Keep the lab addressing plan clearly identified as a simulated Packet Tracer network.
* Review all project files before making the repository public.

---

**Networking Engineering Learning Portfolio — Phase 0**
**Milestone:** IPv4 Addressing and Subnet Masks
**Status:** Completed learning milestone; portfolio review in progress.
