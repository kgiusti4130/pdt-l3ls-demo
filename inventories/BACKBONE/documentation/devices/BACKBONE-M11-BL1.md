# BACKBONE-M11-BL1

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
| Management0 | OOB_MANAGEMENT | oob | MGMT | 172.16.1.31/24 | 172.16.1.1 |

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
   ip address 172.16.1.31/24
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
| Network Engineering // 281.330.8004 | BACKBONE_FABRIC BACKBONE BACKBONE-M11-BL1 | All | Disabled |

#### SNMP EngineID Configuration

| Type | EngineID (Hex) | IP | Port |
| ---- | -------------- | -- | ---- |
| local | 335cf7786127d51f782795df3a14603ace94a08e | - | - |

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
| demoV3 | DEMO_RO | v3 | sha | aes | - | - | 335cf7786127d51f782795df3a14603ace94a08e |
| demoRW | DEMO_RW | v3 | sha | aes | - | - | 335cf7786127d51f782795df3a14603ace94a08e |

#### SNMP Device Configuration

```eos
!
snmp-server ipv4 access-list MGMT_V4_SNMP vrf default
snmp-server ipv4 access-list MGMT_V4_SNMP vrf MGMT
snmp-server ipv4 access-list MGMT_V4_SNMP vrf campus
snmp-server engineID local 335cf7786127d51f782795df3a14603ace94a08e
snmp-server contact Network Engineering // 281.330.8004
snmp-server location BACKBONE_FABRIC BACKBONE BACKBONE-M11-BL1
snmp-server vrf campus local-interface Vlan4092
snmp-server view DEMO_VIEW_ALL iso included
snmp-server group DEMO_RO v3 priv read DEMO_VIEW_ALL
snmp-server group DEMO_RW v3 priv write DEMO_VIEW_ALL
snmp-server user demoRW DEMO_RW v3 localized 335cf7786127d51f782795df3a14603ace94a08e auth sha <removed> priv aes <removed>
snmp-server user demoV3 DEMO_RO v3 localized 335cf7786127d51f782795df3a14603ace94a08e auth sha <removed> priv aes <removed>
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
| BACKBONE-BL-GROUP | Vlan4094 | 10.252.2.1 | Port-Channel551 |

Dual primary detection is disabled.

### MLAG Device Configuration

```eos
!
mlag configuration
   domain-id BACKBONE-BL-GROUP
   local-interface Vlan4094
   peer-address 10.252.2.1
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
| 1-4094 | 40960 |

#### Global Spanning-Tree Settings

- Spanning Tree disabled for VLANs: **4093-4094**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode rapid-pvst
no spanning-tree vlan-id 4093-4094
spanning-tree vlan-id 1-4094 priority 40960
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
| 22 | ENGINEERING_end-user | - |
| 122 | ENGINEERING_VOICE | - |
| 222 | ENGINEERING_L2 | - |
| 250 | IOT | - |
| 333 | IOT | - |
| 3009 | MLAG_L3_VRF_campus | MLAG |
| 3010 | MLAG_L3_VRF_IOT | MLAG |
| 4092 | BORDER-LEAFS | - |
| 4093 | MLAG_L3 | MLAG |
| 4094 | MLAG | MLAG |

### VLANs Device Configuration

