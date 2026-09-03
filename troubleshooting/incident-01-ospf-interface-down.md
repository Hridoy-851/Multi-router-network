# Incident 01 — OSPF Neighbor Adjacency Down

## 1. Incident Overview

**Incident ID:** INC-001
**Category:** Routing / OSPF
**Severity:** Medium
**Environment:** Cisco Packet Tracer
**Topology:** PC0 → SW1 → Router0 → Router1 → SW2 → PC1

### Objective

Restore connectivity between the two remote LANs by identifying and resolving the failure of the OSPF neighbor adjacency between Router0 and Router1.

---

## 2. Problem Statement

PC0 was unable to communicate with PC1, which was located on a different LAN.

PC0 could successfully communicate with its local default gateway, but traffic destined for the remote LAN was unsuccessful.

---

## 3. Network Information

| Device  | Interface     | IP Address       | Role                  |
| ------- | ------------- | ---------------- | --------------------- |
| PC0     | NIC           | 192.168.10.10/24 | Source host           |
| Router0 | LAN interface | 192.168.10.1/24  | Default gateway       |
| Router0 | G0/1          | 10.0.0.1         | Router-to-router link |
| Router1 | G0/1          | 10.0.0.2         | Router-to-router link |
| Router1 | LAN interface | 192.168.20.1/24  | Default gateway       |
| PC1     | NIC           | 192.168.20.10/24 | Destination host      |

---

## 4. Symptoms

The following symptoms were observed:

* PC0 could ping its local gateway `192.168.10.1`.
* PC0 could not ping PC1 at `192.168.20.10`.
* Router0 did not display an OSPF neighbor.
* The Router0–Router1 connection was not operational.
* The remote network was therefore unreachable.

---

## 5. Troubleshooting Process

The troubleshooting process followed a structured approach:

**Connectivity Test → Interface Check → OSPF Neighbor Check → Root Cause Identification → Repair → Verification**

---

### Step 1 — Test Local Connectivity

From PC0:

```text
ping 192.168.10.1
```

### Result

The ping was successful.

This confirmed that:

* PC0 had connectivity to the local switch.
* The local Layer 2 connection was working.
* PC0 could reach Router0.
* The problem was likely beyond the local LAN.

---

### Step 2 — Test Remote Connectivity

From PC0:

```text
ping 192.168.20.10
```

### Result

The ping failed.

This indicated that connectivity between the local network and the remote network needed further investigation.

---

### Step 3 — Check Router0 Interface Status

On Router0:

```text
show ip interface brief
```

The output showed:

```text
Interface              IP-Address      Status                  Protocol
GigabitEthernet0/1     10.0.0.1        administratively down   down
```

### Finding

The Router0 router-to-router interface was **administratively down**.

The term `administratively down` indicates that the interface has been manually disabled with the `shutdown` command.

---

### Step 4 — Check OSPF Neighbor Relationship

On Router0:

```text
show ip ospf neighbor
```

### Result

```text
No OSPF neighbor displayed.
```

### Finding

Router0 had no active OSPF neighbor relationship with Router1.

Because the physical/logical connection between the routers was down, OSPF could not establish or maintain its neighbor adjacency.

---

## 6. Root Cause

The root cause was a manually disabled Router0 router-to-router interface.

Router0's:

```text
GigabitEthernet0/1
```

was in an:

```text
administratively down
```

state.

The interface shutdown caused the Router0–Router1 connection to fail, which prevented the OSPF neighbor relationship from being established.

As a result, Router0 could not learn the remote network through OSPF.

---

## 7. Corrective Action

The affected interface was enabled using:

```text
Router0# configure terminal
Router0(config)# interface gigabitEthernet 0/1
Router0(config-if)# no shutdown
```

The interface was then allowed to transition back to an operational state.

---

## 8. Verification

### Step 1 — Verify Interface Status

Command:

```text
show ip interface brief
```

The interface should now show:

```text
GigabitEthernet0/1    10.0.0.1    up    up
```

This confirms that the router-to-router interface is operational.

---

### Step 2 — Verify OSPF Neighbor

Command:

```text
show ip ospf neighbor
```

The OSPF neighbor relationship was restored.

The adjacency progressed through the OSPF states and reached:

```text
FULL
```

`FULL` indicates that the OSPF routers have successfully established their adjacency and synchronized their link-state databases.

---

### Step 3 — Verify End-to-End Connectivity

From PC0:

```text
ping 192.168.20.10
```

### Result

```text
Success rate: 100%
```

PC0 could successfully communicate with PC1.

---

## 9. Incident Resolution

**Status:** Resolved

The OSPF neighbor adjacency was restored by enabling the disabled router-to-router interface.

End-to-end connectivity between the two LANs was successfully verified.

---

## 10. Technical Lessons Learned

This incident reinforced several important networking concepts:

1. `show ip interface brief` is an important first-level troubleshooting command for checking interface state and IP addressing.

2. `administratively down` normally indicates that an interface has been manually disabled with the `shutdown` command.

3. `show ip ospf neighbor` can be used to verify OSPF neighbor relationships.

4. OSPF requires operational interfaces and connectivity between neighboring routers before an adjacency can be established.

5. A failed router-to-router link can prevent routing information from being exchanged and make remote networks unreachable.

6. Troubleshooting should be systematic rather than based on random configuration changes.

---

## 11. Troubleshooting Method

The incident followed this troubleshooting methodology:

```text
Identify the symptom
        ↓
Test connectivity
        ↓
Check interface status
        ↓
Check routing/OSPF neighbor status
        ↓
Identify root cause
        ↓
Apply corrective action
        ↓
Verify interface
        ↓
Verify OSPF adjacency
        ↓
Verify end-to-end connectivity
```

---

## 12. Commands Used

### Connectivity

```text
ping 192.168.10.1
ping 192.168.20.10
```

### Interface troubleshooting

```text
show ip interface brief
```

### OSPF troubleshooting

```text
show ip ospf neighbor
```

### Interface recovery

```text
configure terminal
interface gigabitEthernet 0/1
no shutdown
```

---

## 13. Portfolio Takeaway

This incident demonstrates the ability to:

* Perform basic network troubleshooting.
* Interpret Cisco interface status.
* Troubleshoot OSPF neighbor relationships.
* Identify a Layer 1/Layer 2 connectivity problem affecting routing.
* Apply an appropriate corrective configuration.
* Verify the repair using Cisco IOS commands and end-to-end connectivity testing.

**Key principle:**

> Don't assume the routing protocol is the problem. Check the underlying interface and connectivity first.
