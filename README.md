 # utm-arch
basic install Arch Linux 2025 on UTM Macbook pro 2017.

## Pre-Install Requirement:
1. download iso from: https://archboot.com/
2. get UTM https://mac.getutm.app/

## Installation Arch Linux
- Create virtual manager with UTM
- choose linux, and just start the VM to boot from ISO file.
- follow installation guide: https://wiki.archlinux.org/title/Installation_guide
- Arch boot a bit bug so just using ctrl+c to exit and doing manually from terminal.
### Pre-install
make everything is setup keymaps, font, check efi boot, internet and timestamp.
```
~ loadkey us
```

- change font
```
~  setfont Lat2-Terminus16
```
  
- check efi boot
``` 
ls /sys/firmware/efi/efivars
```
make sure your have Internet Connection. If not connect check your UTM Network.
```
~ ping ping.archlinux.org
```
## A. Partition disk
check your disk using command ```fdisk -l``` and create partition scheme by following Installation guide.
- select partition: in my case i have Disk /dev/sda: 60GB. run command ```fdsik /dev/sda```
- type *"g"* —— to create GPT Partition.
### 1. Create partition —— UEFI, Swap and Linux
- to create new partition type *"n"* –— Partition number using by default.
- and first sector just following by default.
- last sector will be size **+512M**
- choose the type of partition by type *"t"* to see type *L* with aliases: *eufi*
- create second partition for swap. step 1,2,3 is same like before only type is different using aliases: *swap*
- create third partition type is *linux*
!important after finish to save partition.
- type *w* to write (save partition)
make sure your partition layout already correct before format the partition.

### 2. set partition
- run commond. ```mkfs.ext4 /dev/sda3``` — root partition
- run commond ```mkswap /dev/sda2``` - swap partition
- run command ```mkfs.fat -F 32 /dev/sda1``` — uefi boot

### 3. Mount partition
mount you your file system
```
~ mount /dev/sda3 /mnt
~ mount --mkdir /dev/sda1 /mnt/boot
~ swapon /dev/sda2
```

