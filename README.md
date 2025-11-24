# UTM ARCH LINUX
Basic install Arch Linux 2025 on UTM Macbook pro 2017.
Thank you for Arfan Zubi for tutotial on [Youtube](https://www.youtube.com/watch?v=cOobSmI-XgA). 

## Pre-Install Requirement:
1. download iso from [Arch Linux](https://archlinux.org/download/)
2. get [UTM](https://mac.getutm.app/)

## UTM Setup and guide
- Create virtual manager with UTM
- choose linux, and just start the VM to boot from mirror [ISO file](https://fastly.mirror.pkgbuild.com/iso/2025.11.01/) 
- follow installation guide: https://wiki.archlinux.org/title/Installation_guide

### Pre-install
make everything is setup keymaps, font, check efi boot, internet and timestamp.
```
# loadkey us
```

- change font
```
#  setfont Lat2-Terminus16
```
  
- check efi boot
``` 
# ls /sys/firmware/efi/efivars
```
make sure your have Internet Connection. If not connect check your UTM Network.
```
# ping ping.archlinux.org
```
## A. Partition disk
check your disk using command ```fdisk -l``` and create partition scheme by following Installation guide.
- select partition: in my case i have Disk /dev/sda: 60GB. run command ```fdsik /dev/sda```
- type *"g"* —— to create GPT Partition.
### A.1. Create partition —— UEFI, Swap and Linux
- to create new partition type *"n"* –— Partition number using by default.
- and first sector just following by default.
- last sector will be size **+512M**
- choose the type of partition by type *"t"* to see type *L* with aliases: *eufi*
- create second partition for swap. step 1,2,3 is same like before only type is different using aliases: *swap*
- create third partition type is *linux*
!important after finish to save partition.
- type *w* to write (save partition)
make sure your partition layout already correct before format the partition.

### A.2. set partition
- run commond. ```mkfs.ext4 /dev/sda3``` — root partition
- run commond ```mkswap /dev/sda2``` - swap partition
- run command ```mkfs.fat -F 32 /dev/sda1``` — uefi boot

### A.3. Mount partition
mount you your file system
```
# mount /dev/sda3 /mnt
# mount --mkdir /dev/sda1 /mnt/boot
# swapon /dev/sda2
```
## B. Install packages for the system
```
# pacstrap -K /mnt base base-devel linux linux-firmware e2fsprogs dhcpcd networkmanager vim neovim man-db man-pages texinfo
```
## B.1 Configure the system

- generate fstab file:
```
# genfstab -U /mnt >> /mnt/etc/fstab
```
make it sure it's correct check ```vim /mnt/etc/fstab```.

- chroot
```
# arch-chroot /mnt
```

- timezone
  - ```#ln -sf /usr/share/zoneinfo/Area/Location /etc/localtime```
  - ```# hwclock --systoh``` — clock

- Network configure
  - ```# vim /etc/hostname``` feel your *yourhostname*
  - ```# vim /etc/hosts```
 
- Initramfs
``` # mkinitcpio -P ```

- root password
``` # passwd ``` — type your root password

- Bootloader

```
pacman -S grub efibootmgr
```

```
grub-install --efi-directory=boot --bootloader-id=GRUB
```

```
grub-mkconfig -o /boot/grub/grub.cfg
```

- Reboot
```
# exit
# umount -R /mnt
# poweroff
```

make sure your cd installation has been clear from UTM app before you booting.
note: after finish this step make sure your internet to active, if not using this ```systemctl start NetworkManager``` to start Network Manager.

