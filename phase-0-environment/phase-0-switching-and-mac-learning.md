# Phase 0 — Switching and MAC Address Learning

## 1. Lab Overview

This exercise demonstrates how an Ethernet switch learns device MAC addresses, builds its MAC address table, forwards Ethernet frames, and removes inactive dynamic entries.

* **Software:** Cisco Packet Tracer 9.0.1
* **Topology:** Two PCs connected to a Cisco 2960 switch.
* **Switch:** Switch0 — Cisco 2960
* **Lab status:** Completed — MAC learning and connectivity verified.

> **Safety note:** The topology is isolated inside Cisco Packet Tracer and does not modify the physical home network.

---

## 2. Network Topology

### Devices
* PC0
* PC1
* Switch0 — Cisco 2960

### Connections

| Device | Interface | Switch | Interface |
| :--- | :--- | :--- | :--- |
| **PC0** | FastEthernet0 | Switch0 | `Fa0/1` |
| **PC1** | FastEthernet0 | Switch0 | `Fa0/2` |

* **Cable type:** Copper Straight-Through.
* Both links showed green status indicators.

---

## 3. IPv4 Configuration

### PC0
* **IPv4 address:** `192.168.1.10`
* **Subnet mask:** `255.255.255.0`
* **Default gateway:** Blank
* **DNS server:** Blank

### PC1
* **IPv4 address:** `192.168.1.11`
* **Subnet mask:** `255.255.255.0`
* **Default gateway:** Blank
* **DNS server:** Blank

Both PCs belong to the same IPv4 subnet:
```text
192.168.1.0/24
```

A default gateway is unnecessary for communication between these two PCs because they are on the same subnet. DNS is also unnecessary when communicating directly using IP addresses.

---

## 4. Connectivity Verification

### Test 1 — PC0 to PC1
* **Command:** `ping 192.168.1.11`
* **Result:**
  * Packets sent: 4
  * Packets received: 4
  * Packets lost: 0
  * Packet loss: 0%
  * Average response time: 4 ms

### Test 2 — PC1 to PC0
* **Command:** `ping 192.168.1.10`
* **Result:**
  * Packets sent: 4
  * Packets received: 4
  * Packets lost: 0
  * Packet loss: 0%
  * Average response time: 0 ms

### Interpretation
* Both PCs successfully communicated in both directions.
* Ping uses ICMP Echo Request and Echo Reply messages to test IP connectivity.
* Round-trip time (RTT) measures the time taken for a request to reach its destination and for the reply to return.
* Packet loss indicates packets sent without receiving a corresponding reply.

---

## 5. Understanding MAC Addresses

A Media Access Control (MAC) address is a Layer 2 address used for communication over Ethernet networks.

### Important points:
* A MAC address identifies a network interface at the data-link layer.
* An IPv4 address identifies a device's logical location within an IP network.
* Ethernet switches use MAC addresses to decide where to forward Ethernet frames.
* A MAC address is different from an IPv4 address.

---

## 6. How a Switch Learns MAC Addresses

A switch learns MAC addresses automatically by examining the source MAC address of incoming Ethernet frames.

It records:
* The VLAN.
* The MAC address.
* The switch port where the frame arrived.
* Whether the entry is dynamic or static.

### Command used
```text
show mac-address-table
```

### Observed output
```text
Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    0001.43ac.2881    DYNAMIC     Fa0/1
   1    00d0.ba68.15b5    DYNAMIC     Fa0/2
```

### Interpretation
* PC0's MAC address was learned on `Fa0/1`.
* PC1's MAC address was learned on `Fa0/2`.
* Both entries belonged to VLAN 1.
* **DYNAMIC** means the switch learned the entries automatically.

### Important concept
The switch learns from the *source* MAC address, but uses the *destination* MAC address to decide where to forward a frame.

---

## 7. Unknown Unicast Flooding

When a switch receives a frame whose destination MAC address is not in its MAC address table, it performs unknown unicast flooding.

The switch forwards the frame through eligible ports in the same VLAN, except the port on which the frame arrived.

This allows the frame to reach its intended destination even when the switch has not yet learned that destination's MAC address.

---

## 8. MAC Address Relearning

Dynamic MAC entries can be removed from a switch's table. If a device sends another Ethernet frame, the switch can learn its source MAC address again.

For example:
1. PC0 sends a frame through `Fa0/1`.
2. Switch0 learns PC0's source MAC address.
3. PC1 sends a frame through `Fa0/2`.
4. Switch0 learns PC1's source MAC address.

