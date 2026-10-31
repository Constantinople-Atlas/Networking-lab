# Phase 0 — First LAN in Cisco Packet Tracer

## Lab Status

* **Status:** Completed — connectivity verified
* **Simulator:** Cisco Packet Tracer 9.0.1
* **Project file:** `phase-0-first-lan.pkt`
* **Topology:** Three PCs connected to one Cisco 2960 switch

---

## 1. Objective

Build a small Ethernet LAN containing three computers connected through a network switch, configure static IPv4 addresses, and test communication between the computers.

---

## 2. Network Topology

The final topology contains:
* PC0
* PC1
* PC2
* Switch0 (Cisco 2960)

### Physical connections

| Device | Network interface | Switch interface | Cable type |
| :--- | :--- | :--- | :--- |
| **PC0** | FastEthernet0 | `Fa0/1` | Copper Straight-Through |
| **PC1** | FastEthernet0 | `Fa0/2` | Copper Straight-Through |
| **PC2** | FastEthernet0 | `Fa0/3` | Copper Straight-Through |

All three computers were connected to the 2960 switch using Copper Straight-Through cables. All connection indicators were green, showing active links in the simulation.

### Logical topology

```text
PC0 ---------------- Fa0/1
                       |
PC1 ---------------- Fa0/2
                    Switch0
PC2 ---------------- Fa0/3
                    (2960)
```

---

## 3. IPv4 Configuration

The three computers were configured with static IPv4 addresses.

| Setting | PC0 | PC1 | PC2 |
| :--- | :--- | :--- | :--- |
| **IPv4 address** | `192.168.1.10` | `192.168.1.11` | `192.168.1.12` |
| **Subnet mask** | `255.255.255.0` | `255.255.255.0` | `255.255.255.0` |
| **Default gateway** | Blank | Blank | Blank |
| **DNS server** | Blank | Blank | Blank |

All three computers belong to the same IPv4 subnet:
```text
192.168.1.0/24
```
They have unique IP addresses within that subnet.

### Why was a default gateway unnecessary?
The computers were communicating within their local subnet. A default gateway is normally needed when a device must send traffic to destinations outside its local IP network.

### Why was DNS unnecessary?
The connectivity tests used destination IP addresses directly rather than domain names. Therefore, DNS resolution was not required for these tests.

---

## 4. Connectivity Tests

### Test 1 — PC0 to PC1
* **Command:** `ping 192.168.1.11`
* **Results:**
  * Sent: 4
  * Received: 4
  * Lost: 0
  * Packet loss: 0%
  * Average response time: 4 ms
* **Conclusion:** PC0 successfully communicated with PC1.

### Test 2 — PC1 to PC0
* **Command:** `ping 192.168.1.10`
* **Results:**
  * Sent: 4
  * Received: 4
  * Lost: 0
  * Packet loss: 0%
  * Average response time: 0 ms
* **Conclusion:** PC1 successfully communicated with PC0.

### Test 3 — PC2 to PC1
* **Command:** `ping 192.168.1.11`
* **Results:**
  * Sent: 4
  * Received: 4
  * Lost: 0
  * Packet loss: 0%
* **Conclusion:** PC2 successfully communicated with PC1.

### Test 4 — PC0 to PC2
* **Command:** `ping 192.168.1.12`
* **Results:**
  * Sent: 4
  * Received: 4
  * Lost: 0
  * Packet loss: 0%
* **Conclusion:** PC0 successfully communicated with PC2.

### Overall connectivity result
All recorded connectivity tests were successful. The three computers successfully communicated within the simulated LAN.

---

## 5. Concepts Learned

* **Ethernet Connections:** Ethernet cables provide wired network connections between computers and switches.
* **Static IPv4 Addressing:** Static addressing allows network administrators to manually assign IP addresses to devices.
* **Subnet Masks:** A subnet mask identifies the network and host portions of an IPv4 address.
* **Unique IP Addresses:** Each computer on the same IP subnet must have a unique IP address to avoid address conflicts.
* **Network Switches:** A Layer 2 switch connects devices within a LAN and forwards Ethernet frames using destination MAC addresses.
* **Ping:** Ping uses ICMP Echo Request and Echo Reply messages to test connectivity between devices.
* **Packet Loss and Response Time:**
  * Packet loss measures how many transmitted packets did not receive responses.
  * Response time measures how long it takes for a response to return.

---

## 6. Limitations

This exercise tested basic LAN connectivity only.

### It did not configure:
* A router.
* DHCP services.
* DNS services.
* VLANs.
* Internet access.
* Inter-subnet routing.

The topology was simulated in Cisco Packet Tracer and did not modify the physical home network.

---

## 7. Project Files

The following files belong to this milestone:
* `phase-0-first-lan.pkt`
* `phase-0-first-lan-report.md`

The Packet Tracer project contains the simulated three-PC LAN.

---

## 8. Progress Checklist

* [x] Three PCs added.
* [x] Cisco 2960 switch added.
* [x] Ethernet cables connected.
* [x] PC0 configured with a static IPv4 address.
* [x] PC1 configured with a static IPv4 address.
* [x] PC2 configured with a static IPv4 address.
* [x] PC0 successfully pinged PC1.
* [x] PC1 successfully pinged PC0.
* [x] PC2 successfully pinged PC1.
* [x] PC0 successfully pinged PC2.
* [x] Final Packet Tracer project saved.
* [x] Written report saved.
* [ ] Final documentation reviewed and sanitized for GitHub publication.

---

## 9. Safety and Change-Control Notes

* No physical home network settings were changed.
* No router configuration was modified.
* No external network was accessed through this simulation.
* All practical exercises were performed inside Cisco Packet Tracer.

---

**Document:** Phase 0 — First LAN in Cisco Packet Tracer
**Track:** Networking Engineering
**Status:** Completed learning milestone; portfolio review in progress.
