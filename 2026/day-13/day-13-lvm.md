 1  lsblk
    2  create lv
    3  lvm
    4  pv create
    5  clear
    6  df -h
    7  lsblk
    8  clear
    9  lsblk
   10  pvs
   11  vgcreate sample-vg /dev/nvme1n1  
   12  lvcreate -L 500M -n "app-data" sample-vg
   13  mkfs.ext4 /dev/sample-vg/app-data
   14  mkdir -p /mnt/app-data 
   15  mount /dev/sample-vg/app-data /mnt/app-data
   16  df -h /mnt/app-data
   17  lvextend -L +200M /dev/sample-vg/app-data
   18  resize2fs /dev/sample-vg/app-data
   19  df -h /mnt/app-data
   20  ls
   21  df -h
   22  lvextend -L +400M /dev/sample-vg/app-data
   23  resize2fs /dev/sample-vg/app-data
   24  df -h /mnt/app-data
   25  history 
What I Learned

LVM provides flexibility — we can extend storage without deleting data.

Storage management happens in three layers: Physical Volume → Volume Group → Logical Volume.

Logical volumes can be resized dynamically, which is very useful in real DevOps production environments.
<img width="997" height="1075" alt="Screenshot from 2026-02-25 23-22-23" src="https://github.com/user-attachments/assets/a312498e-d740-4ae1-b331-74346f472f66" />
