# Day 13 – Linux Volume Management (LVM)

## Commands Used

### Switch to root
sudo -i

### Create virtual disk (if no spare disk)
dd if=/dev/zero of=/tmp/disk1.img bs=1M count=1024
losetup -fP /tmp/disk1.img
losetup -a

---

## Step 1: Check Current Storage
lsblk
pvs
vgs
lvs
df -h

---

## Step 2: Create Physical Volume
pvcreate /dev/loop0
pvs

---

## Step 3: Create Volume Group
vgcreate devops-vg /dev/loop0
vgs

---

## Step 4: Create Logical Volume
lvcreate -L 500M -n app-data devops-vg
lvs

---

## Step 5: Format and Mount
mkfs.ext4 /dev/devops-vg/app-data
mkdir -p /mnt/app-data
mount /dev/devops-vg/app-data /mnt/app-data
df -h /mnt/app-data

---

## Step 6: Extend Logical Volume
lvextend -L +200M /dev/devops-vg/app-data
resize2fs /dev/devops-vg/app-data
df -h /mnt/app-data

---

## What I Learned

1. Difference between PV, VG, and LV.
2. Storage can be extended without reformatting everything.
3. LVM is flexible and useful in production servers.
