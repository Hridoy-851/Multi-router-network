# Multi-Router Network

A hands-on Cisco Packet Tracer project focused on building, configuring, and troubleshooting a multi-router network using IPv4, static routing, and OSPF.

This project is part of my 6-month journey toward becoming a **Junior Network Engineer / NOC Engineer / Network Support Engineer**.

---

## 🎯 Project Objective

The objective of this project is to move beyond basic networking labs and develop practical skills in:

* Multi-router network configuration
* IPv4 addressing and subnetting
* Static routing
* Dynamic routing with OSPF
* OSPF neighbor relationships
* Routing table analysis
* Network troubleshooting
* Connectivity verification
* Cisco IOS CLI

The project follows a practical learning cycle:

> **Learn → Build → Break → Troubleshoot → Document → Explain**

---

## 🌐 Network Topology

Current topology:

```text
                 Router-to-Router Link
              10.0.0.0/30
                   
PC0 ── SW1 ── Router0 ───────── Router1 ── SW2 ── PC1
               │                    │
        192.168.10.0/24       192.168.20.0/24
```

---

## 📋 IP Addressing

| Device  | Interface     | IP Address       | Purpose               |
| ------- | ------------- | ---------------- | --------------------- |
| PC0     | NIC           | 192.168.10.10/24 | Host                  |
| Router0 | LAN Interface | 192.168.10.1/24  | Default Gateway       |
| Router0 | G0/1          | 10.0.0.1/30      | Router-to-Router Link |
| Router1 | G0/1          | 10.0.0.2/30      | Router-to-Router Link |
| Router1 | LAN Interface | 192.168.20.1/24  | Default Gateway       |
| PC1     | NIC           | 192.168.20.10/24 | Host                  |

---

## 🔧 Technologies & Concepts

### IPv4

* IPv4 addressing
* Subnet masks
* Network and host addresses
* /24 LAN networks
* /30 point-to-point network

### Routing

* Static routing
* Dynamic routing
* OSPF
* Routing table analysis
* Remote network reachability

### OSPF

* OSPF process configuration
* Router ID
* Network statements
* OSPF neighbor adjacency
* OSPF states
* Route advertisement
* OSPF verification

### Troubleshooting

* Interface status analysis
* OSPF neighbor troubleshooting
* Ping-based connectivity testing
* Root-cause identification
* Configuration recovery
* Verification after corrective action

---

# 🧪 Labs

## Lab 01 — Multi-Router Static Routing

**Status:** ✅ Completed

### Objectives

* Connect two routers.
* Configure IPv4 addressing.
* Establish router-to-router connectivity.
* Configure static routes.
* Allow communication between remote LANs.
* Verify connectivity using `ping`.

### Packet Tracer File

`multi-router-static-routing.pkt`

---

## Lab 02 — OSPF

**Status:** 🔄 In Progress

### Topics Practiced

* OSPF process configuration
* OSPF Router ID
* OSPF network statements
* OSPF neighbor adjacency
* OSPF route learning
* OSPF verification commands
* Troubleshooting OSPF

Important verification commands:

```text
show ip ospf
show ip ospf neighbor
show ip route
show ip protocols
```

---

# 🚨 Troubleshooting Incidents

A major goal of this project is to practice troubleshooting intentionally broken networks.

Each incident is documented using:

**Problem → Symptoms → Investigation → Root Cause → Solution → Verification → Lessons Learned**

---

## Incident 01 — OSPF Neighbor Adjacency Down

**Status:** ✅ Resolved

### Problem

PC0 could not communicate with PC1 on the remote LAN.

### Root Cause

Router0's router-to-router interface was manually disabled.

The interface showed:

```text
administratively down
```

This prevented connectivity between Router0 and Router1 and consequently prevented the OSPF neighbor relationship from becoming operational.

### Investigation

Commands used:

```text
show ip interface brief
show ip ospf neighbor
```

### Solution

The interface was enabled:

```text
interface gigabitEthernet 0/1
no shutdown
```

### Verification

The OSPF adjacency was restored to:

```text
FULL
```

End-to-end connectivity was then verified:

```text
ping 192.168.20.10
```

Result:

```text
Success rate: 100%
```

Detailed documentation:

[`Incident 01 — OSPF Neighbor Down`](troubleshooting/incident-01-ospf-interface-down.md)

---

# 🔍 Troubleshooting Methodology

For network failures, I am practicing a systematic troubleshooting process:

```text
1. Identify the symptom
          ↓
2. Test connectivity
          ↓
3. Check interface status
          ↓
4. Check routing information
          ↓
5. Check routing protocol status
          ↓
6. Identify the root cause
          ↓
7. Apply corrective action
          ↓
8. Verify the fix
          ↓
9. Document the incident
```

The objective is to avoid making random configuration changes and instead identify the actual root cause.

---

# 🖥️ Cisco IOS Commands Practiced

### Interface Verification

```text
show ip interface brief
```

### Routing Table

```text
show ip route
```

### OSPF Neighbor Verification

```text
show ip ospf neighbor
```

### OSPF Configuration Information

```text
show ip ospf
show ip protocols
```

### Connectivity Testing

```text
ping
traceroute
```

---

# 📁 Current Project Structure

```text
multi-router-network/
│
├── README.md
│
├── multi-router-static-routing.pkt
│
└── troubleshooting/
    └── incident-01-ospf-interface-down.md
```

This structure will be expanded as additional labs and troubleshooting incidents are completed.

---

# 📈 Skills Developed

Through this project I am developing practical experience with:

* Cisco Packet Tracer
* Cisco IOS CLI
* IPv4 addressing
* Subnetting
* Static routing
* OSPF
* Routing table analysis
* OSPF neighbor troubleshooting
* Network connectivity testing
* Systematic troubleshooting
* Technical documentation

---

# 🚀 Future Improvements

Planned additions to this project include:

* More OSPF troubleshooting scenarios
* Additional routers
* Multiple LANs
* VLAN implementation
* Inter-VLAN routing
* OSPF with multiple networks
* ACL implementation
* SSH management
* Network security
* Additional routing scenarios
* 20+ troubleshooting incidents

The final version of the project will evolve into a more realistic enterprise network.

---

# 📚 Learning Approach

This project is not intended to demonstrate only successful configurations.

I am intentionally creating, breaking, troubleshooting, and documenting network scenarios to develop practical problem-solving skills.

> **Configuration is only part of networking.
> Understanding why a network fails—and knowing how to fix it—is equally important.**

---

## 👤 Author

**Md. Hridoy Sheikh**

4th-Year CSE Student
Aspiring Junior Network Engineer

### Career Focus

* Junior Network Engineer
* NOC Engineer
* Network Support Engineer
* IT Support / Network Operations

---

## 📌 Project Status

**Current Stage:** OSPF & Troubleshooting Development

**Project Status:** 🟡 In Progress

More labs, troubleshooting incidents, documentation, and enterprise networking concepts will be added as the project develops.
