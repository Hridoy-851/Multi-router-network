# Troubleshooting Incident #3 — Remote LAN Unreachable

## Incident Summary

PC0 was unable to reach PC1 across the multi-router network.

The troubleshooting process was used to determine whether the problem was related to the end device, routing, OSPF, or a router interface.

## Topology

```text
PC0 ── SW1 ── Router0 ───── Router1 ── SW2 ── PC1
```

## Addressing

| Device  | Interface | IP Address       |
| ------- | --------- | ---------------- |
| Router0 | Gi0/0     | 192.168.10.1/24  |
| Router0 | Gi0/1     | 10.0.0.1/30      |
| Router1 | Gi0/1     | 10.0.0.2/30      |
| Router1 | Gi0/0     | 192.168.20.1/24  |
| PC0     | NIC       | 192.168.10.10/24 |
| PC1     | NIC       | 192.168.20.10/24 |

## Reported Symptom

PC0 could not reach PC1:

```text
PC0 → 192.168.20.10
```

The ping returned:

```text
Reply from 192.168.10.1: Destination host unreachable.
```

Result:

```text
Packets: Sent = 4, Received = 0, Lost = 4 (100% loss)
```

## Troubleshooting Process

### 1. Check Router0's Route to the Remote LAN

Command:

```text
Router0# show ip route 192.168.20.0
```

Result:

```text
% Network not in table
```

This showed that Router0 had no route to the remote LAN `192.168.20.0/24`.

### 2. Test Router0 → Router1 LAN Interface

Command:

```text
Router0# ping 192.168.20.1
```

Result:

```text
Success rate is 0 percent (0/5)
```

This confirmed that Router0 could not reach Router1's LAN interface.

### 3. Check OSPF Neighbor Status

Command:

```text
Router0# show ip ospf neighbor
```

Result:

```text
2.2.2.2   1   FULL/DR   10.0.0.2   GigabitEthernet0/1
```

The OSPF adjacency was still `FULL`.

This was an important finding: the OSPF neighbor relationship itself was healthy.

### 4. Check OSPF Routes

Command:

```text
Router0# show ip route ospf
```

Result:

```text
```

No OSPF routes were displayed.

The remote LAN `192.168.20.0/24` was therefore no longer being learned through OSPF.

### 5. Inspect Router1 Interfaces

Command:

```text
Router1# show ip interface brief
```

The relevant interface showed:

```text
GigabitEthernet0/0    192.168.20.1    administratively down    down
```

This identified the fault.

## Root Cause

Router1's LAN interface `GigabitEthernet0/0` was administratively shut down.

Because this interface was down:

1. Router1 lost its connected `192.168.20.0/24` network.
2. Router1 stopped advertising that network through OSPF.
3. Router0 removed the OSPF route to `192.168.20.0/24`.
4. PC0 could no longer reach PC1.

## Corrective Action

The interface was restored with:

```text
Router1# configure terminal
Router1(config)# interface gigabitEthernet0/0
Router1(config-if)# no shutdown
Router1(config-if)# end
```

## Verification

Router1's interface returned to:

```text
GigabitEthernet0/0    192.168.20.1    up    up
```

Router0 relearned the remote network through OSPF:

```text
O    192.168.20.0/24 [110/2] via 10.0.0.2
```

Finally, PC0 successfully reached PC1:

```text
C:\>ping 192.168.20.10
```

Result:

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

## Final Status

**RESOLVED**

## Lessons Learned

* Check interface status before assuming OSPF is broken.
* A `FULL` OSPF adjacency does not guarantee that every expected network is being advertised.
* A connected network disappearing from a router can cause its OSPF route to disappear from neighboring routers.
* Use `show ip route` and `show ip ospf neighbor` together when troubleshooting routing problems.
* Always verify connectivity after applying a fix.
* Troubleshoot systematically instead of changing multiple configurations at once.
