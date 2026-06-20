# Enterprise Network Segmentation Using VLANs, STP, and Port Security

## Project Overview

This project demonstrates the implementation of VLANs, Trunking, Spanning Tree Protocol (STP), and Port Security using Cisco 2960 switches in Cisco Packet Tracer.

The network is divided into four departments:

- HR (VLAN 10)
- IT (VLAN 20)
- Finance (VLAN 30)
- Marketing (VLAN 40)

The objective of this project is to improve network organization, reduce broadcast traffic, provide VLAN isolation, prevent switching loops using STP, and secure switch ports using Port Security.

---

## Technologies Used

- Cisco Packet Tracer
- Cisco 2960 Switches
- VLANs
- Trunking
- Spanning Tree Protocol (STP)
- Port Security

---

## Network Topology

The network consists of:

- 2 Cisco 2960 Switches
- 8 PCs
- 2 Trunk Links between switches
- 4 VLANs

Department Allocation:

| Department | VLAN ID | Devices |
|------------|----------|----------|
| HR | VLAN 10 | PC0, PC1 |
| IT | VLAN 20 | PC2, PC3 |
| Finance | VLAN 30 | PC4, PC5 |
| Marketing | VLAN 40 | PC6, PC7 |

---

## IP Addressing Scheme

### VLAN 10 - HR

| Device | IP Address |
|----------|------------|
| PC0 | 192.168.10.10 |
| PC1 | 192.168.10.20 |

### VLAN 20 - IT

| Device | IP Address |
|----------|------------|
| PC2 | 192.168.20.10 |
| PC3 | 192.168.20.20 |

### VLAN 30 - Finance

| Device | IP Address |
|----------|------------|
| PC4 | 192.168.30.10 |
| PC5 | 192.168.30.20 |

### VLAN 40 - Marketing

| Device | IP Address |
|----------|------------|
| PC6 | 192.168.40.10 |
| PC7 | 192.168.40.20 |

Subnet Mask for all devices:

```bash
255.255.255.0
```

---

# VLAN Configuration

Run on both switches.

```bash
enable
configure terminal

vlan 10
name HR

vlan 20
name IT

vlan 30
name FINANCE

vlan 40
name MARKETING

end
```

### Verification

```bash
show vlan brief
```

---

# Switch 1 Configuration

Assign HR and IT VLANs.

```bash
configure terminal

interface range fa0/1-2
switchport mode access
switchport access vlan 10

interface range fa0/3-4
switchport mode access
switchport access vlan 20

end
```

### Explanation

- Fa0/1 – Fa0/2 → HR Department
- Fa0/3 – Fa0/4 → IT Department

---

# Switch 2 Configuration

Assign Finance and Marketing VLANs.

```bash
configure terminal

interface range fa0/1-2
switchport mode access
switchport access vlan 30

interface range fa0/3-4
switchport mode access
switchport access vlan 40

end
```

### Explanation

- Fa0/1 – Fa0/2 → Finance Department
- Fa0/3 – Fa0/4 → Marketing Department

---

# Trunk Configuration

Configure trunk ports on both switches.

```bash
configure terminal

interface range fa0/23-24
switchport mode trunk

end
```

### Verification

```bash
show interfaces trunk
```

### Explanation

Trunk links allow multiple VLANs to travel between switches using a single physical connection.

---

# Spanning Tree Protocol (STP)

Two physical links were connected between the switches to create redundancy.

Without STP, this configuration could cause network loops and broadcast storms.

Verify STP status:

```bash
show spanning-tree
```

### Observed Output

```text
Fa0/23 Root FWD
Fa0/24 Altn BLK
```

### Explanation

- Root FWD = Active path
- Altn BLK = Backup path
- STP blocks redundant links to prevent loops
- If the active link fails, the blocked link becomes active automatically

---

# Port Security Configuration

Port Security was configured on Switch 1, Interface Fa0/1.

```bash
configure terminal

interface fa0/1

switchport port-security

switchport port-security maximum 1

switchport port-security violation shutdown

switchport port-security mac-address sticky

end
```

### Explanation

| Command | Purpose |
|----------|----------|
| switchport port-security | Enables Port Security |
| maximum 1 | Allows only one device |
| violation shutdown | Shuts down port if violation occurs |
| mac-address sticky | Learns MAC address automatically |

### Verification

```bash
show port-security
```

---

# Connectivity Testing

## Successful Communication

Devices within the same VLAN can communicate.

Example:

```bash
ping 192.168.10.20
```

Expected Result:

```text
Reply from 192.168.10.20
```

---

## VLAN Isolation Testing

Devices in different VLANs cannot communicate.

Example:

```bash
ping 192.168.20.10
```

Expected Result:

```text
Request timed out
```

This confirms VLAN isolation.

---

# Verification Commands

```bash
show vlan brief

show interfaces trunk

show spanning-tree

show port-security
```

---

# Project Files

This repository contains:

- README.md
- Cisco Packet Tracer (.pkt) File
- Network Topology Screenshots
- VLAN Verification Screenshots
- STP Verification Screenshots
- Port Security Verification Screenshots

---

# Skills Demonstrated

- VLAN Configuration
- Cisco Switching
- Trunk Configuration
- Layer 2 Networking
- Spanning Tree Protocol (STP)
- Port Security
- Network Segmentation
- Troubleshooting
- Enterprise Network Design

---

# Learning Outcomes

Through this project, I learned:

- How VLANs separate departments within a network
- How trunk links carry VLAN traffic between switches
- How STP prevents switching loops
- How Port Security protects switch ports from unauthorized devices
- How to verify switch configurations using Cisco IOS commands

---

# Conclusion

This project successfully demonstrates the implementation of an enterprise-style Layer 2 network using Cisco 2960 switches. VLANs were used to segment departments, trunk links enabled VLAN communication across switches, STP prevented network loops, and Port Security improved network security.

The project provides practical experience with fundamental networking concepts commonly used in real-world enterprise environments.