```eos
!
vlan 22
   name ENGINEERING_end-user
!
vlan 122
   name ENGINEERING_VOICE
!
vlan 222
   name ENGINEERING_L2
!
vlan 250
   name IOT
!
vlan 333
   name IOT
!
vlan 3009
   name MLAG_L3_VRF_campus
   trunk group MLAG
!
vlan 3010
   name MLAG_L3_VRF_IOT
   trunk group MLAG
!
vlan 4092
   name BORDER-LEAFS
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
| Ethernet10 | DEMO User Port | *access | *22 | *- | *- | 10 |
| Ethernet11 | IOT DEVICE | *access | *333 | *- | *- | 11 |
| Ethernet55/1 | MLAG_BACKBONE-M12-BL1_Ethernet55/1 | *trunk | *- | *- | *MLAG | 551 |
| Ethernet56/1 | MLAG_BACKBONE-M12-BL1_Ethernet56/1 | *trunk | *- | *- | *MLAG | 551 |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet47 | P2P_CORE-LEGACY_Te1/1/1 | - | 10.252.6.1/31 | campus | 1500 | False | - | - |
| Ethernet48 | P2P_CORE-LEGACY_Te2/1/1 | - | 10.252.6.3/31 | campus | 1500 | False | - | - |
| Ethernet49/1 | P2P_BACKBONE-M11-CR1_Ethernet1/1 | - | 10.252.1.1/31 | default | 9214 | False | - | - |
| Ethernet50/1 | P2P_BACKBONE-M12-CR1_Ethernet1/1 | - | 10.252.1.3/31 | default | 9214 | False | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet10
   description DEMO User Port
   no shutdown
   channel-group 10 mode active
!
interface Ethernet11
   description IOT DEVICE
   no shutdown
   channel-group 11 mode active
!
interface Ethernet47
   description P2P_CORE-LEGACY_Te1/1/1
   no shutdown
   mtu 1500
   no switchport
   vrf campus
   ip address 10.252.6.1/31
   ip ospf network point-to-point
   pim ipv4 sparse-mode
   ip igmp version 2
!
interface Ethernet48
   description P2P_CORE-LEGACY_Te2/1/1
   no shutdown
   mtu 1500
   no switchport
   vrf campus
   ip address 10.252.6.3/31
   ip ospf network point-to-point
   pim ipv4 sparse-mode
   ip igmp version 2
!
interface Ethernet49/1
   description P2P_BACKBONE-M11-CR1_Ethernet1/1
   no shutdown
   mtu 9214
   no switchport
   ip address 10.252.1.1/31
   pim ipv4 sparse-mode
!
interface Ethernet50/1
   description P2P_BACKBONE-M12-CR1_Ethernet1/1
   no shutdown
   mtu 9214
   no switchport
   ip address 10.252.1.3/31
   pim ipv4 sparse-mode
!
interface Ethernet55/1
   description MLAG_BACKBONE-M12-BL1_Ethernet55/1
   no shutdown
   channel-group 551 mode active
!
interface Ethernet56/1
   description MLAG_BACKBONE-M12-BL1_Ethernet56/1
   no shutdown
   channel-group 551 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel10 | SERVER_host1 | access | 22 | - | - | - | - | 10 | - |
| Port-Channel11 | SERVER_host2 | access | 333 | - | - | - | - | 11 | - |
| Port-Channel551 | MLAG_BACKBONE-M12-BL1_Port-Channel551 | trunk | - | - | MLAG | - | - | - | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel10
   description SERVER_host1
   no shutdown
   switchport access vlan 22
   switchport mode access
   switchport
   mlag 10
!
interface Port-Channel11
   description SERVER_host2
   no shutdown
   switchport access vlan 333
   switchport mode access
   switchport
   mlag 11
   spanning-tree portfast
   spanning-tree bpduguard enable
!
interface Port-Channel551
   description MLAG_BACKBONE-M12-BL1_Port-Channel551
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
| Loopback0 | ROUTER_ID | default | 10.252.0.21/32 |
| Loopback1 | VXLAN_TUNNEL_SOURCE | default | 10.252.0.37/32 |
| Loopback2 | SFLOW SOURCE | default | 10.255.1.10/32 |
| Loopback10 | DIAG_VRF_campus | campus | 10.252.2.37/32 |
| Loopback11 | DIAG_VRF_IOT | IOT | 10.253.2.37/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | ROUTER_ID | default | - |
| Loopback1 | VXLAN_TUNNEL_SOURCE | default | - |
| Loopback2 | SFLOW SOURCE | default | - |
| Loopback10 | DIAG_VRF_campus | campus | - |
| Loopback11 | DIAG_VRF_IOT | IOT | - |

#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description ROUTER_ID
   no shutdown
   ip address 10.252.0.21/32
!
interface Loopback1
   description VXLAN_TUNNEL_SOURCE
   no shutdown
   ip address 10.252.0.37/32
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
   ip address 10.252.2.37/32
!
interface Loopback11
   description DIAG_VRF_IOT
   no shutdown
   vrf IOT
   ip address 10.253.2.37/32
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan22 | ENGINEERING_end-user | campus | - | False |
| Vlan122 | ENGINEERING_VOICE | campus | - | False |
| Vlan333 | IOT | IOT | - | False |
| Vlan3009 | MLAG_L3_VRF_campus | campus | 9214 | False |
| Vlan3010 | MLAG_L3_VRF_IOT | IOT | 9214 | False |
| Vlan4092 | BORDER-LEAFS | IOT | - | True |
| Vlan4093 | MLAG_L3 | default | 9214 | False |
| Vlan4094 | MLAG | default | 9214 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ------ | ------- |
| Vlan22 |  campus  |  -  |  10.10.22.1/24  |  -  |  -  |  -  |
| Vlan122 |  campus  |  -  |  10.10.122.1/24  |  -  |  -  |  -  |
| Vlan333 |  IOT  |  -  |  10.10.222.1/24  |  -  |  -  |  -  |
| Vlan3009 |  campus  |  10.252.2.2/31  |  -  |  -  |  -  |  -  |
| Vlan3010 |  IOT  |  10.252.2.2/31  |  -  |  -  |  -  |  -  |
| Vlan4092 |  IOT  |  -  |  -  |  -  |  -  |  -  |
| Vlan4093 |  default  |  10.252.2.2/31  |  -  |  -  |  -  |  -  |
| Vlan4094 |  default  |  10.252.2.0/31  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan22
   description ENGINEERING_end-user
   no shutdown
   vrf campus
   ip helper-address 10.10.10.14
   ip helper-address 10.10.10.15
   pim ipv4 sparse-mode
   pim ipv4 local-interface Loopback10
   ip address virtual 10.10.22.1/24
!
interface Vlan122
   description ENGINEERING_VOICE
   no shutdown
   vrf campus
   ip helper-address 10.10.10.14
   ip helper-address 10.10.10.15
   pim ipv4 sparse-mode
   pim ipv4 local-interface Loopback10
   ip address virtual 10.10.122.1/24
!
interface Vlan333
   description IOT
   no shutdown
   vrf IOT
   pim ipv4 sparse-mode
   pim ipv4 local-interface Loopback11
   ip address virtual 10.10.222.1/24
!
interface Vlan3009
   description MLAG_L3_VRF_campus
   no shutdown
   mtu 9214
   vrf campus
   ip address 10.252.2.2/31
!
interface Vlan3010
   description MLAG_L3_VRF_IOT
   no shutdown
   mtu 9214
   vrf IOT
   ip address 10.252.2.2/31
!
interface Vlan4092
   description BORDER-LEAFS
   shutdown
   vrf IOT
   pim ipv4 sparse-mode
   pim ipv4 local-interface Loopback11
!
interface Vlan4093
   description MLAG_L3
   no shutdown
   mtu 9214
   ip address 10.252.2.2/31
   pim ipv4 sparse-mode
!
interface Vlan4094
   description MLAG
   no shutdown
   mtu 9214
   no autostate
   ip address 10.252.2.0/31
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
| 22 | 10022 | - | - |
| 122 | 10122 | - | - |
| 222 | 10222 | - | - |
| 250 | 10250 | - | - |
| 333 | 10333 | - | - |
| 4092 | 14092 | - | - |

##### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Multicast Group |
| ---- | --- | --------------- |
| campus | 10 | 232.0.0.11 |
| IOT | 11 | 232.0.0.12 |

#### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description BACKBONE-M11-BL1_VTEP
   vxlan source-interface Loopback0
   vxlan virtual-router encapsulation mac-address mlag-system-id
   vxlan udp-port 4789
   vxlan vlan 22 vni 10022
   vxlan vlan 122 vni 10122
   vxlan vlan 222 vni 10222
   vxlan vlan 250 vni 10250
   vxlan vlan 333 vni 10333
   vxlan vlan 4092 vni 14092
   vxlan vrf campus vni 10
   vxlan vrf IOT vni 11
   vxlan mlag source-interface Loopback1
   vxlan vrf campus multicast group 232.0.0.11
   vxlan vrf IOT multicast group 232.0.0.12
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
| IOT | True |
| MGMT | False |

#### IP Routing Device Configuration

```eos
!
ip routing
ip routing vrf campus
ip routing vrf IOT
no ip routing vrf MGMT
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| campus | false |
| IOT | false |
| MGMT | false |

