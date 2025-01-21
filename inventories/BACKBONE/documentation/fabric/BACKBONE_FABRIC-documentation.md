# BACKBONE_FABRIC

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
| BACKBONE | spine | BACKBONE-M11-AR1 | 172.16.1.21/24 | cEOS-lab | Provisioned | - |
| BACKBONE | l3leaf | BACKBONE-M11-BL1 | 172.16.1.31/24 | cEOS-lab | Provisioned | - |
| BACKBONE | super-spine | BACKBONE-M11-CR1 | 172.16.1.11/24 | cEOS-lab | Provisioned | - |
| BACKBONE | spine | BACKBONE-M12-AR1 | 172.16.1.22/24 | cEOS-lab | Not Available | - |
| BACKBONE | l3leaf | BACKBONE-M12-BL1 | 172.16.1.32/24 | cEOS-lab | Provisioned | - |
| BACKBONE | super-spine | BACKBONE-M12-CR1 | 172.16.1.12/24 | cEOS-lab | Provisioned | - |
| BACKBONE | spine | BACKBONE-M13-AR1 | 172.16.1.23/24 | cEOS-lab | Provisioned | - |
| BACKBONE | spine | BACKBONE-M14-AR1 | 172.16.1.24/24 | cEOS-lab | Not Available | - |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

### Fabric Switches with inband Management IP

| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

## Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| spine | BACKBONE-M11-AR1 | Ethernet49/1 | super-spine | BACKBONE-M11-CR1 | Ethernet27/1 |
| spine | BACKBONE-M11-AR1 | Ethernet50/1 | super-spine | BACKBONE-M12-CR1 | Ethernet27/1 |
| l3leaf | BACKBONE-M11-BL1 | Ethernet49/1 | super-spine | BACKBONE-M11-CR1 | Ethernet1/1 |
| l3leaf | BACKBONE-M11-BL1 | Ethernet50/1 | super-spine | BACKBONE-M12-CR1 | Ethernet1/1 |
| l3leaf | BACKBONE-M11-BL1 | Ethernet55/1 | mlag_peer | BACKBONE-M12-BL1 | Ethernet55/1 |
| l3leaf | BACKBONE-M11-BL1 | Ethernet56/1 | mlag_peer | BACKBONE-M12-BL1 | Ethernet56/1 |
| super-spine | BACKBONE-M11-CR1 | Ethernet2/1 | l3leaf | BACKBONE-M12-BL1 | Ethernet49/1 |
| super-spine | BACKBONE-M11-CR1 | Ethernet28/1 | spine | BACKBONE-M12-AR1 | Ethernet49/1 |
| super-spine | BACKBONE-M11-CR1 | Ethernet29/1 | spine | BACKBONE-M13-AR1 | Ethernet49/1 |
| super-spine | BACKBONE-M11-CR1 | Ethernet30/1 | spine | BACKBONE-M14-AR1 | Ethernet49/1 |
| spine | BACKBONE-M12-AR1 | Ethernet50/1 | super-spine | BACKBONE-M12-CR1 | Ethernet28/1 |
| l3leaf | BACKBONE-M12-BL1 | Ethernet50/1 | super-spine | BACKBONE-M12-CR1 | Ethernet2/1 |
| super-spine | BACKBONE-M12-CR1 | Ethernet29/1 | spine | BACKBONE-M13-AR1 | Ethernet50/1 |
| super-spine | BACKBONE-M12-CR1 | Ethernet30/1 | spine | BACKBONE-M14-AR1 | Ethernet50/1 |

## Fabric IP Allocation

### Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 10.252.0.64/26 | 64 | 16 | 25.0 % |
| 10.252.1.0/24 | 256 | 8 | 3.13 % |

### Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |
| BACKBONE-M11-AR1 | Ethernet49/1 | 10.252.0.65/31 | BACKBONE-M11-CR1 | Ethernet27/1 | 10.252.0.64/31 |
| BACKBONE-M11-AR1 | Ethernet50/1 | 10.252.0.67/31 | BACKBONE-M12-CR1 | Ethernet27/1 | 10.252.0.66/31 |
| BACKBONE-M11-BL1 | Ethernet49/1 | 10.252.1.1/31 | BACKBONE-M11-CR1 | Ethernet1/1 | 10.252.1.0/31 |
| BACKBONE-M11-BL1 | Ethernet50/1 | 10.252.1.3/31 | BACKBONE-M12-CR1 | Ethernet1/1 | 10.252.1.2/31 |
| BACKBONE-M11-CR1 | Ethernet2/1 | 10.252.1.4/31 | BACKBONE-M12-BL1 | Ethernet49/1 | 10.252.1.5/31 |
| BACKBONE-M11-CR1 | Ethernet28/1 | 10.252.0.68/31 | BACKBONE-M12-AR1 | Ethernet49/1 | 10.252.0.69/31 |
| BACKBONE-M11-CR1 | Ethernet29/1 | 10.252.0.72/31 | BACKBONE-M13-AR1 | Ethernet49/1 | 10.252.0.73/31 |
| BACKBONE-M11-CR1 | Ethernet30/1 | 10.252.0.76/31 | BACKBONE-M14-AR1 | Ethernet49/1 | 10.252.0.77/31 |
| BACKBONE-M12-AR1 | Ethernet50/1 | 10.252.0.71/31 | BACKBONE-M12-CR1 | Ethernet28/1 | 10.252.0.70/31 |
| BACKBONE-M12-BL1 | Ethernet50/1 | 10.252.1.7/31 | BACKBONE-M12-CR1 | Ethernet2/1 | 10.252.1.6/31 |
| BACKBONE-M12-CR1 | Ethernet29/1 | 10.252.0.74/31 | BACKBONE-M13-AR1 | Ethernet50/1 | 10.252.0.75/31 |
| BACKBONE-M12-CR1 | Ethernet30/1 | 10.252.0.78/31 | BACKBONE-M14-AR1 | Ethernet50/1 | 10.252.0.79/31 |

### Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 10.252.0.0/29 | 8 | 2 | 25.0 % |
| 10.252.0.8/29 | 8 | 4 | 50.0 % |
| 10.252.0.16/28 | 16 | 2 | 12.5 % |

### Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| BACKBONE | BACKBONE-M11-AR1 | 10.252.0.9/32 |
| BACKBONE | BACKBONE-M11-BL1 | 10.252.0.21/32 |
| BACKBONE | BACKBONE-M11-CR1 | 10.252.0.3/32 |
| BACKBONE | BACKBONE-M12-AR1 | 10.252.0.10/32 |
| BACKBONE | BACKBONE-M12-BL1 | 10.252.0.22/32 |
| BACKBONE | BACKBONE-M12-CR1 | 10.252.0.4/32 |
| BACKBONE | BACKBONE-M13-AR1 | 10.252.0.11/32 |
| BACKBONE | BACKBONE-M14-AR1 | 10.252.0.12/32 |

### VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------------ | ------------------- | ------------------ | ------------------ |
| 10.252.0.32/29 | 8 | 2 | 25.0 % |

### VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
| BACKBONE | BACKBONE-M11-BL1 | 10.252.0.37/32 |
| BACKBONE | BACKBONE-M12-BL1 | 10.252.0.37/32 |
