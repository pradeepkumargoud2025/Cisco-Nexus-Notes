## 📌 Cisco Nexus – Functional Planes

The **supervisor module** in Cisco Nexus switches divides the traffic it manages into **three functional planes**:

---

### 1️⃣ Data Plane
- Handles all **data (transit) traffic**.  
- Core function: **forwarding packets** from one interface to another.  
- Packets **not destined** for the switch itself are called **transit packets**.  
- These packets are processed and switched entirely within the data plane.

---

### 2️⃣ Control Plane
- Handles all **routing protocol control traffic**.  
- Runs protocols such as:
  - **BGP** (Border Gateway Protocol)
  - **OSPF** (Open Shortest Path First)
- Sends and receives **control packets** between devices.
- These packets are **destined to the router's own addresses** and influence how traffic is routed.

---

### 3️⃣ Management Plane
- Responsible for **device management functions**.  
- Handles:
  - **CLI** (Command-Line Interface) access
  - **SNMP** (Simple Network Management Protocol)
- Allows administrators to **configure, monitor, and maintain** the device.

---

💡 **Summary:**  
- **Data Plane** → Moves user traffic.  
- **Control Plane** → Learns and decides where traffic should go.  
- **Management Plane** → Lets you manage and monitor the device.