### Static Routes

#### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP | Exit interface | Administrative Distance | Tag | Route Name | Metric |
| --- | ------------------ | ----------- | -------------- | ----------------------- | --- | ---------- | ------ |
| MGMT | 0.0.0.0/0 | 172.16.1.1 | - | 1 | - | - | - |
| campus | 10.0.0.0/8 | Null0 | - | 1 | - | - | - |
| campus | 172.16.0.0/12 | Null0 | - | 1 | - | - | - |
| campus | 192.168.0.0/16 | Null0 | - | 1 | - | - | - |
| campus | 0.0.0.0/0 | Null0 | - | 1 | - | - | - |

#### Static Routes Device Configuration

```eos
!
ip route vrf campus 0.0.0.0/0 Null0
ip route vrf campus 10.0.0.0/8 Null0
ip route vrf campus 172.16.0.0/12 Null0
ip route vrf campus 192.168.0.0/16 Null0
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
| 65000.2 | 10.252.0.21 |

| BGP Tuning |
| ---------- |
| graceful-restart restart-time 300 |
| graceful-restart |
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
| Remote AS | 65000.2 |
| Next-hop self | True |
| Send community | all |
| Maximum routes | 12000 |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive | TTL Max Hops |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- | ------------ |
| 10.252.0.3 | 65000.1 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.252.0.4 | 65000.1 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.252.1.0 | 65000.1 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.252.1.2 | 65000.1 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.252.2.3 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | default | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |
| 10.252.2.3 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | campus | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |
| 10.252.2.3 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | IOT | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |

#### Router BGP EVPN Address Family

##### EVPN Peer Groups

| Peer Group | Activate | Route-map In | Route-map Out | Encapsulation |
| ---------- | -------- | ------------ | ------------- | ------------- |
| EVPN-OVERLAY-PEERS | True |  - | - | default |

#### Router BGP VLANs

| VLAN | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute |
| ---- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ |
| 22 | 10.252.0.21:10022 | 10022:10022 | - | - | learned |
| 122 | 10.252.0.21:10122 | 10122:10122 | - | - | learned |
| 222 | 10.252.0.21:10222 | 10222:10222 | - | - | learned |
| 250 | 10.252.0.21:10250 | 10250:10250 | - | - | learned |
| 333 | 10.252.0.21:10333 | 10333:10333 | - | - | learned |
| 4092 | 10.252.0.21:14092 | 14092:14092 | - | - | learned |

#### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute | EVPN Multicast |
| --- | ------------------- | ------------ | -------------- |
| campus | 10.252.0.21:10 | connected | IPv4: True<br>Transit: False |
| IOT | 10.252.0.21:11 | connected | IPv4: True<br>Transit: False |

#### Router BGP Device Configuration

```eos
!
router bgp 65000.2
   bgp asn notation asdot
   router-id 10.252.0.21
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
   neighbor MLAG-IPv4-UNDERLAY-PEER remote-as 65000.2
   neighbor MLAG-IPv4-UNDERLAY-PEER next-hop-self
   neighbor MLAG-IPv4-UNDERLAY-PEER description BACKBONE-M12-BL1
   neighbor MLAG-IPv4-UNDERLAY-PEER route-map RM-MLAG-PEER-IN in
   neighbor MLAG-IPv4-UNDERLAY-PEER password 7 <removed>
   neighbor MLAG-IPv4-UNDERLAY-PEER send-community
   neighbor MLAG-IPv4-UNDERLAY-PEER maximum-routes 12000
   neighbor 10.252.0.3 peer group EVPN-OVERLAY-PEERS
   neighbor 10.252.0.3 remote-as 65000.1
   neighbor 10.252.0.3 description BACKBONE-M11-CR1_Loopback0
   neighbor 10.252.0.4 peer group EVPN-OVERLAY-PEERS
   neighbor 10.252.0.4 remote-as 65000.1
   neighbor 10.252.0.4 description BACKBONE-M12-CR1_Loopback0
   neighbor 10.252.1.0 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.252.1.0 remote-as 65000.1
   neighbor 10.252.1.0 description BACKBONE-M11-CR1_Ethernet1/1
   neighbor 10.252.1.2 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.252.1.2 remote-as 65000.1
   neighbor 10.252.1.2 description BACKBONE-M12-CR1_Ethernet1/1
   neighbor 10.252.2.3 peer group MLAG-IPv4-UNDERLAY-PEER
   neighbor 10.252.2.3 description BACKBONE-M12-BL1_Vlan4093
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan 22
      rd 10.252.0.21:10022
      route-target both 10022:10022
      redistribute learned
   !
   vlan 122
      rd 10.252.0.21:10122
      route-target both 10122:10122
      redistribute learned
   !
   vlan 222
      rd 10.252.0.21:10222
      route-target both 10222:10222
      redistribute learned
   !
   vlan 250
      rd 10.252.0.21:10250
      route-target both 10250:10250
      redistribute learned
   !
   vlan 333
      rd 10.252.0.21:10333
      route-target both 10333:10333
      redistribute learned
   !
   vlan 4092
      rd 10.252.0.21:14092
      route-target both 14092:14092
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
      rd 10.252.0.21:10
      route-target import evpn 10:10
      route-target export evpn 10:10
      router-id 10.252.0.21
      neighbor 10.252.2.3 peer group MLAG-IPv4-UNDERLAY-PEER
      neighbor 10.252.2.3 description BACKBONE-M12-BL1_Vlan3009
      network 0.0.0.0/0
      network 10.0.0.0/8
      network 172.16.0.0/12
      network 192.168.0.0/16
      redistribute connected route-map RM-CONN-2-BGP-VRFS
      evpn multicast
   !
   vrf IOT
      rd 10.252.0.21:11
      route-target import evpn 11:11
      route-target export evpn 11:11
      router-id 10.252.0.21
      neighbor 10.252.2.3 peer group MLAG-IPv4-UNDERLAY-PEER
      neighbor 10.252.2.3 description BACKBONE-M12-BL1_Vlan3010
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
| IOT | enabled |

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
   !
   vrf IOT
      ipv4
         routing
