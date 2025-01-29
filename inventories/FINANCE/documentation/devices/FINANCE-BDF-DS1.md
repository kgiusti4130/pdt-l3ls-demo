# FINANCE-BDF-DS1

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
- [MLAG](#mlag)
  - [MLAG Summary](#mlag-summary)
  - [MLAG Device Configuration](#mlag-device-configuration)
- [LLDP](#lldp)
  - [LLDP Summary](#lldp-summary)
  - [LLDP Device Configuration](#lldp-device-configuration)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Device Configuration](#internal-vlan-allocation-policy-device-configuration)
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [MAC Address Table](#mac-address-table)
  - [MAC Address Table Summary](#mac-address-table-summary)
  - [MAC Address Table Device Configuration](#mac-address-table-device-configuration)
- [Interfaces](#interfaces)
  - [Interface Defaults](#interface-defaults)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Port-Channel Interfaces](#port-channel-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
  - [VLAN Interfaces](#vlan-interfaces)
  - [VXLAN Interface](#vxlan-interface)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [Virtual Router MAC Address](#virtual-router-mac-address)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [ARP](#arp)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
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
- [Virtual Source NAT](#virtual-source-nat)
  - [Virtual Source NAT Summary](#virtual-source-nat-summary)
  - [Virtual Source NAT Configuration](#virtual-source-nat-configuration)
- [Errdisable](#errdisable)
  - [Errdisable Summary](#errdisable-summary)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | Description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | OOB_MANAGEMENT | oob | MGMT | 10.252.15.101/24 | 172.16.1.1 |

##### IPv6

| Management Interface | Description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | OOB_MANAGEMENT | oob | MGMT | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management1
   description OOB_MANAGEMENT
   no shutdown
   vrf MGMT
   ip address 10.252.15.101/24
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
| Network Engineering // 281.330.8004 | FINANCE_FABRIC FINANCE FINANCE-BDF-DS1 | All | Disabled |

#### SNMP EngineID Configuration

| Type | EngineID (Hex) | IP | Port |
| ---- | -------------- | -- | ---- |
| local | 6f67f7a8dab131f004918f259e3d79d7a2f47fa2 | - | - |

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
| demoV3 | DEMO_RO | v3 | sha | aes | - | - | 6f67f7a8dab131f004918f259e3d79d7a2f47fa2 |
| demoRW | DEMO_RW | v3 | sha | aes | - | - | 6f67f7a8dab131f004918f259e3d79d7a2f47fa2 |

#### SNMP Device Configuration

```eos
!
snmp-server ipv4 access-list MGMT_V4_SNMP vrf default
snmp-server ipv4 access-list MGMT_V4_SNMP vrf MGMT
snmp-server ipv4 access-list MGMT_V4_SNMP vrf campus
snmp-server engineID local 6f67f7a8dab131f004918f259e3d79d7a2f47fa2
snmp-server contact Network Engineering // 281.330.8004
snmp-server location FINANCE_FABRIC FINANCE FINANCE-BDF-DS1
snmp-server vrf campus local-interface Vlan4092
snmp-server view DEMO_VIEW_ALL iso included
snmp-server group DEMO_RO v3 priv read DEMO_VIEW_ALL
snmp-server group DEMO_RW v3 priv write DEMO_VIEW_ALL
snmp-server user demoRW DEMO_RW v3 localized 6f67f7a8dab131f004918f259e3d79d7a2f47fa2 auth sha <removed> priv aes <removed>
snmp-server user demoV3 DEMO_RO v3 localized 6f67f7a8dab131f004918f259e3d79d7a2f47fa2 auth sha <removed> priv aes <removed>
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
| False | 5 | - | True |

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
   shutdown
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

## MLAG

### MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| FINANCE_L3_LEAF1 | Vlan4094 | 10.252.9.1 | Port-Channel551 |

Dual primary detection is disabled.

### MLAG Device Configuration

```eos
!
mlag configuration
   domain-id FINANCE_L3_LEAF1
   local-interface Vlan4094
   peer-address 10.252.9.1
   peer-link Port-Channel551
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

STP mode: **rapid-pvst**

#### Rapid-PVST Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 1-4094 | 8192 |

#### Global Spanning-Tree Settings

- Spanning Tree disabled for VLANs: **4093-4094**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode rapid-pvst
no spanning-tree vlan-id 4093-4094
spanning-tree vlan-id 1-4094 priority 8192
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

## VLANs

### VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 32 | FINANCE_end-user | - |
| 132 | FINANCE_VOICE | - |
| 232 | FINANCE_L2 | - |
| 250 | IOT | - |
| 3009 | MLAG_L3_VRF_campus | MLAG |
| 4092 | INBAND_MGMT | - |
| 4093 | MLAG_L3 | MLAG |
| 4094 | MLAG | MLAG |

### VLANs Device Configuration

```eos
!
vlan 32
   name FINANCE_end-user
!
vlan 132
   name FINANCE_VOICE
!
vlan 232
   name FINANCE_L2
!
vlan 250
   name IOT
!
vlan 3009
   name MLAG_L3_VRF_campus
   trunk group MLAG
!
vlan 4092
   name INBAND_MGMT
!
vlan 4093
   name MLAG_L3
   trunk group MLAG
!
vlan 4094
   name MLAG
   trunk group MLAG
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

### Interface Defaults

#### Interface Defaults Summary

- Default Routed Interface MTU: 9214

#### Interface Defaults Device Configuration

```eos
!
interface defaults
   mtu 9214
```

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet1 | L2_FINANCE-TX1-ES1_Ethernet49 | *trunk | *32,132,232,250,4092 | *- | *- | 1 |
| Ethernet2 | L2_FINANCE-TX1-ES2_Ethernet49 | *trunk | *32,132,232,250,4092 | *- | *- | 2 |
| Ethernet55/1 | MLAG_FINANCE-BDF-DS2_Ethernet55/1 | *trunk | *- | *- | *MLAG | 551 |
| Ethernet56/1 | MLAG_FINANCE-BDF-DS2_Ethernet56/1 | *trunk | *- | *- | *MLAG | 551 |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet48 | P2P_BACKBONE-M11-AR1_Ethernet2 | - | 10.252.4.5/31 | default | 1500 | False | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description L2_FINANCE-TX1-ES1_Ethernet49
   no shutdown
   channel-group 1 mode active
!
interface Ethernet2
   description L2_FINANCE-TX1-ES2_Ethernet49
   no shutdown
   channel-group 2 mode active
!
interface Ethernet48
   description P2P_BACKBONE-M11-AR1_Ethernet2
   no shutdown
   mtu 1500
   no switchport
   ip address 10.252.4.5/31
   pim ipv4 sparse-mode
!
interface Ethernet55/1
   description MLAG_FINANCE-BDF-DS2_Ethernet55/1
   no shutdown
   channel-group 551 mode active
!
interface Ethernet56/1
   description MLAG_FINANCE-BDF-DS2_Ethernet56/1
   no shutdown
   channel-group 551 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel1 | L2_FINANCE-TX1-ES1_Port-Channel49 | trunk | 32,132,232,250,4092 | - | - | - | - | 1 | - |
| Port-Channel2 | L2_FINANCE-TX1-ES2_Port-Channel49 | trunk | 32,132,232,250,4092 | - | - | - | - | 2 | - |
| Port-Channel551 | MLAG_FINANCE-BDF-DS2_Port-Channel551 | trunk | - | - | MLAG | - | - | - | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel1
   description L2_FINANCE-TX1-ES1_Port-Channel49
   no shutdown
   switchport trunk allowed vlan 32,132,232,250,4092
   switchport mode trunk
   switchport
   mlag 1
!
interface Port-Channel2
   description L2_FINANCE-TX1-ES2_Port-Channel49
   no shutdown
   switchport trunk allowed vlan 32,132,232,250,4092
   switchport mode trunk
   switchport
   mlag 2
!
interface Port-Channel551
   description MLAG_FINANCE-BDF-DS2_Port-Channel551
   no shutdown
   switchport mode trunk
   switchport trunk group MLAG
   switchport
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | ROUTER_ID | default | 10.252.9.67/32 |
| Loopback1 | VXLAN_TUNNEL_SOURCE | default | 10.252.9.131/32 |
| Loopback2 | SFLOW SOURCE | default | 10.255.1.10/32 |
| Loopback10 | DIAG_VRF_campus | campus | 10.252.97.35/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | ROUTER_ID | default | - |
| Loopback1 | VXLAN_TUNNEL_SOURCE | default | - |
| Loopback2 | SFLOW SOURCE | default | - |
| Loopback10 | DIAG_VRF_campus | campus | - |

#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description ROUTER_ID
   no shutdown
   ip address 10.252.9.67/32
!
interface Loopback1
   description VXLAN_TUNNEL_SOURCE
   no shutdown
   ip address 10.252.9.131/32
!
interface Loopback2
   description SFLOW SOURCE
   no shutdown
   ip address 10.255.1.10/32
!
interface Loopback10
   description DIAG_VRF_campus
   no shutdown
   vrf campus
   ip address 10.252.97.35/32
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan32 | FINANCE_end-user | campus | - | False |
| Vlan132 | FINANCE_VOICE | campus | - | False |
| Vlan3009 | MLAG_L3_VRF_campus | campus | 1500 | False |
| Vlan4092 | Inband Management | default | 1500 | False |
| Vlan4093 | MLAG_L3 | default | 1500 | False |
| Vlan4094 | MLAG | default | 1500 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ------ | ------- |
| Vlan32 |  campus  |  -  |  10.10.32.1/24  |  -  |  -  |  -  |
| Vlan132 |  campus  |  -  |  10.10.132.1/24  |  -  |  -  |  -  |
| Vlan3009 |  campus  |  10.252.9.2/31  |  -  |  -  |  -  |  -  |
| Vlan4092 |  default  |  10.252.9.194/26  |  -  |  10.252.9.193  |  -  |  -  |
| Vlan4093 |  default  |  10.252.9.2/31  |  -  |  -  |  -  |  -  |
| Vlan4094 |  default  |  10.252.9.0/31  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan32
   description FINANCE_end-user
   no shutdown
   vrf campus
   ip helper-address 10.10.10.14
   ip helper-address 10.10.10.15
   pim ipv4 sparse-mode
   pim ipv4 local-interface Loopback10
   ip address virtual 10.10.32.1/24
!
interface Vlan132
   description FINANCE_VOICE
   no shutdown
   vrf campus
   ip helper-address 10.10.10.14
   ip helper-address 10.10.10.15
   pim ipv4 sparse-mode
   pim ipv4 local-interface Loopback10
   ip address virtual 10.10.132.1/24
!
interface Vlan3009
   description MLAG_L3_VRF_campus
   no shutdown
   mtu 1500
   vrf campus
   ip address 10.252.9.2/31
!
interface Vlan4092
   description Inband Management
   no shutdown
   mtu 1500
   ip address 10.252.9.194/26
   ip attached-host route export 19
   ip virtual-router address 10.252.9.193
!
interface Vlan4093
   description MLAG_L3
   no shutdown
   mtu 1500
   ip address 10.252.9.2/31
   pim ipv4 sparse-mode
!
interface Vlan4094
   description MLAG
   no shutdown
   mtu 1500
   no autostate
   ip address 10.252.9.0/31
```

### VXLAN Interface

#### VXLAN Interface Summary

| Setting | Value |
| ------- | ----- |
| Source Interface | Loopback0 |
| MLAG Source Interface | Loopback1 |
| UDP port | 4789 |
| EVPN MLAG Shared Router MAC | mlag-system-id |

##### VLAN to VNI, Flood List and Multicast Group Mappings

| VLAN | VNI | Flood List | Multicast Group |
| ---- | --- | ---------- | --------------- |
| 32 | 10032 | - | - |
| 132 | 10132 | - | - |
| 232 | 10232 | - | - |
| 250 | 10250 | - | - |

##### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Multicast Group |
| ---- | --- | --------------- |
| campus | 10 | 232.0.0.11 |

#### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description FINANCE-BDF-DS1_VTEP
   vxlan source-interface Loopback0
   vxlan virtual-router encapsulation mac-address mlag-system-id
   vxlan udp-port 4789
   vxlan vlan 32 vni 10032
   vxlan vlan 132 vni 10132
   vxlan vlan 232 vni 10232
   vxlan vlan 250 vni 10250
   vxlan vrf campus vni 10
   vxlan mlag source-interface Loopback1
   vxlan vrf campus multicast group 232.0.0.11
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

### Virtual Router MAC Address

#### Virtual Router MAC Address Summary

Virtual Router MAC Address: aa:aa:bb:bb:cc:cc

#### Virtual Router MAC Address Device Configuration

```eos
!
ip virtual-router mac-address aa:aa:bb:bb:cc:cc
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
| 65000.999 | 10.252.9.67 |

| BGP Tuning |
| ---------- |
| graceful-restart restart-time 300 |
| graceful-restart |
| update wait-install |
| no bgp default ipv4-unicast |
| distance bgp 20 200 200 |
| maximum-paths 4 ecmp 4 |

#### Router BGP Peer Groups

##### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Source | Loopback0 |
| BFD | True |
| Ebgp multihop | 3 |
| Send community | all |
| Maximum routes | 0 (no limit) |

##### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

##### MLAG-IPv4-UNDERLAY-PEER

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Remote AS | 65000.999 |
| Next-hop self | True |
| Send community | all |
| Maximum routes | 12000 |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive | TTL Max Hops |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- | ------------ |
| 10.252.0.3 | 65000.1 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.252.0.4 | 65000.1 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.252.4.4 | 65000.11 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.252.9.3 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | default | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |
| 10.252.9.3 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | campus | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |

#### Router BGP EVPN Address Family

##### EVPN Peer Groups

| Peer Group | Activate | Route-map In | Route-map Out | Encapsulation |
| ---------- | -------- | ------------ | ------------- | ------------- |
| EVPN-OVERLAY-PEERS | True |  - | - | default |

#### Router BGP VLANs

| VLAN | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute |
| ---- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ |
| 32 | 10.252.9.67:10032 | 10032:10032 | - | - | learned |
| 132 | 10.252.9.67:10132 | 10132:10132 | - | - | learned |
| 232 | 10.252.9.67:10232 | 10232:10232 | - | - | learned |
| 250 | 10.252.9.67:10250 | 10250:10250 | - | - | learned |

#### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute | EVPN Multicast |
| --- | ------------------- | ------------ | -------------- |
| campus | 10.252.9.67:10 | connected | IPv4: True<br>Transit: False |

#### Router BGP Device Configuration

```eos
!
router bgp 65000.999
   bgp asn notation asdot
   router-id 10.252.9.67
   update wait-install
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   graceful-restart restart-time 300
   graceful-restart
   maximum-paths 4 ecmp 4
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS password 7 <removed>
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS password 7 <removed>
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER peer group
   neighbor MLAG-IPv4-UNDERLAY-PEER remote-as 65000.999
   neighbor MLAG-IPv4-UNDERLAY-PEER next-hop-self
   neighbor MLAG-IPv4-UNDERLAY-PEER description FINANCE-BDF-DS2
   neighbor MLAG-IPv4-UNDERLAY-PEER route-map RM-MLAG-PEER-IN in
   neighbor MLAG-IPv4-UNDERLAY-PEER password 7 <removed>
   neighbor MLAG-IPv4-UNDERLAY-PEER send-community
   neighbor MLAG-IPv4-UNDERLAY-PEER maximum-routes 12000
   neighbor 10.252.0.3 peer group EVPN-OVERLAY-PEERS
   neighbor 10.252.0.3 remote-as 65000.1
   neighbor 10.252.0.3 description BACKBONE-M11-CR1 Loopback
   neighbor 10.252.0.4 peer group EVPN-OVERLAY-PEERS
   neighbor 10.252.0.4 remote-as 65000.1
   neighbor 10.252.0.4 description BACKBONE-M12-CR1 Loopback
   neighbor 10.252.4.4 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.252.4.4 remote-as 65000.11
   neighbor 10.252.4.4 local-as 65000.303 no-prepend replace-as
   neighbor 10.252.4.4 description BACKBONE-M11-AR1
   neighbor 10.252.9.3 peer group MLAG-IPv4-UNDERLAY-PEER
   neighbor 10.252.9.3 description FINANCE-BDF-DS2_Vlan4093
   redistribute connected route-map RM-CONN-2-BGP
   redistribute attached-host
   !
   vlan 32
      rd 10.252.9.67:10032
      route-target both 10032:10032
      redistribute learned
   !
   vlan 132
      rd 10.252.9.67:10132
      route-target both 10132:10132
      redistribute learned
   !
   vlan 232
      rd 10.252.9.67:10232
      route-target both 10232:10232
      redistribute learned
   !
   vlan 250
      rd 10.252.9.67:10250
      route-target both 10250:10250
      redistribute learned
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
      neighbor MLAG-IPv4-UNDERLAY-PEER activate
   !
   vrf campus
      rd 10.252.9.67:10
      route-target import evpn 10:10
      route-target export evpn 10:10
      router-id 10.252.9.67
      update wait-install
      neighbor 10.252.9.3 peer group MLAG-IPv4-UNDERLAY-PEER
      neighbor 10.252.9.3 description FINANCE-BDF-DS2_Vlan3009
      redistribute connected route-map RM-CONN-2-BGP-VRFS
      evpn multicast
```

## BFD

### Router BFD

#### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 300 | 300 | 3 |

#### Router BFD Device Configuration

```eos
!
router bfd
   multihop interval 300 min-rx 300 multiplier 3
```

## Multicast

### IP IGMP Snooping

#### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

#### IP IGMP Snooping Device Configuration

```eos
```

### Router Multicast

#### IP Router Multicast Summary

- Routing for IPv4 multicast is enabled.
- Software forwarding by the Software Forwarding Engine (SFE)

#### IP Router Multicast VRFs

| VRF Name | Multicast Routing |
| -------- | ----------------- |
| campus | enabled |

#### Router Multicast Device Configuration

```eos
!
router multicast
   ipv4
      routing
      software-forwarding sfe
   !
   vrf campus
      ipv4
         routing
```

### PIM Sparse Mode

#### PIM Sparse Mode Enabled Interfaces

| Interface Name | VRF Name | IP Version | Border Router | DR Priority | Local Interface |
| -------------- | -------- | ---------- | ------------- | ----------- | --------------- |
| Ethernet48 | - | IPv4 | - | - | - |
| Vlan32 | campus | IPv4 | - | - | Loopback10 |
| Vlan132 | campus | IPv4 | - | - | Loopback10 |
| Vlan4093 | - | IPv4 | - | - | - |

## Filters

### Prefix-lists

#### Prefix-lists Summary

##### PL-L2LEAF-INBAND-MGMT

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.252.9.192/26 |

##### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.252.9.64/26 eq 32 |
| 20 | permit 10.252.9.128/26 eq 32 |

##### PL-MLAG-PEER-VRFS

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.252.9.2/31 |

#### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-L2LEAF-INBAND-MGMT
   seq 10 permit 10.252.9.192/26
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 10.252.9.64/26 eq 32
   seq 20 permit 10.252.9.128/26 eq 32
!
ip prefix-list PL-MLAG-PEER-VRFS
   seq 10 permit 10.252.9.2/31
```

### Route-maps

#### Route-maps Summary

##### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |
| 20 | permit | ip address prefix-list PL-L2LEAF-INBAND-MGMT | - | - | - |

##### RM-CONN-2-BGP-VRFS

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | deny | ip address prefix-list PL-MLAG-PEER-VRFS | - | - | - |
| 20 | permit | - | - | - | - |

##### RM-MLAG-PEER-IN

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | - | origin incomplete | - | - |

#### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
!
route-map RM-CONN-2-BGP permit 20
   match ip address prefix-list PL-L2LEAF-INBAND-MGMT
!
route-map RM-CONN-2-BGP-VRFS deny 10
   match ip address prefix-list PL-MLAG-PEER-VRFS
!
route-map RM-CONN-2-BGP-VRFS permit 20
!
route-map RM-MLAG-PEER-IN permit 10
   description Make routes learned over MLAG Peer-link less preferred on spines to ensure optimal routing
   set origin incomplete
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

## Virtual Source NAT

### Virtual Source NAT Summary

| Source NAT VRF | Source NAT IPv4 Address | Source NAT IPv6 Address |
| -------------- | ----------------------- | ----------------------- |
| campus | 10.252.97.35 | - |

### Virtual Source NAT Configuration

```eos
!
ip address virtual source-nat vrf campus address 10.252.97.35
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