If a device moves to another switch port and sends traffic, the switch can update the MAC-to-port association. This helps the switch maintain an accurate record of where network devices are connected.

---

## 9. MAC Address Aging

MAC address aging is the process by which a switch removes inactive dynamic MAC address entries after a configured period.

### Why is aging useful?
* It prevents old entries from remaining indefinitely.
* It helps the switch adapt when devices move between ports.
* It helps keep the MAC address table current.

### Practical observation
At one point, the MAC address table was empty when the following command was executed:
```text
show mac-address-table
```

We then generated traffic from PC0 to PC1:
```text
ping 192.168.1.11
```

The result was:
* 4 packets sent.
* 4 packets received.
* 0% packet loss.
* Average round-trip time of 0 ms.

After the ping, we inspected the switch again. Both dynamic MAC entries had returned:

| VLAN | MAC address | Type | Port |
| :--- | :--- | :--- | :--- |
| 1 | `0001.43ac.2881` | DYNAMIC | `Fa0/1` |
| 1 | `00d0.ba68.15b5` | DYNAMIC | `Fa0/2` |

### Important limitation
We attempted to display the aging timer using:
```text
show mac address-table aging-time
```

Packet Tracer returned:
```text
% Invalid input detected at '^' marker.
```

This indicates that the simulated switch image did not accept that command. We therefore did not verify the exact aging timer or measure the precise time taken for entries to expire. Our practical observations demonstrate an empty table followed by successful MAC relearning, but do not independently establish why or when the earlier entries disappeared.

---

## 10. Important Commands

| Command | Purpose |
| :--- | :--- |
| `show mac-address-table` | Displays the MAC address table |
| `show mac address-table` | Alternative syntax attempted during the lab; command support can vary |
| `ping 192.168.1.11` | Tests connectivity from PC0 to PC1 |
| `ping 192.168.1.10` | Tests connectivity from PC1 to PC0 |

> **Note:** The MAC address table command shown in the observed successful output was `show mac-address-table`. Command availability and syntax may vary between switch models and simulated IOS versions.

---

## 11. Knowledge Checks

The following questions were answered correctly during the lesson.

* **Question 1:** What does a switch use to forward Ethernet frames?
  * **Answer:** The destination MAC address.
* **Question 2:** What happens when a switch receives a frame with an unknown unicast destination?
  * **Answer:** It floods the frame through eligible ports in the same VLAN, except the incoming port.
* **Question 3:** What can happen when a device moves to another switch port and sends traffic?
  * **Answer:** The switch can relearn the MAC address on the new port.
* **Question 4:** Why did the MAC addresses appear again after the ping?
  * **Answer:** The switch learned the source MAC addresses from incoming Ethernet frames.

**Score:** 4/4 on these recorded questions.

---

## 12. Lessons Learned

By completing this exercise, I practised:
* Building a basic switched LAN.
* Connecting PCs to a Cisco 2960 switch.
* Configuring static IPv4 addresses.
* Testing connectivity with ping.
* Understanding Ethernet MAC addresses.
* Inspecting the MAC address table.
* Understanding dynamic MAC learning.
* Understanding unknown unicast flooding.
* Understanding MAC address relearning.
* Learning the purpose of MAC address aging.
* Recognising limitations in simulated switch command support.

---

## 13. Safety and Documentation Notes

* This topology was created in Cisco Packet Tracer.
* It is a simulated environment.
* No changes were made to the physical router.
* No Windows network configuration was modified.
* No existing virtual machines were started or modified.
* Before publishing this project on GitHub, review screenshots and documentation for private information.

---

## 14. Phase 0 Progress

* [x] Inspect the host computer.
* [x] Inspect basic Windows network configuration.
* [x] Test local gateway connectivity.
* [x] Test external IP connectivity.
* [x] Test DNS resolution.
* [x] Install and launch Cisco Packet Tracer.
* [x] Build a two-PC switched LAN.
* [x] Configure static IPv4 addresses.
* [x] Verify connectivity in both directions.
* [x] Inspect the switch MAC address table.
* [x] Understand dynamic MAC learning.
* [x] Understand unknown unicast flooding.
* [x] Understand MAC address relearning.
* [x] Study MAC address aging and its limitations.
* [x] Complete the Phase 0 practical assessment.
* [ ] Complete the remaining documentation review.
* [ ] Prepare Phase 0 portfolio materials.

---

**Document:** Phase 0 — Switching and MAC Address Learning
**Track:** Networking Engineering
**Status:** Completed learning milestone; portfolio review in progress.