```

### PIM Sparse Mode

#### PIM Sparse Mode Enabled Interfaces

| Interface Name | VRF Name | IP Version | Border Router | DR Priority | Local Interface |
| -------------- | -------- | ---------- | ------------- | ----------- | --------------- |
| Ethernet47 | campus | IPv4 | - | - | - |
| Ethernet48 | campus | IPv4 | - | - | - |
| Ethernet49/1 | - | IPv4 | - | - | - |
| Ethernet50/1 | - | IPv4 | - | - | - |
| Vlan22 | campus | IPv4 | - | - | Loopback10 |
| Vlan122 | campus | IPv4 | - | - | Loopback10 |
| Vlan333 | IOT | IPv4 | - | - | Loopback11 |
| Vlan4092 | IOT | IPv4 | - | - | Loopback11 |
| Vlan4093 | - | IPv4 | - | - | - |

## Filters

### Prefix-lists

#### Prefix-lists Summary

##### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.252.0.16/28 eq 32 |
| 20 | permit 10.252.0.32/29 eq 32 |

##### PL-MLAG-PEER-VRFS

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.252.2.2/31 |

#### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 10.252.0.16/28 eq 32
   seq 20 permit 10.252.0.32/29 eq 32
!
ip prefix-list PL-MLAG-PEER-VRFS
   seq 10 permit 10.252.2.2/31
```

### Route-maps

#### Route-maps Summary

##### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |

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
| IOT | enabled |
| MGMT | disabled |

### VRF Instances Device Configuration

```eos
!
vrf instance campus
!
vrf instance IOT
!
vrf instance MGMT
```

## Virtual Source NAT

### Virtual Source NAT Summary

| Source NAT VRF | Source NAT IPv4 Address | Source NAT IPv6 Address |
| -------------- | ----------------------- | ----------------------- |
| campus | 10.252.2.37 | - |
| IOT | 10.253.2.37 | - |

### Virtual Source NAT Configuration

```eos
!
ip address virtual source-nat vrf campus address 10.252.2.37
ip address virtual source-nat vrf IOT address 10.253.2.37
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
