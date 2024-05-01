# xOps-2024

### CPE camp private cloud [Proxmox](https://www.proxmox.com/en/proxmox-virtual-environment/features)

* Start Date: 30 Mar 2024
* End Date: 19 May 2024

## Diagram
![xOps_Dia01](https://github.com/kititach/xOps-2024/assets/48780839/43ef5a7b-3045-4e89-abdf-2ee322a85e11)
![week2-1](https://github.com/kititach/xOps-2024/assets/48780839/14c8c153-85b5-41f1-ba14-3753875cd9ea)
![week2-2](https://github.com/kititach/xOps-2024/assets/48780839/ddcf4f1e-ebbd-4460-b91f-2111f7ef2c5b)

## Diagram New
![xOps_Dia02_no_ip](https://github.com/kititach/xOps-2024/assets/48780839/1f8ee550-0da4-48e4-8959-54914d2a3570)

https://raw.githubusercontent.com/pitimon/xOps2024/main/misc/Ceph-cluster_blog-storage.webp

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

Corosync
---
  - Corosync เป็นซอฟต์แวร์ที่ใช้ในระบบคอมพิวเตอร์แบบการทำงานร่วมกัน (cluster computing) เพื่อให้เครื่องคอมพิวเตอร์หลายเครื่องสามารถทำงานร่วมกันอย่างเป็นความสัมพันธ์ โดย Corosync มักถูกใช้ในการสร้างและบริหารจัดการกลุ่มคอมพิวเตอร์ที่ใช้ร่วมกันเพื่อความเสถียรและประสิทธิภาพ เช่น ในระบบคลัสเตอร์สำหรับเว็บเซิร์ฟเวอร์ การโฮสต์ฐานข้อมูล หรือแม้กระทั่งการประมวลผลที่มีความต้องการในการควบคุมและความน่าเชื่อถือสูง ๆ โดยส่วนใหญ่ Corosync จะใช้ร่วมกับซอฟต์แวร์ Pacemaker เพื่อให้มั่นใจในความเสถียรและประสิทธิภาพของระบบคลัสเตอร์ที่สร้างขึ้น.

kvm (lernel-base virtual machine)
---
KVM (Kernel-based Virtual Machine) เป็นเทคโนโลยีการสร้างเครื่องเสมือนบนระบบปฏิบัติการ Linux ซึ่งใช้โค้ดของแกน Linux เป็นพื้นฐาน. KVM อนุญาตให้คุณสร้างและจำลองเครื่องเสมือน (virtual machines) บนเซิร์ฟเวอร์หรือเครื่องคอมพิวเตอร์ของคุณ โดยที่แต่ละเครื่องเสมือนสามารถรันระบบปฏิบัติการแยกต่างหากได้ เช่น Linux, Windows, BSD ฯลฯ  
- นักพัฒนาและผู้ดูแลระบบสามารถใช้ KVM เพื่อสร้างพื้นที่สำรอง (sandboxing) สำหรับการทดสอบและพัฒนาซอฟต์แวร์ การเปิดใช้งานแอปพลิเคชันหลาย ๆ ตัวบนเครื่องเดียวกัน หรือการใช้ทรัพยากรคอมพิวเตอร์อย่างมีประสิทธิภาพโดยใช้งานระบบเครือข่ายคลาวด์ (cloud computing) ซึ่งเป็นวิธีที่นิยมในการให้บริการในสถาปัตยกรรมระดับสายพันธุ์ (scale-out architectures) และระบบพื้นฐานเป็นระบบคลาวด์ (cloud-native infrastructure).
- KVM ใช้การจำลองฮาร์ดแวร์ของโปรเซสเซอร์หรือ CPU (virtualization extensions) เพื่อเพิ่มประสิทธิภาพและประสิทธิภาพในการทำงานของเครื่องเสมือน โดยที่ไม่ต้องใช้ซอฟต์แวร์ที่ทำงานในระดับแนวโน้ม (paravirtualization) ทำให้มีประสิทธิภาพที่เหมาะสมสำหรับการใช้งานในสถาปัตยกรรมที่เป็นของโปรเซสเซอร์ (native processor architecture) และมีความเชื่อถือที่มากขึ้นในการจำลองเครื่องเสมือน.

LXC (linux container)
---

Linux bridge
---

Openvswitch
---

LVM (Logical Volume Manager)
---
LVM, LVM-Thin, และ ZFS เป็นเครื่องมือสำหรับการจัดการพื้นที่จัดเก็บข้อมูลในระบบปฏิบัติการ Linux และอื่น ๆ อย่างไรก็ตามพวกเขามีวัตถุประสงค์และคุณสมบัติที่แตกต่างกันดังนี้:  
 1. LVM (Logical Volume Manager):
    - LVM เป็นโครงสร้างที่อนุญาตให้ผู้ใช้สร้างพาร์ติชัน รวมถึงการจัดเก็บข้อมูลบนพาร์ติชันดิสก์หรือระดับของหลายพาร์ติชันในระดับที่สูงขึ้น เช่น การสร้างโซนพื้นที่เก็บข้อมูลเชิงต่อเนื่อง และการจัดเก็บข้อมูลที่มีขนาดเปลี่ยนแปลงได้.  
    - LVM ใช้ขนาดของพาร์ติชันเสมือน ๆ หรือ "Logical Volumes" เพื่อจัดการพื้นที่เก็บข้อมูล และผู้ใช้สามารถทำการเพิ่ม ลด หรือย้ายขนาดของ Logical Volumes ได้โดยไม่ต้องตัดข้อมูล.  
    - คุณสมบัติของ LVM รวมถึงความยืดหยุ่นในการจัดการพื้นที่เก็บข้อมูลและการดูแลรักษา.
 2. LVM Thin:
    - LVM Thin เป็นส่วนขยายของ LVM ที่อนุญาตให้ใช้พื้นที่เก็บข้อมูลเป็น Thin Provisioning ซึ่งหมายถึงการจัดสรรพื้นที่เก็บข้อมูลเมื่อจำเป็นและตามความต้องการ โดยไม่ต้องกำหนดขนาดคงที่ล่วงหน้า.  
    - คุณสมบัติ Thin Provisioning ช่วยลดการสูญเสียพื้นที่ที่ไม่จำเป็นและช่วยประหยัดพื้นที่จัดเก็บข้อมูล.
 3. ZFS (Zettabyte File System):
    - ZFS เป็นระบบไฟล์และโครงสร้างการจัดการพื้นที่เก็บข้อมูลที่แตกต่างจากระบบไฟล์传统ในด้านความสามารถและความปลอดภัย.
    - ZFS มีคุณสมบัติเช่น Copy-on-Write, Snapshots, Data Deduplication, Compression, และ Raid-Z (การจัดเก็บข้อมูลแบบ RAID บนระบบไฟล์ระดับแอปลิเคชัน) เพื่อเพิ่มประสิทธิภาพและความปลอดภัยของข้อมูล.
    - ZFS เหมาะสำหรับการใช้งานที่ต้องการความปลอดภัยของข้อมูลสูงและความยืดหยุ่นในการจัดการพื้นที่เก็บข้อมูล.  
    
OVS bond, bridge , inPort
---

proxmox build cluster
---

ceph osd, mon, mds , RBD , cephFS
---


