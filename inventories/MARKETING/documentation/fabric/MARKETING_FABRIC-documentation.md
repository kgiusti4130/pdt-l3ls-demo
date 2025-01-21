# MARKETING_FABRIC

## Table of Contents

- [Fabric Switches and Management IP](#fabric-switches-and-management-ip)
  - [Fabric Switches with inband Management IP](#fabric-switches-with-inband-management-ip)
- [Fabric Topology](#fabric-topology)
- [Fabric IP Allocation](#fabric-ip-allocation)
  - [Fabric Point-To-Point Links](#fabric-point-to-point-links)
  - [Point-To-Point Links Node Allocation](#point-to-point-links-node-allocation)
  - [Loopback Interfaces (BGP EVPN Peering)](#loopback-interfaces-bgp-evpn-peering)
  - [Loopback0 Interfaces Node Allocation](#loopback0-interfaces-node-allocation)
  - [VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)](#vtep-loopback-vxlan-tunnel-source-interfaces-vteps-only)
  - [VTEP Loopback Node allocation](#vtep-loopback-node-allocation)

## Fabric Switches and Management IP

| POD | Type | Node | Management IP | Platform | Provisioned in CloudVision | Serial Number |
| --- | ---- | ---- | ------------- | -------- | -------------------------- | ------------- |
| MARKETING | l3leaf | MARKETING-BDF-DS1 | 10.252.15.101/24 | 7050SX3 | Provisioned | - |
| MARKETING | l3leaf | MARKETING-BDF-DS2 | 10.252.15.102/24 | 7050SX3 | Provisioned | - |
| MARKETING | l2leaf | MARKETING-TX1-ES1 | 10.252.15.201/24 | 720XP | Provisioned | - |
| MARKETING | l2leaf | MARKETING-TX1-ES2 | 10.252.15.202/24 | 720XP | Provisioned | - |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

### Fabric Switches with inband Management IP

| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |
| MARKETING | l2leaf | MARKETING-TX1-ES1 | 10.252.1.198/26 | Vlan4092 |
| MARKETING | l2leaf | MARKETING-TX1-ES2 | 10.252.1.199/26 | Vlan4092 |

## Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| l3leaf | MARKETING-BDF-DS1 | Ethernet1 | l2leaf | MARKETING-TX1-ES1 | Ethernet49 |
| l3leaf | MARKETING-BDF-DS1 | Ethernet2 | l2leaf | MARKETING-TX1-ES2 | Ethernet49 |
| l3leaf | MARKETING-BDF-DS1 | Ethernet55/1 | mlag_peer | MARKETING-BDF-DS2 | Ethernet55/1 |
| l3leaf | MARKETING-BDF-DS1 | Ethernet56/1 | mlag_peer | MARKETING-BDF-DS2 | Ethernet56/1 |
| l3leaf | MARKETING-BDF-DS2 | Ethernet1 | l2leaf | MARKETING-TX1-ES1 | Ethernet50 |
| l3leaf | MARKETING-BDF-DS2 | Ethernet2 | l2leaf | MARKETING-TX1-ES2 | Ethernet50 |

## Fabric IP Allocation

### Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |

### Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |

### Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 10.252.1.64/26 | 64 | 2 | 3.13 % |

### Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| MARKETING | MARKETING-BDF-DS1 | 10.252.1.67/32 |
| MARKETING | MARKETING-BDF-DS2 | 10.252.1.68/32 |

### VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------------ | ------------------- | ------------------ | ------------------ |
| 10.252.1.128/26 | 64 | 2 | 3.13 % |

### VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
| MARKETING | MARKETING-BDF-DS1 | 10.252.1.131/32 |
| MARKETING | MARKETING-BDF-DS2 | 10.252.1.131/32 |
