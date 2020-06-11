---
category: memo
tags:
  - pve
  - Proxmox VE
date: 2020-06-07
title: Proxmox VE 建置基礎
lang: zh-Hant-TW
vssue-id: 6
---

Proxmox VE，是一個開源的伺服器虛擬化環境 Linux 發行版。Proxmox VE 基於 Debian，使用基於 Ubuntu 的客製化核心。[wiki]

![](https://img.shields.io/github/license/meteorlxy/vuepress-theme-meteorlxy.svg?style=flat)

<!-- more -->

# Proxmox VE - pve

### 掛載 USB

安裝 NTFS 檔案系統套件

```
apt install ntfs-3g
```

掛載檔案系統

```
mkdir /mnt/usb/data
mount /dev/sdg1 /mnt/data
```

確定掛載是否完成

```
mount /dev/sdd1 /mnt/usb64
```

如掛載成功

```
/dev/sdd1 on /mnt/usb64 type exfat (rw,relatime,fmask=0022,dmask=0022,iocharset=utf8,namecase=0,errors=remount-ro)
```

### 硬碟直通

列出所有硬碟:

```
$ ls -l /dev/disk/by-id/

lrwxrwxrwx 1 root root  9 May 15 06:46 ata-KINGSTON_SA400S37120G_50026B778207E3FD -> ../../sdb
lrwxrwxrwx 1 root root 10 May 15 06:46 ata-KINGSTON_SA400S37120G_50026B778207E3FD-part1 -> ../../sdb1
lrwxrwxrwx 1 root root 10 May 15 06:46 ata-KINGSTON_SA400S37120G_50026B778207E3FD-part9 -> ../../sdb9
lrwxrwxrwx 1 root root  9 May 15 06:46 ata-KINGSTON_SHFS37A120G_50026B725907C34B -> ../../sda
lrwxrwxrwx 1 root root 10 May 15 06:46 ata-KINGSTON_SHFS37A120G_50026B725907C34B-part1 -> ../../sda1
lrwxrwxrwx 1 root root 10 May 15 06:46 ata-KINGSTON_SHFS37A120G_50026B725907C34B-part9 -> ../../sda9
lrwxrwxrwx 1 root root 10 May 15 06:46 dm-name-pve-root -> ../../dm-0
lrwxrwxrwx 1 root root 10 May 15 06:46 dm-name-pve-swap -> ../../dm-1
lrwxrwxrwx 1 root root 10 May 15 06:46 dm-uuid-LVM-edAHDVNslRGkTO14gCxzGkVbgjZWYLfG7hrwi8DvTiRANKIwHIJDMz6OZSV1W3Nv -> ../../dm-1
lrwxrwxrwx 1 root root 10 May 15 06:46 dm-uuid-LVM-edAHDVNslRGkTO14gCxzGkVbgjZWYLfGoTbvyzforTD2ZbxAD57qbMDdJ00KxGsP -> ../../dm-0
lrwxrwxrwx 1 root root 10 May 15 06:46 lvm-pv-uuid-2KGX1r-8heP-vrR6-RUef-jKcI-qozm-GsT2Eh -> ../../sdc3
lrwxrwxrwx 1 root root  9 May 15 06:46 usb-JetFlash_Transcend_32GB_CC6CRTBLFQ51Z89Q-0:0 -> ../../sdc
lrwxrwxrwx 1 root root 10 May 15 06:46 usb-JetFlash_Transcend_32GB_CC6CRTBLFQ51Z89Q-0:0-part1 -> ../../sdc1
lrwxrwxrwx 1 root root 10 May 15 06:46 usb-JetFlash_Transcend_32GB_CC6CRTBLFQ51Z89Q-0:0-part2 -> ../../sdc2
lrwxrwxrwx 1 root root 10 May 15 06:46 usb-JetFlash_Transcend_32GB_CC6CRTBLFQ51Z89Q-0:0-part3 -> ../../sdc3
lrwxrwxrwx 1 root root  9 May 15 18:35 usb-JetFlash_Transcend_64GB_CCCPMP9E35J7Z3BM-0:0 -> ../../sdd
lrwxrwxrwx 1 root root 10 May 15 18:35 usb-JetFlash_Transcend_64GB_CCCPMP9E35J7Z3BM-0:0-part1 -> ../../sdd1
lrwxrwxrwx 1 root root  9 May 15 06:46 wwn-0x50026b725907c34b -> ../../sda
lrwxrwxrwx 1 root root 10 May 15 06:46 wwn-0x50026b725907c34b-part1 -> ../../sda1
lrwxrwxrwx 1 root root 10 May 15 06:46 wwn-0x50026b725907c34b-part9 -> ../../sda9
lrwxrwxrwx 1 root root  9 May 15 06:46 wwn-0x50026b778207e3fd -> ../../sdb
lrwxrwxrwx 1 root root 10 May 15 06:46 wwn-0x50026b778207e3fd-part1 -> ../../sdb1
lrwxrwxrwx 1 root root 10 May 15 06:46 wwn-0x50026b778207e3fd-part9 -> ../../sdb9
```

設定虛擬機硬碟:

```
qm set <vmid> --sata<index> <your_disk>
```

example:

```
qm set 101 --sata0 /dev/disk/by-id/ata-KINGSTON_SA400S37120G_50026B778207E3FD
```

檢視所有可掛載之路徑:

```
showmount -e <NAS_IP_ADDRESS>
```

### LXC 容器設定

內顯直通:

```
$ ls -la /dev/dri/
total 0
drwxr-xr-x  3 root root        100 May 17 22:55 .
drwxr-xr-x 20 root root       4400 May 18 23:47 ..
drwxr-xr-x  2 root root         80 May 17 22:55 by-path
crw-rw----  1 root video  226,   0 May 17 23:02 card0
crw-rw----  1 root render 226, 128 May 17 22:55 renderD128
```

在下方加上設定檔案 `/etc/pve/lxc/<lxc_id>.conf`

```
lxc.cgroup.devices.allow: c 226:0 rwm
lxc.cgroup.devices.allow: c 226:128 rwm
lxc.mount.entry: /dev/dri/card0 dev/dri/card0 none bind,optional,create=file
lxc.mount.entry: /dev/dri/renderD128 dev/dri/renderD128 none bind,optional,create=file
lxc.apparmor.profile: unconfined
```

進入容器設定:

```
apt-get remove apparmor --purge
```

nfs share in lxc:

```
apt install nfs-common -yq

mkdir -p /mnt/<folder_name>

mount -t nfs 192.168.1.3:/mnt/mypool/<folder_name> /mnt/<folder_name>
```

add setting in `/etc/fstab`:

```
<host_ip_address>:/mnt/mypool/<folder_name> /mnt/<folder_name> nfs rw 0 0
```

ubuntu 20.4 lxc docker install:

```
apt update -yq
apt install docker.io -yq
apt install curl -yq
curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```
