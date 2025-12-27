Question 1️⃣

👉 What happens internally on a switch when you connect a new device to an access port?
(Explain step-by-step: link up → MAC learning → traffic flow)

Ans:
When a new device is connected to a switch access port, the following happens internally:

* Physical link comes up – the switch and device negotiate speed and duplex.
* The port is already configured as an access port, so frames are placed into a specific VLAN.
* When the device sends its first frame (usually ARP), the switch learns the source MAC address and stores it in the CAM table, mapped to that port and VLAN.
* If the destination MAC is unknown, the switch floods the frame to all ports in the same VLAN except the source port.
* Once the destination device replies, its MAC is learned, and from then on, the switch forwards traffic based on the destination MAC address as a unicast.
== This is how normal Layer-2 MAC learning and forwarding occurs on a switch.

Question 2️⃣

👉 What is the difference between an access port and a trunk port?
Where do you use each in a real network?

Ans:
An access port is configured to carry traffic for only one VLAN.
Frames are sent and received without VLAN tags, and it is typically used to connect end devices like servers, PCs, or printers.

A trunk port is used to carry traffic for multiple VLANs over a single link.
It uses 802.1Q tagging to identify VLANs and is commonly used between switches, switch to router, or switch to firewall connections.

In trunk ports, one VLAN can be configured as a native VLAN, whose traffic is sent untagged.

Question 3️⃣

👉 What is STP, and what problem does it solve in a Layer-2 network?
(Explain the problem first, then the solution)

Ans:
STP stands for Spanning Tree Protocol.
In a Layer-2 network, loops can occur when there are redundant links, which can cause broadcast storms, MAC table instability, and duplicate frames.

STP prevents these issues by logically blocking redundant paths, while still keeping them as backup.
It does this by electing a root bridge and calculating the best loop-free path to the root, placing non-optimal ports into a blocking state.

This ensures that the network has redundancy without loops.

Question 4️⃣

👉 What is the Root Bridge in STP, and how is it elected?

Ans:
The Root Bridge in STP is the switch with the lowest Bridge ID.
The Bridge ID consists of the bridge priority and the MAC address.

During election:

1. The switch with the lowest priority value becomes the root.
2. If priorities are equal, the switch with the lowest MAC address is elected.

In production networks, we usually manually set the priority to ensure the desired switch becomes the Root Bridge.

Question 5️⃣

👉 What are the different STP port states, and what is the purpose of each?

Ans:
STP has five port states:

1. Blocking – The port does not forward frames and does not learn MAC addresses. It only listens for BPDUs to prevent loops.
2. Listening – The port listens to BPDUs and participates in STP calculations but does not forward traffic or learn MAC addresses.
3. Learning – The port starts learning MAC addresses but still does not forward traffic.
4. Forwarding – The port forwards traffic and learns MAC addresses normally.
5. Disabled – The port is administratively down and does not participate in STP.

These states ensure a loop-free topology while allowing redundant links to become active when needed.

Question 6️⃣

👉 What is the difference between STP port roles and STP port states?

Ans:
🔑 Key Concept (Remember This Line)

1. Port roles decide where a port sits in the topology.
2. Port states decide what the port is allowed to do.

🔹 STP Port Roles (Topology Decision)
Port roles define the function of the port in the STP topology.

Root Port (RP)
→ Best path towards the Root Bridge (one per non-root switch)

Designated Port (DP)
→ Best port for a network segment (forwards traffic)

Blocking / Alternate Port
→ Redundant port kept for loop prevention

👉 Roles are based on path cost and topology.

🔹 STP Port States (Traffic Behavior)

Port states define what the port does operationally.
Blocking → No traffic, no MAC learning
Listening → STP calculation only
Learning → Learns MACs, no forwarding
Forwarding → Normal traffic flow
Disabled → Admin down

👉 States control traffic and MAC learning.

🧠 One-Line Interview Answer (Very Powerful)
Port roles define the logical position of a port in the STP topology, while port states define the operational behavior of the port, 
such as whether it forwards traffic or learns MAC addresses.

Question 7️⃣

👉 What is PortFast? Where do you enable it, and why?
(Think practical — real network usage)

Ans:
* PortFast is a feature that allows a switch port to move immediately into the forwarding state, bypassing the listening and learning states.
* It is typically enabled on access ports connected to end devices, such as servers or PCs.
* The main purpose of PortFast is to reduce the delay when a device connects to the network and to prevent issues like DHCP timeout.
* PortFast should never be enabled on ports connected to other switches, because it can cause Layer-2 loops.

Question 8️⃣

👉 What is BPDU Guard? Why do we use it with PortFast?
(Explain the risk and protection)

Ans:
BPDU Guard is a protection feature used on PortFast-enabled ports.
Its purpose is to prevent accidental or unauthorized switch connections.
If a PortFast-enabled port receives a BPDU, BPDU Guard immediately puts the port into err-disabled state.
This protects the network from Layer-2 loops that could occur if someone connects a switch to an access port.
BPDU Guard is typically enabled on access ports facing end devices.

Question 9️⃣

👉 What is LACP, and why do we use it?

Ans:
LACP stands for Link Aggregation Control Protocol.
It is used to bundle multiple physical interfaces into a single logical interface called a port-channel.
The main purpose of LACP is to provide higher bandwidth, redundancy, and load balancing between devices.
LACP also helps ensure that only correctly matched interfaces are bundled together, preventing misconfigurations.
It is commonly used between switch-to-switch, switch-to-server, or switch-to-firewall connections in data center networks.

Question 🔟

