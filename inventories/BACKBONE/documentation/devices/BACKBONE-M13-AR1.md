# BACKBONE-M13-AR1

## Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [DNS Domain](#dns-domain)
  - [IP Name Servers](#ip-name-servers)
  - [Clock Settings](#clock-settings)
  - [NTP](#ntp)
  - [Management SSH](#management-ssh)
  - [Management Console](#management-console)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
  - [Enable Password](#enable-password)
  - [Management defaults](#management-defaults)
  - [TACACS Servers](#tacacs-servers)
  - [IP TACACS Source Interfaces](#ip-tacacs-source-interfaces)
  - [AAA Server Groups](#aaa-server-groups)
  - [AAA Authentication](#aaa-authentication)
  - [AAA Authorization](#aaa-authorization)
  - [AAA Accounting](#aaa-accounting)
- [DHCP Relay](#dhcp-relay)
  - [DHCP Relay Summary](#dhcp-relay-summary)
  - [DHCP Relay Device Configuration](#dhcp-relay-device-configuration)
- [Monitoring](#monitoring)
  - [Logging](#logging)
  - [SNMP](#snmp)
  - [SFlow](#sflow)
- [Monitor Connectivity](#monitor-connectivity)
  - [Global Configuration](#global-configuration)
  - [Monitor Connectivity Device Configuration](#monitor-connectivity-device-configuration)
- [LLDP](#lldp)
  - [LLDP Summary](#lldp-summary)
  - [LLDP Device Configuration](#lldp-device-configuration)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Device Configuration](#internal-vlan-allocation-policy-device-configuration)
- [MAC Address Table](#mac-address-table)
  - [MAC Address Table Summary](#mac-address-table-summary)
  - [MAC Address Table Device Configuration](#mac-address-table-device-configuration)
- [Interfaces](#interfaces)
  - [Switchport Default](#switchport-default)
  - [Interface Defaults](#interface-defaults)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [ARP](#arp)
  - [Router BGP](#router-bgp)
- [Multicast](#multicast)
  - [Router Multicast](#router-multicast)
  - [PIM Sparse Mode](#pim-sparse-mode)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
- [ACL](#acl)
  - [IP Access-lists](#ip-access-lists)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [Errdisable](#errdisable)
  - [Errdisable Summary](#errdisable-summary)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | Description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management0 | OOB_MANAGEMENT | oob | MGMT | 172.16.1.23/24 | 172.16.1.1 |

##### IPv6

| Management Interface | Description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management0 | OOB_MANAGEMENT | oob | MGMT | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management0
   description OOB_MANAGEMENT
   no shutdown
   vrf MGMT
   ip address 172.16.1.23/24
   no lldp transmit
   no lldp receive
```

### DNS Domain

DNS domain: demo.com

#### DNS Domain Device Configuration

```eos
dns domain demo.com
!
```

### IP Name Servers

#### IP Name Servers Summary

| Name Server | VRF | Priority |
| ----------- | --- | -------- |
| 8.8.8.8 | MGMT | - |
| 208.67.222.222 | MGMT | - |

#### IP Name Servers Device Configuration

```eos
ip name-server vrf MGMT 8.8.8.8
ip name-server vrf MGMT 208.67.222.222
```

### Clock Settings

#### Clock Timezone Settings

Clock Timezone is set to **US/Eastern**.

#### Clock Device Configuration

```eos
!
clock timezone US/Eastern
```

### NTP

#### NTP Summary

##### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| pool.ntp.org | MGMT | - | - | True | - | - | - | - | - |
| time.google.com | MGMT | True | - | True | - | - | - | - | - |

#### NTP Device Configuration

```eos
!
ntp server vrf MGMT pool.ntp.org iburst
ntp server vrf MGMT time.google.com prefer iburst
```

### Management SSH

#### IPv4 ACL

| IPv4 ACL | VRF |
| -------- | --- |
| MGMT_V4_SSH | default |
| MGMT_V4_SSH | MGMT |
| MGMT_V4_SSH | campus |

#### SSH Timeout and Management

| Idle Timeout | SSH Management |
| ------------ | -------------- |
| 30 | Enabled |

#### Max number of SSH sessions limit and per-host limit

| Connection Limit | Max from a single Host |
| ---------------- | ---------------------- |
| 32 | 10 |

#### Ciphers and Algorithms

| Ciphers | Key-exchange methods | MAC algorithms | Hostkey server algorithms |
|---------|----------------------|----------------|---------------------------|
| default | default | default | default |

#### VRFs

| VRF | Status |
| --- | ------ |
| campus | Enabled |
| default | Enabled |
| MGMT | Enabled |

#### Management SSH Device Configuration

```eos
!
management ssh
   ip access-group MGMT_V4_SSH vrf campus in
   ip access-group MGMT_V4_SSH vrf default in
   ip access-group MGMT_V4_SSH vrf MGMT in
   idle-timeout 30
   connection per-host 10
   connection limit 32
   !
   vrf campus
      no shutdown
   !
   vrf default
      no shutdown
   !
   vrf MGMT
      no shutdown
```

### Management Console

#### Management Console Timeout

Management Console Timeout is set to **30** minutes.

#### Management Console Device Configuration

```eos
!
management console
   idle-timeout 30
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | Default Services |
| ---- | ----- | ---------------- |
| False | True | True |

#### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| campus | MGMT_V4_EAPI | - |
| default | MGMT_V4_EAPI | - |
| MGMT | MGMT_V4_EAPI | - |

#### Management API HTTP Device Configuration

```eos
!
management api http-commands
   protocol https
   no protocol http
   default-services
   no shutdown
   !
   vrf campus
      no shutdown
      ip access-group MGMT_V4_EAPI
   !
   vrf default
      no shutdown
      ip access-group MGMT_V4_EAPI
   !
   vrf MGMT
      no shutdown
      ip access-group MGMT_V4_EAPI
```

## Authentication

### Local Users

#### Local Users Summary

| User | Privilege | Role | Disabled | Shell |
| ---- | --------- | ---- | -------- | ----- |
| keeper | 15 | network-admin | False | - |
| networking_user | 15 | network-admin | False | - |
| test | 15 | network-admin | False | - |

#### Local Users Device Configuration

```eos
!
username keeper privilege 15 role network-admin secret sha512 <removed>
username networking_user privilege 15 role network-admin secret sha512 <removed>
username test privilege 15 role network-admin secret sha512 <removed>
```

### Enable Password

Enable password has been disabled

### Management defaults

Default secret hash is set to sha512

#### Management defaults Device Configuration

```eos
!
management defaults
  secret hash sha512
```

### TACACS Servers

#### TACACS Servers

| VRF | TACACS Servers | Single-Connection | Timeout |
| --- | -------------- | ----------------- | ------- |
| campus | 10.10.10.11 | False | - |
| campus | 10.10.10.12 | False | - |

Global timeout: 2 seconds

#### TACACS Servers Device Configuration

```eos
!
tacacs-server timeout 2
tacacs-server host 10.10.10.11 vrf campus key 0 <removed>
tacacs-server host 10.10.10.12 vrf campus key 0 <removed>
```

### IP TACACS Source Interfaces

#### IP TACACS Source Interfaces

| VRF | Source Interface Name |
| --- | --------------- |
| campus | vlan4092 |

#### IP TACACS Source Interfaces Device Configuration

```eos
!
ip tacacs vrf campus source-interface vlan4092
```

### AAA Server Groups

#### AAA Server Groups Summary

| Server Group Name | Type  | VRF | IP address |
| ------------------| ----- | --- | ---------- |
| DEMO-CPPM | tacacs+ | campus | 10.10.10.11 |
| DEMO-CPPM | tacacs+ | campus | 10.10.10.12 |

#### AAA Server Groups Device Configuration

```eos
!
aaa group server tacacs+ DEMO-CPPM
   server 10.10.10.11 vrf campus
   server 10.10.10.12 vrf campus
```

### AAA Authentication

#### AAA Authentication Summary

| Type | Sub-type | User Stores |
| ---- | -------- | ---------- |
| Login | default | local |
| Login | console | local |

#### AAA Authentication Device Configuration

```eos
aaa authentication login default local
aaa authentication login console local
!
```

### AAA Authorization

#### AAA Authorization Summary

| Type | User Stores |
| ---- | ----------- |
| Exec | local |

Authorization for configuration commands is disabled.

#### AAA Authorization Device Configuration

```eos
aaa authorization exec default local
!
```

### AAA Accounting

#### AAA Accounting Summary

| Type | Commands | Record type | Group | Logging |
| ---- | -------- | ----------- | ----- | ------- |
| Exec - Console | - | start-stop | - | True |
| Commands - Console | all | start-stop |  -  | True |
| Exec - Default | - | start-stop | DEMO-CPPM | True |
| System - Default | - | start-stop | DEMO-CPPM | - |
| Commands - Default | all | start-stop | DEMO-CPPM | True |

#### AAA Accounting Device Configuration

```eos
aaa accounting exec console start-stop logging
aaa accounting commands all console start-stop logging
aaa accounting exec default start-stop group DEMO-CPPM logging
aaa accounting system default start-stop group DEMO-CPPM
aaa accounting commands all default start-stop group DEMO-CPPM logging
```

## DHCP Relay

### DHCP Relay Summary

- DHCP Relay is disabled for tunnelled requests
- DHCP Relay is enabled for MLAG peer-link requests

### DHCP Relay Device Configuration

```eos
!
dhcp relay
   tunnel requests disabled
```

## Monitoring

### Logging

#### Logging Servers and Features Summary

| Type | Level |
| -----| ----- |
| Console | critical |
| Buffer | informational |
| Trap | debugging |

| Format Type | Setting |
| ----------- | ------- |
| Timestamp | high-resolution |
| Hostname | hostname |
| Sequence-numbers | true |
| RFC5424 | False |

| VRF | Source Interface |
| --- | ---------------- |
| campus | Vlan4092 |

| VRF | Hosts | Ports | Protocol |
| --- | ----- | ----- | -------- |
| campus | logs.demo.com | 512 | UDP |

#### Logging Servers and Features Device Configuration

```eos
!
logging buffered 512000 informational
logging trap debugging
logging console critical
logging vrf campus host logs.demo.com 512
logging format timestamp high-resolution
logging format sequence-numbers
logging vrf campus source-interface Vlan4092
```

### SNMP

#### SNMP Configuration Summary

| Contact | Location | SNMP Traps | State |
| ------- | -------- | ---------- | ----- |
| Network Engineering // 281.330.8004 | BACKBONE_FABRIC BACKBONE BACKBONE-M13-AR1 | All | Disabled |

#### SNMP EngineID Configuration

| Type | EngineID (Hex) | IP | Port |
| ---- | -------------- | -- | ---- |
| local | f4ba54fb9f57e664361c7a33bcee5b400153c50e | - | - |

#### SNMP ACLs

| IP | ACL | VRF |
| -- | --- | --- |
| IPv4 | MGMT_V4_SNMP | default |
| IPv4 | MGMT_V4_SNMP | MGMT |
| IPv4 | MGMT_V4_SNMP | campus |

#### SNMP Local Interfaces

| Local Interface | VRF |
| --------------- | --- |
| Vlan4092 | campus |

#### SNMP VRF Status

| VRF | Status |
| --- | ------ |
| campus | Enabled |

#### SNMP Views Configuration

| View | MIB Family Name | Status |
| ---- | --------------- | ------ |
| DEMO_VIEW_ALL | iso | Included |

#### SNMP Groups Configuration

| Group | SNMP Version | Authentication | Read | Write | Notify |
| ----- | ------------ | -------------- | ---- | ----- | ------ |
| DEMO_RO | v3 | priv | DEMO_VIEW_ALL | - | - |
| DEMO_RW | v3 | priv | - | DEMO_VIEW_ALL | - |

#### SNMP Users Configuration

| User | Group | Version | Authentication | Privacy | Remote Address | Remote Port | Engine ID |
| ---- | ----- | ------- | -------------- | ------- | -------------- | ----------- | --------- |
| demoV3 | DEMO_RO | v3 | sha | aes | - | - | f4ba54fb9f57e664361c7a33bcee5b400153c50e |
| demoRW | DEMO_RW | v3 | sha | aes | - | - | f4ba54fb9f57e664361c7a33bcee5b400153c50e |

#### SNMP Device Configuration

```eos
!
snmp-server ipv4 access-list MGMT_V4_SNMP vrf default
snmp-server ipv4 access-list MGMT_V4_SNMP vrf MGMT
snmp-server ipv4 access-list MGMT_V4_SNMP vrf campus
snmp-server engineID local f4ba54fb9f57e664361c7a33bcee5b400153c50e
snmp-server contact Network Engineering // 281.330.8004
snmp-server location BACKBONE_FABRIC BACKBONE BACKBONE-M13-AR1
snmp-server vrf campus local-interface Vlan4092
snmp-server view DEMO_VIEW_ALL iso included
snmp-server group DEMO_RO v3 priv read DEMO_VIEW_ALL
snmp-server group DEMO_RW v3 priv write DEMO_VIEW_ALL
snmp-server user demoRW DEMO_RW v3 localized f4ba54fb9f57e664361c7a33bcee5b400153c50e auth sha <removed> priv aes <removed>
snmp-server user demoV3 DEMO_RO v3 localized f4ba54fb9f57e664361c7a33bcee5b400153c50e auth sha <removed> priv aes <removed>
snmp-server vrf campus
```

### SFlow

#### SFlow Summary

| VRF | SFlow Source | SFlow Destination | Port |
| --- | ------------ | ----------------- | ---- |
| default | - | 127.0.0.1 | 6343 |
| default | Loopback2 | - | - |

sFlow Sample Rate: 2000

sFlow Polling Interval: 20

sFlow is enabled.

#### SFlow Device Configuration

```eos
!
sflow sample 2000
sflow polling-interval 20
sflow vrf default destination 127.0.0.1
sflow vrf default source-interface Loopback2
sflow run
```

## Monitor Connectivity

### Global Configuration

#### Probing Configuration

| Enabled | Interval | Default Interface Set | Address Only |
| ------- | -------- | --------------------- | ------------ |
| True | 5 | - | True |

#### Host Parameters

| Host Name | Description | IPv4 Address | Probing Interface Set | Address Only | URL |
| --------- | ----------- | ------------ | --------------------- | ------------ | --- |
| AWS-East-1 | - | 50.17.255.254 | - | True | - |
| Google | - | 8.8.8.8 | - | True | http://www.google.com |
| OpenDNS | - | 208.67.222.222 | - | True | - |

### Monitor Connectivity Device Configuration

```eos
!
monitor connectivity
   interval 5
   no shutdown
   !
   host AWS-East-1
      ip 50.17.255.254
   !
   host Google
      ip 8.8.8.8
      url http://www.google.com
   !
   host OpenDNS
      ip 208.67.222.222
```

## LLDP

### LLDP Summary

#### LLDP Global Settings

| Enabled | Management Address | Management VRF | Timer | Hold-Time | Re-initialization Timer | Drop Received Tagged Packets |
| ------- | ------------------ | -------------- | ----- | --------- | ----------------------- | ---------------------------- |
| True | Vlan4092 | campus | 30 | 120 | 2 | - |

#### LLDP Interface Settings

| Interface | Transmit | Receive |
| --------- | -------- | ------- |

### LLDP Device Configuration

```eos
!
lldp management-address Vlan4092
lldp management-address vrf campus
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **none**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode none
```

## Internal VLAN Allocation Policy

### Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

### Internal VLAN Allocation Policy Device Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

## MAC Address Table

### MAC Address Table Summary

- MAC address table entry maximum age: 1800 seconds

### MAC Address Table Device Configuration

```eos
!
mac address-table aging-time 1800
```

## Interfaces

### Switchport Default

#### Switchport Defaults Summary

- Default Switchport Mode: routed

#### Switchport Default Device Configuration

```eos
!
switchport default mode routed
```

### Interface Defaults

#### Interface Defaults Summary

- Default Routed Interface MTU: 1500

#### Interface Defaults Device Configuration

```eos
!
interface defaults
   mtu 1500
```

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet2 | P2P_FINANCE-BDF-DS2_Ethernet48 | - | 10.252.4.6/31 | default | 9214 | False | - | - |
| Ethernet3 | P2P_MARKETING-BDF-DS2_Ethernet48 | - | 10.252.4.10/31 | default | 9214 | False | - | - |
| Ethernet49/1 | P2P_BACKBONE-M11-CR1_Ethernet29/1 | - | 10.252.0.73/31 | default | 9214 | False | - | - |
| Ethernet50/1 | P2P_BACKBONE-M12-CR1_Ethernet29/1 | - | 10.252.0.75/31 | default | 9214 | False | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet2
   description P2P_FINANCE-BDF-DS2_Ethernet48
   no shutdown
   mtu 9214
   no switchport
   ip address 10.252.4.6/31
   pim ipv4 sparse-mode
!
interface Ethernet3
   description P2P_MARKETING-BDF-DS2_Ethernet48
   no shutdown
   mtu 9214
   no switchport
   ip address 10.252.4.10/31
   pim ipv4 sparse-mode
!
interface Ethernet49/1
   description P2P_BACKBONE-M11-CR1_Ethernet29/1
   no shutdown
   mtu 9214
   no switchport
   ip address 10.252.0.73/31
   pim ipv4 sparse-mode
!
interface Ethernet50/1
   description P2P_BACKBONE-M12-CR1_Ethernet29/1
   no shutdown
   mtu 9214
   no switchport
   ip address 10.252.0.75/31
   pim ipv4 sparse-mode
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | ROUTER_ID | default | 10.252.0.11/32 |
| Loopback2 | SFLOW SOURCE | default | 10.255.1.10/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | ROUTER_ID | default | - |
| Loopback2 | SFLOW SOURCE | default | - |

#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description ROUTER_ID
   no shutdown
   ip address 10.252.0.11/32
!
interface Loopback2
   description SFLOW SOURCE
   no shutdown
   ip address 10.255.1.10/32
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

### IP Routing

#### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| campus | True |
| MGMT | False |

#### IP Routing Device Configuration

```eos
!
ip routing
ip routing vrf campus
no ip routing vrf MGMT
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| campus | false |
| MGMT | false |

### Static Routes

#### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP | Exit interface | Administrative Distance | Tag | Route Name | Metric |
| --- | ------------------ | ----------- | -------------- | ----------------------- | --- | ---------- | ------ |
| MGMT | 0.0.0.0/0 | 172.16.1.1 | - | 1 | - | - | - |

#### Static Routes Device Configuration

```eos
!
ip route vrf MGMT 0.0.0.0/0 172.16.1.1
```

### ARP

Global ARP timeout: 1500

#### ARP Device Configuration

```eos
!
arp aging timeout default 1500
```

### Router BGP

ASN Notation: asdot

#### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65000.22 | 10.252.0.11 |

| BGP Tuning |
| ---------- |
| graceful-restart restart-time 300 |
| graceful-restart |
| no bgp default ipv4-unicast |
| distance bgp 20 200 200 |
| maximum-paths 4 ecmp 4 |

#### Router BGP Peer Groups

##### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive | TTL Max Hops |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- | ------------ |
| 10.252.0.72 | 65000.1 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.252.0.74 | 65000.1 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.252.4.7 | 65000.303 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.252.4.11 | 65000.302 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |

#### Router BGP Device Configuration

```eos
!
router bgp 65000.22
   bgp asn notation asdot
   router-id 10.252.0.11
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   graceful-restart restart-time 300
   graceful-restart
   maximum-paths 4 ecmp 4
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS password 7 <removed>
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor 10.252.0.72 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.252.0.72 remote-as 65000.1
   neighbor 10.252.0.72 description BACKBONE-M11-CR1_Ethernet29/1
   neighbor 10.252.0.74 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.252.0.74 remote-as 65000.1
   neighbor 10.252.0.74 description BACKBONE-M12-CR1_Ethernet29/1
   neighbor 10.252.4.7 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.252.4.7 remote-as 65000.303
   neighbor 10.252.4.7 description FINANCE-BDF-DS2
   neighbor 10.252.4.11 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.252.4.11 remote-as 65000.302
   neighbor 10.252.4.11 description MARKETING-BDF-DS2
   redistribute connected route-map RM-CONN-2-BGP
   !
   address-family ipv4
      neighbor IPv4-UNDERLAY-PEERS activate
```

## Multicast

### Router Multicast

#### IP Router Multicast Summary

- Routing for IPv4 multicast is enabled.

#### Router Multicast Device Configuration

```eos
!
router multicast
   ipv4
      routing
```

### PIM Sparse Mode

#### PIM Sparse Mode Enabled Interfaces

| Interface Name | VRF Name | IP Version | Border Router | DR Priority | Local Interface |
| -------------- | -------- | ---------- | ------------- | ----------- | --------------- |
| Ethernet2 | - | IPv4 | - | - | - |
| Ethernet3 | - | IPv4 | - | - | - |
| Ethernet49/1 | - | IPv4 | - | - | - |
| Ethernet50/1 | - | IPv4 | - | - | - |

## Filters

### Prefix-lists

#### Prefix-lists Summary

##### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.252.0.8/29 eq 32 |

#### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 10.252.0.8/29 eq 32
```

### Route-maps

#### Route-maps Summary

##### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |

#### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
```

## ACL

### IP Access-lists

#### IP Access-lists Device Configuration

```eos
!
ip access-list MGMT_V4_EAPI
   counters per-entry
   10 permit ip 172.16.1.0/24 any
   65000 deny ip any any log
!
ip access-list MGMT_V4_SNMP
   counters per-entry
   10 permit ip 172.16.1.0/24 any
   65000 deny ip any any log
!
ip access-list MGMT_V4_SSH
   counters per-entry
   10 permit ip 172.16.1.0/24 any
   65000 deny ip any any log
```

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| campus | enabled |
| MGMT | disabled |

### VRF Instances Device Configuration

```eos
!
vrf instance campus
!
vrf instance MGMT
```

## Errdisable

### Errdisable Summary

|  Detect Cause | Enabled |
| ------------- | ------- |
| acl | True |
| arp-inspection | True |

|  Detect Cause | Enabled | Interval |
| ------------- | ------- | -------- |
| bpduguard | True | 90 |
| link-flap | True | 90 |
| portsec | True | 90 |

```eos
!
errdisable detect cause acl
errdisable detect cause arp-inspection
errdisable recovery cause bpduguard
errdisable recovery cause link-flap
errdisable recovery cause portsec
errdisable recovery interval 90
```
