# Image download [amd64, qcow2]
- [Ubuntu Cloud Images](https://cloud-images.ubuntu.com/)
- [Debian Cloud Images](https://cloud.debian.org/images/cloud/)

# Step
>> shell to proxmox node
```
wget https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img
wget http://rustdesk.ipv9.me/iso/jammy-server-cloudimg-amd64.img
```
```
qm create 8000 --core 2 --memory 2048 --name u2204CloudTemplate --net0 virtio,bridge=vmbr1 --serial0 socket --agent enabled=1
qm importdisk 8000 jammy-server-cloudimg-amd64.img ceph_replic
qm set 8000 --scsihw virtio-scsi-pci --scsi0 ceph_replic:vm-8000-disk-0
qm resize 8000 scsi0 +8G
qm set 8000 --ide2 ceph_replic:cloudinit
qm set 8000 --boot c --bootdisk scsi0
```
- clone Template 

```
qm clone 8000 521 --full true --name vm521

```

- echo multi-vm clone from template
```
n=500;for i in {1..20};do vmname=$(($n + $i)) ; echo qm clone 8000 $vmname --full true --name vm$vmname;done
```

- vm destroy
```
qm destroy [vmid]
```

### Tailscale technic
- [Tailscale Signup](https://tailscale.com/)  
>> IPv4 forward
```
echo 'net.ipv4.ip_forward = 1' | sudo tee -a /etc/sysctl.conf
echo 'net.ipv6.conf.all.forwarding = 1' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p /etc/sysctl.conf
```
```
sudo tailscale down
sudo tailscale up --advertise-routes=10.85.1.0/24,10.80.1.0/24 --reset --accept-dns=false
```
>> - advertise-route (ต้องการประกาศให้ client อื่นๆ ผ่านโหนดนี้ไปยัง subnet อะไรบ้าง)
>> - ประกาศ subnet จาก parameter นี้แล้ว ต้องไป approve ที่ tailscale portal ด้วย

## Post Install/clone
- Qemu agent
```
sudo apt update ; sudo apt install qemu-guest-agent -y
sudo systemctl restart qemu-guest-agent
sudo systemctl status qemu-guest-agent
```
- set Timezone
```
sudo dpkg-reconfigure tzdata
```
- set ntp client
```
sudo apt install chrony -y
sudo bash -c 'echo "server ntp.en.rmutt.ac.th iburst" >> /etc/chrony/chrony.conf'
```
- restart service
```
sudo systemctl restart chronyd
```
- check
```
chronyc sources 
```
- result
>> ^* ntp.en.rmutt.ac.th            2   6    17    12   -171us[  -65us] +/-   18ms             

### daemons have recently crashed proxmox
- Saat melihat status ceph cluster terdapat HEALTH_WARN
```
root@node01:~# ceph -s
  cluster:
    id:     c6cb4a87-93e9-404d-8bec-77e4d29aae23
    health: HEALTH_OK
 
  services:
    mon: 7 daemons, quorum node01,node02,node03,node05,node06,node07,node04 (age 19h)
    mgr: node03(active, since 19h), standbys: node02, node07, node01, node04, node05, node06
    mds: 1/1 daemons up, 6 standby
    osd: 13 osds: 13 up (since 8m), 13 in (since 9m)
 
  data:
    volumes: 1/1 healthy
    pools:   4 pools, 97 pgs
    objects: 12.66k objects, 49 GiB
    usage:   160 GiB used, 4.0 TiB / 4.2 TiB avail
    pgs:     97 active+clean
 
  io:
    client:   0 B/s rd, 134 KiB/s wr, 0 op/s rd, 23 op/s wr
```
- To display a list of messages:
```
root@node01:~# ceph crash ls
ID                                                                ENTITY      NEW  
2024-04-24T05:19:54.405640Z_168fbad4-ae22-411c-9422-028f643dea57  mon.node04   * 
```
- If you want to read the message:
```
ceph crash info <id>

root@node01:~# ceph crash info 2024-04-24T05:19:54.405640Z_168fbad4-ae22-411c-9422-028f643dea57
{
    "assert_condition": "store_size > 0",
    "assert_file": "./src/mon/HealthMonitor.cc",
    "assert_func": "bool HealthMonitor::check_member_health()",
    "assert_line": 586,
    "assert_msg": "./src/mon/HealthMonitor.cc: In function 'bool HealthMonitor::check_member_health()' thread 7f012b5986c0 time 2024-04-24T12:19:54.403107+0700\n./src/mon/HealthMonitor.cc: 586: FAILED ceph_assert(store_size > 0)\n",
    "assert_thread_name": "safe_timer",
    "backtrace": [
        "/lib/x86_64-linux-gnu/libc.so.6(+0x3c050) [0x7f013145b050]",
        "/lib/x86_64-linux-gnu/libc.so.6(+0x8ae2c) [0x7f01314a9e2c]",
        "gsignal()",
        "abort()",
        "(ceph::__ceph_assert_fail(char const*, char const*, int, char const*)+0x185) [0x7f0131c9d834]",
        "/usr/lib/ceph/libceph-common.so.2(+0x29d974) [0x7f0131c9d974]",
        "(HealthMonitor::check_member_health()+0x1322) [0x55a4478ca972]",
        "(HealthMonitor::tick()+0x57) [0x55a4478cd107]",
        "(Monitor::tick()+0xd0) [0x55a4477e1b30]",
        "(Context::complete(int)+0x9) [0x55a4478034f9]",
        "(CommonSafeTimer<std::mutex>::timer_thread()+0x114) [0x7f0131d8d254]",
        "(CommonSafeTimerThread<std::mutex>::entry()+0xd) [0x7f0131d8dd3d]",
        "/lib/x86_64-linux-gnu/libc.so.6(+0x89134) [0x7f01314a8134]",
        "/lib/x86_64-linux-gnu/libc.so.6(+0x1097dc) [0x7f01315287dc]"
    ],
    "ceph_version": "18.2.2",
    "crash_id": "2024-04-24T05:19:54.405640Z_168fbad4-ae22-411c-9422-028f643dea57",
    "entity_name": "mon.node04",
    "os_id": "12",
    "os_name": "Debian GNU/Linux 12 (bookworm)",
    "os_version": "12 (bookworm)",
    "os_version_id": "12",
    "process_name": "ceph-mon",
    "stack_sig": "3e1d2ffbf424c7ab5745ebe12f9f8640ac49fb5ae25dc9a05e93c8500673e575",
    "timestamp": "2024-04-24T05:19:54.405640Z",
    "utsname_hostname": "node04",
    "utsname_machine": "x86_64",
    "utsname_release": "6.5.11-8-pve",
    "utsname_sysname": "Linux",
    "utsname_version": "#1 SMP PREEMPT_DYNAMIC PMX 6.5.11-8 (2024-01-30T12:27Z)"
}
```
- then:
```
ceph crash archive <id>
```
- or:
```
ceph crash archive-all
```
- ceph crash clear
```
ceph crash clear <id>
```
  
