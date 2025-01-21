# Repository info

## site_builder

- Create **dhcpd.conf** file for linux DHCP server

- Create ZTP configs by populating **devices.csv**
  - Upgrade EOS to desired version
  - Install token if using CVaaS
  - Add intended running config to switch

- Create site folder in "sample_sites" containing all required files by populating **sites.csv**
  - ANTA tests
  - Ansible inventory file
  - group_vars and variables
  - Required playbooks
  - ansible.cfg
  - Makefile for each site

## port_builder

- Create AVD compatible yaml format of **NETWORK_PORTS.yml** or **CONNECTED_ENDPOINTS.yml** by populating **switches.csv**

## ContainerLAB for testing

ContainerLAB topology located

```sh
/workspace/inventories/BACKBONE/clab/BACKBONE.yaml
```

### .devcontainer included which contains all packges to run AVD
