# Troubleshooting Incident #2 — Intermittent First-Ping Loss

## Incident Summary

PC0 was reported as being unable to reach PC1.

The network was investigated systematically to determine whether the problem was related to OSPF, routing, WAN connectivity, or the end devices.

## Topology

```text
PC0 ── SW1 ── Router0 ───── Router1 ── SW2 ── PC1
```

## Reported Symptom

PC0 was suspected to have connectivity problems reaching:

```text
PC1: 192.168.20.10
```

## Troubleshooting Steps

### 1. Verify Router0 → Router1 LAN

```text
Router0# ping 192.168.20.1
```

Result:

```text
Success rate is 100 percent (5/5)
```

Router0 could successfully reach Router1's LAN interface.

### 2. Verify Router0 → PC1

```text
Router0# ping 192.168.20.10
```

Initial result:

```text
.!!!!
Success rate is 80 percent (4/5)
```

The first packet was unsuccessful, but the following four packets succeeded.

### 3. Verify Router1 → Router0 LAN

```text
Router1# ping 192.168.10.1
```

Result:

```text
Success rate is 100 percent (5/5)
```

### 4. Verify Router1 → PC0

```text
Router1# ping 192.168.10.10
```

Initial result:

```text
.!!!!
Success rate is 80 percent (4/5)
```

Again, only the first packet was unsuccessful.

### 5. Check ARP Tables

Router0 contained an ARP entry for its directly connected next hop:

```text
10.0.0.2 → 0001.4364.EC02
```

Router1 contained an ARP entry for PC1:

```text
192.168.20.10 → 0001.6324.CA3A
```

The ARP tables were consistent with the network topology.

### 6. Perform End-to-End PC Test

From PC0:

```text
C:\>ping 192.168.20.10
```

Result:

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

A second test also produced:

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

## Root Cause / Finding

No persistent network fault was found.

The initial `80%` router ping result was consistent with normal ARP resolution behavior. After ARP information was learned, subsequent communication succeeded.

## OSPF Status

OSPF was verified to be operational:

* Router0 and Router1 had a `FULL` OSPF adjacency.
* Router0 learned `192.168.20.0/24` through OSPF.
* Router1 learned `192.168.10.0/24` through OSPF.
* WAN connectivity between the routers was successful.

Therefore, OSPF was not the cause of the observed behavior.

## Resolution

No configuration change was required.

The network was confirmed to be operating normally through repeated end-to-end connectivity tests.

## Lessons Learned

1. Do not assume that a single failed ping means the network is broken.
2. Test connectivity repeatedly before changing configuration.
3. Check the OSPF neighbor state before troubleshooting OSPF routes.
4. Check routing before troubleshooting end devices.
5. Understand ARP behavior and its effect on the first packet.
6. Troubleshoot from lower-level connectivity toward the end-to-end application path.
7. Always verify the final result from the affected end device.

## Verification

Final verification:

```text
PC0 → PC1
4/4 packets received
0% packet loss
```

Status: **RESOLVED / NO PERSISTENT FAULT FOUND**
