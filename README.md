# Troubleshooting #5: Intranet Drops Out on One Desk Since a New PC Arrived

_Level: Easy — BuildTheNet 3D Sandbox troubleshooting — IP addressing_

**Status: Verified.** Completed on 2026-11-25; every requirement was machine-checked against the network built in BuildTheNet and passed.

Snapshot taken 2026-11-25. Built in a simulation — no physical or production equipment was involved.

## The ticket

**Reported symptom:** Windows reported an IP address conflict; the intranet works on PC-1 or PC-2, never both.

Helpdesk Ticket #105: Since PC-2 was set up this morning, PC-1's user says the intranet stops working, and Windows showed "There is an IP address conflict with another system on the network". PC-1's address is reserved for it in the IPAM sheet; PC-2 was addressed by hand.

_Area: IP addressing_

## Required resolution

Make PC-1 and PC-2 both reach Intranet-Server reliably. PC-1 keeps its reserved address, and no server or router changes.

## Checks

| # | Check | Status | Checked by |
|---|---|---|---|
| 1 | PC-1 to Intranet-Server works | Passed | Sandbox check |
| 2 | PC-2 to Intranet-Server works | Passed | Sandbox check |
| 3 | Intranet-Server to PC-2 works | Passed | Sandbox check |
| 4 | Intranet-Server to PC-1 works | Passed | Sandbox check |
| 5 | PC-1 keeps its documented addressing | Passed | Sandbox check |
| 6 | Router-1 keeps its documented addressing | Passed | Sandbox check |
| 7 | Intranet-Server keeps its documented addressing | Passed | Sandbox check |

## Root cause

PC-2 was configured with 192.168.1.10, the address already reserved for PC-1.

## The fix

What was changed to resolve the ticket, compared with the network as it was reported:

Compared with the starting topology, the learner changed the configuration of 1 device.

**PC-2**

```diff
  interface FastEthernet0
-  ip address 192.168.1.10 255.255.255.0
+  ip address 192.168.1.11 255.255.255.0
```

## Result

Every check passed on this snapshot:

> Every host on 192.168.1.0/24 has its own address again, and both PCs reach Intranet-Server.

## Topology

![Topology diagram](topology.svg)

## Addressing

| Device | Interface | Address | Mask | Mode | Gateway |
|---|---|---|---|---|---|
| PC-1 | FastEthernet0 | 192.168.1.10/24 | 255.255.255.0 | VLAN 1 | 192.168.1.1 |
| PC-2 | FastEthernet0 | 192.168.1.11/24 | 255.255.255.0 | VLAN 1 | 192.168.1.1 |
| Router-1 | GigabitEthernet0/0 | 192.168.1.1/24 | 255.255.255.0 | routed |  |
| Router-1 | GigabitEthernet0/1 | 10.1.1.1/24 | 255.255.255.0 | routed |  |
| Intranet-Server | FastEthernet0 | 10.1.1.20/24 | 255.255.255.0 | VLAN 1 | 10.1.1.1 |

## Device configurations

Running-configs as `show running-config` prints them in the BuildTheNet sandbox. Passwords, secrets, keys and SNMP communities are replaced with `<removed>`.

- [Switch-1](configs/Switch-1.txt)
- [Router-1](configs/Router-1.txt)

---

Built on [BuildTheNet](https://www.buildthenet.io/sandbox)
