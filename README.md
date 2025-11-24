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
### Partition disk
check your disk using command ```fdisk -l``` and create partition scheme by following Installation guide.
- select partition: in my case i have Disk /dev/sda: 60GB. run command ```fdsik /dev/sda```
- type "g" > to create GPT Partition.



