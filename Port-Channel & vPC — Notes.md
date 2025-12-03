# EtherChannel, Port-Channel & vPC – Cisco Notes

This repository contains clear and simple explanations about:
- What is EtherChannel?
- What is Port-Channel?
- Why do we use Port-Channels?
- What is vPC in Nexus?
- Differences between Port-Channel vs vPC

# EtherChannel - Definition

EtherChannel is a technology that combines multiple physical links between two network devices into one logical link to increase bandwidth and provide redundancy.

# How EtherChannel Works

EtherChannel bundles 2–8 physical links.
All links must match:
- Speed
- Duplex
- VLAN allowed
- Trunk/Access mode

Load-balancing works per:
- Source MAC
- Destination MAC
- Source IP
- Destination IP

Protocols:
- PAgP (Cisco)
- LACP (IEEE)

# EtherChannel Diagram

+---------+        +---------+
| SwitchA |========| SwitchB |
|         |========|         |
|         |========|         |
+---------+        +---------+
     ||                ||
   Port-Channel (Logical)


# Port-Channel - Definition

Port-Channel is the *logical interface* created when an EtherChannel is formed.
You configure STP, VLAN, and routing on this Port-Channel interface (not on physical links).

# How Port-Channel Works

Port-Channel = Logical interface
Eth1/1 + Eth1/2 + Eth1/3 = Port-Channel10

Config is applied on Port-Channel:
- switchport trunk allowed vlan ...
- ip address ...
- spanning-tree ...

Physical interfaces inherit the config from Port-Channel.

# Port-Channel Diagram

Port-Channel10
      ||
+-----------------+
|   Logical Link  |
+-----------------+
   ||     ||     ||
 Eth1/1  Eth1/2  Eth1/3

# vPC - Definition

vPC (Virtual Port-Channel) allows two separate Nexus switches to appear as one logical switch to downstream devices.
Downstream device believes it is connected to a single switch, but actually connected to two — providing:
- No STP blocking
- Active/Active forwarding
- High availability

# vPC Architecture Notes

Components:
- vPC Peer Switches (Nexus A & Nexus B)
- vPC Peer-Link
- vPC Keepalive Link
- vPC Member Ports

Benefits:
- Active/Active forwarding
- Loop-free topology
- No STP blocking
- Multi-chassis link aggregation

# vPC Diagram

        vPC Peer-Link
    +--------------------+
    |                    |
+---------+          +---------+
| Nexus A |          | Nexus B |
+---------+          +---------+
     ||                  ||
     ||    vPC 10        ||
     +---------+----------+
               |
        +--------------+
        |   Server     |
        +--------------+


# EtherChannel vs Port-Channel

EtherChannel = Technology
Port-Channel = Logical interface created by EtherChannel

Example:
EtherChannel bundles links 1/1, 1/2
Port-Channel10 is the logical interface for these links

# vPC vs Port-Channel

Port-Channel:
- Single switch
- All physical links connect to same switch

vPC:
- Two separate Nexus switches
- Active/Active forwarding
- Multi-chassis port channel

# Full Comparison Table

Feature         | EtherChannel | Port-Channel | vPC
------------------------------------------------------------
Definition      | Bundling tech| Logical Intf | Multi-chassis bundle
Switches needed | 2 links only | 1 switch     | 2 switches
Protocol        | LACP/PAgP    | N/A          | Uses LACP
STP Behavior    | Normal       | Normal       | Active/Active
Redundancy      | Yes          | Yes          | Highest


## Example ##

You have two physical links between two switches:

Switch A ---------------- Switch B
Eth1/1   ----------------  Eth1/1
Eth1/2   ----------------  Eth1/2

These are physical interfaces.

Now, you want:

* More bandwidth
* Redundancy
* No STP blocking
* Single logical bundle

So you enable EtherChannel technology using LACP:

🔧 Step 1: Activate EtherChannel (bundling physical ports)

On both switches:
interface eth1/1
 channel-group 10 mode active

interface eth1/2
 channel-group 10 mode active

# Note: Now EtherChannel technology bundles these 2 interfaces.

🔧 Step 2: A New Logical Interface Appears → Port-Channel10

The moment you bundle interfaces, the switch automatically creates a new interface called: "interface port-channel10"

This is not a physical port.

This is a logical virtual interface.

💡 Why is Port-Channel a Logical Interface?

Because:

* You don’t configure anything on eth1/1 or eth1/2
* You configure everything on port-channel10 instead

Example:

interface port-channel10
 switchport trunk allowed vlan 10,20,30
 spanning-tree portfast trunk

These settings are applied to both links eth1/1 and eth1/2.

🧠 Think of it like this
Physical ports (Eth1/1, Eth1/2)

→ Are like cables.

EtherChannel

→ Is the technology that binds these cables together.

Port-Channel10

→ Is the final single logical pipe formed by bundling those cables.





