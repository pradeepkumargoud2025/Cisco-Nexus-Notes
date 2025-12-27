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