👉 What is vPC in Cisco Nexus? Why do we use it instead of traditional port-channel?
(Explain the problem first, then the solution)

Ans:
vPC stands for Virtual Port Channel.
It allows a device to form a single logical port-channel across two physical Nexus switches.
In traditional port-channel, all member interfaces must be connected to one switch, which creates a single point of failure.
With vPC, we can connect a downstream device, like a server or access switch, to two Nexus switches simultaneously, while appearing as a single port-channel.
This provides active-active forwarding, redundancy, and no STP blocking, making it ideal for data center environments.

Question 1️⃣1️⃣

👉 What happens if the vPC peer-link goes down?
(Explain carefully — this is a common interview trap)

Ans:
The vPC peer-link is critical for synchronization between the two Nexus switches.
If the vPC peer-link goes down but the vPC keepalive is still up, both switches detect the failure.

In this case:
* The secondary vPC switch shuts down its vPC member ports
* The primary switch continues forwarding traffic

This prevents split-brain scenarios and duplicate traffic.

If both peer-link and keepalive go down, a split-brain condition can occur, which is why the keepalive link is mandatory.

Question 1️⃣2️⃣

👉 What is a vPC orphan port?
Why is it a concern in vPC design?
(Think: connection + failure impact)

Ans:
A vPC orphan port is a port that is connected to only one of the vPC peer switches and is not part of a vPC.
It is a concern because during a vPC peer-link failure, the secondary switch may shut down its vPC member ports, but orphan ports remain up.
This can cause traffic blackholing or asymmetric traffic, especially if the orphan port is connected to a critical device.
Therefore, orphan ports should be carefully planned, and features like orphan-port suspend or proper design should be used.

Question 2️⃣

👉 How do you protect orphan ports in a vPC environment?
(Think features + best practices)

Ans.
Orphan ports are ports not part of a vPC.
During a vPC peer-link failure, these ports can cause traffic issues.
To protect them, we either enable orphan-port suspend, which automatically shuts them down when the peer-link fails, or we connect only non-critical devices to orphan ports.
Best practice is to minimize orphan ports in production networks.

Question 3️⃣

👉 What are the LACP modes, and what do they mean?
(Explain both Active and Passive, and when to use each)

Ans:
LACP (Link Aggregation Control Protocol) has three main modes in Cisco:

Active – The port actively sends LACP negotiation packets to form a port-channel.
Passive – The port will respond to LACP packets but does not initiate negotiation.
On / Static – The port is forced into a port-channel without using LACP negotiation.

Usage:
Active–Active: Both ends actively negotiate → preferred in production.
Active–Passive: One end initiates, the other responds → works too.
On / Static: Used when LACP is not desired, but risk of misconfiguration is higher.

Tip: LACP is preferred over static because it prevents misconfiguration and mismatched links.

Question 4️⃣

👉 What happens if there is a mismatch between LACP modes on two ends?
(Think practical – link will or will not come up?)

Ans:
If there is a mismatch between LACP modes on two ends, the port-channel will not form.
For example, if one side is Active and the other is Passive, it can form, but if one side is Passive and the other is Passive, no negotiation occurs and the port-channel fails.

In this case:
1. Physical interfaces remain up, but
2. No traffic is forwarded through the logical port-channel

This is why it’s important to match LACP modes correctly (Active–Active or Active–Passive) in production.

Question 1️⃣

You have a vPC between two Nexus switches. One Nexus switch goes down completely. What happens to traffic for devices connected via the vPC? How is redundancy handled?

Ans.
If one Nexus switch in a vPC goes down, the other switch continues forwarding traffic through the vPC member ports.
This ensures redundancy and active-active connectivity for downstream devices.
Orphan ports on the downed switch will lose connectivity, but vPC peer-link and keepalive design prevent split-brain and ensure that traffic continues safely.

Question 2️⃣

You configured LACP between two switches, but traffic is not passing. What steps do you take to troubleshoot?

Ans.
If LACP is configured but traffic is not passing, I would follow these steps:

Check LACP mode on both ends using show port-channel summary (ensure compatible modes: Active–Active or Active–Passive).
Verify physical interfaces — make sure all member ports are up and no errors.
Check port-channel status — confirm the logical port-channel is up and forwarding traffic.
Look for misconfigurations — speed, duplex, VLAN mismatch.
Check logs — see if LACP negotiation failed or ports are suspended.

This structured approach ensures the issue is isolated and resolved quickly.

Question 3️⃣

A device connected to an orphan port loses connectivity during a vPC peer-link failure. How do you resolve this?

Ans:
If a device connected to an orphan port loses connectivity during a vPC peer-link failure, the issue can be resolved by ensuring redundancy.
Best practices include:

Connect critical devices to vPC member ports on both Nexus switches instead of orphan ports.

If orphan ports must be used, enable orphan-port suspend, so the network avoids traffic blackholes during failures.

Design the network so critical services are never solely dependent on orphan ports.

This ensures resiliency and avoids asymmetric traffic or downtime.

Question 4️⃣

What is the role of the vPC keepalive link, and what happens if it fails while the peer-link is still up?

Ans:
The vPC keepalive link is a Layer-3 link used to monitor the health of the peer Nexus switch.
It does not carry data traffic. Its main purpose is to detect if the peer switch is alive.

If the keepalive fails but the peer-link is still up:

The switch may assume the peer is down and put its vPC member ports into a suspended state to prevent split-brain.

Traffic will continue through the surviving switch only.

This ensures that duplicate traffic or loops do not occur even if the peer is unreachable at the control level.

