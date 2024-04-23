# xOps-2024

### CPE camp private cloud [Proxmox](https://www.proxmox.com/en/proxmox-virtual-environment/features)

* Start Date: 30 Mar 2024
* End Date: 19 May 2024

## Diagram
![xOps_Dia01](https://github.com/kititach/xOps-2024/assets/48780839/43ef5a7b-3045-4e89-abdf-2ee322a85e11)
![week2-1](https://github.com/kititach/xOps-2024/assets/48780839/14c8c153-85b5-41f1-ba14-3753875cd9ea)
![week2-2](https://github.com/kititach/xOps-2024/assets/48780839/ddcf4f1e-ebbd-4460-b91f-2111f7ef2c5b)

## Diagram New
![xOps_Dia02](https://github.com/kititach/xOps-2024/assets/48780839/5e26cbd3-726c-41b6-a3e5-af35f4e3d272)

## Lab Equipment
* Fortinet 60E
  * [datasheet](https://www.firewalls.com/pub/media/wysiwyg/datasheets/Fortinet/FG-FW-60E.pdf)
  * [FortiOS](https://docs.fortinet.com/product/fortigate/hardware)
* Fortiswitch 424D
  * [Specification](https://www.avfirewalls.com.au/FortiSwitch-424D.asp)
* Dell R330
  * [datasheet](https://i.dell.com/sites/csdocuments/Shared-Content_data-Sheets_Documents/en/aa/Dell_PowerEdge_R330_SpecSheet_final.pdf)
* Proxmox
  * [Download](https://www.proxmox.com/en/downloads)
  * [PVE Admin Guide](https://pve.proxmox.com/pve-docs/pve-admin-guide.html)

## Install Lab
### FW 
  * Reset default (on power 60s)
  * Update Hardware (for new product)
  * Change ip, host
  * Create Network 801, 802, 803, 804, (mgmt to pve 851, 852 (Vlan))
  * Create User & Group
  * Setup VPN 
  * Create Policy (interface, VPN)
### SW
  * sw1 (service)
    * Reset default
    * Update Hardware (for new product)
    * Change ip, host
    * Create trunk port server1(1,2), server2(3,4), server3(5,6)
    * Create Vlan 801, 802, 803, 804

  * sw2 (mgmt)
    * Reset default
    * Update Hardware (for new product)
    * Change ip, host
    * Create trunk port server1(1,2), server2(3,4), server3(5,6)
    * Create Vlan 851, 852

### Server
  * Setup BIOS, IDRAC, HDD
  * Install OS

### coming soon
-------------------------------------------------
- corosync
- kvm (lernel-base virtual machine)
- LXC (linux container)
- Linux bridge
- Openvswitch

- LVM , disk partion
- OVS bond, bridge , inPort
- proxmox build cluster
- ceph osd, mon, mds , RBD , cephFS
