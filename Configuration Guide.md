Spanning Tree Edge Port (PortFast Equivalent)
---------------------------------------------
The spanning-tree port type edge command on Cisco Nexus switches configures a port as an edge port, which is the NX-OS equivalent of PortFast on Cisco IOS switches.

This configuration allows the port to immediately enter the forwarding state without waiting for the Spanning Tree Protocol (STP) convergence process. It provides faster connectivity for end devices such as servers, workstations, or IP phones connected to that port.

⚠️ Note: This command should only be applied to ports connected to hosts. It must not be used on ports connecting to other switches, as doing so could lead to network loops.

interface ethernet1/1
  switchport
  switchport mode access
  spanning-tree port type edge
