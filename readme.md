# Repository info

## site_builder

- Create ***dhcpd.conf*** file for linux DHCP server and output to **/workspace/site_builder/dhcpd-config/dhcpd-config.cfg**

- Create ZTP configs by populating ***devices.csv***
  - Upgrade EOS to desired version
  - Install token if using CVaaS
  - Add intended running config to switch

- Create site folder in "sample_sites" containing all required files by populating ***sites.csv***
  - ANTA tests
  - Ansible inventory file
  - group_vars and variables
  - Required playbooks
  - ansible.cfg
  - Makefile for each site

## port_builder

- Create AVD compatible yaml format of **NETWORK_PORTS.yml** or **CONNECTED_ENDPOINTS.yml** by populating **switches.csv**

Output is located in **/workspace/port_builder/ports/GENERATED_CONNECTED_ENDPOINTS.yml**

## ContainerLAB for testing

This is used to test and validate network changes before pushing to prod.

ContainerLAB topology located here in **/workspace/inventories/BACKBONE/clab/BACKBONE.yaml**

**.devcontainer** folder is included which contains all packges to run AVD, Ansible & ANTA

## Bringing a new site online

1. Enter info into ***sites.csv*** and run make build.
2. Add new site DS switch layer 3 leafs Loopback0 overlay interface to **BACKBONE/group_vars/BACKBONE.yml** **evpn_gateay** key.
3. Add new site to global_vars **DEMO_GLOBAL.yml** **l3_edge** key.

## ANTA example

```sh
cd /workspace/inventories/BACKBONE/anta
anta nrfu -u test -p 'test' -i BACKBONE_anta_inventory.yml -c BACKBONE_anta_tests.yml
```

## ANTA on single device

```sh
cd /workspace/inventories/BACKBONE/anta
anta nrfu -u test -p 'test' -i BACKBONE_anta_inventory.yml -c BACKBONE_anta_tests.yml -d BACKBONE-M12-BL1
```