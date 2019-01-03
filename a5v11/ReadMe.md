# Useful links 

[OpenWrt description page](https://openwrt.org/toh/unbranded/a5-v11)

# MTD partition table
```
# cat /proc/mtd
dev:    size   erasesize  name
mtd0: 00030000 00001000 "u-boot"
mtd1: 00010000 00001000 "u-boot-env"
mtd2: 00010000 00001000 "factory"
mtd3: 003b0000 00001000 "firmware"
mtd4: 0011b4ef 00001000 "kernel"
mtd5: 00294b11 00001000 "rootfs"
mtd6: 00041000 00001000 "rootfs_data"
```

# Files

[u-boot-A5-V11.zip](u-boot-A5-V11.zip) - custom u-boot images, allowing to boot openwrt properly, supporting tftp recovery etc. 

- uboot_usb_128_03.img - 16 Mb RAM model 
- uboot_usb_256_03.img - 32 Mb RAM model 

[lede-17.01.4-ramips-rt305x-a5-v11-squashfs-sysupgrade.bin](lede-17.01.4-ramips-rt305x-a5-v11-squashfs-sysupgrade.bin) - basic OpenWrt (LEDE) image, allowing to enable ExtRoot, including:

- luci web interface
- Full USB storage support
- f2fs support, both kernel module and mkfs
- block-mount

# Installing LEDE 17.01.4 on a stock (Qualcomm) firmware

## Preparations

- Copy lede-17.01.4-ramips-rt305x-a5-v11-squashfs-sysupgrade.bin image to FAT-formatted USB flash drive and rename to firmware.bin
- Unpack u-boot-A5-V11.zip to the same drive
- Connect your Linux laptop/PC to the mini router via Ethernet and power up the mini router
- Connect the USB Flash drive to the mini router
- Telnet to the mini router with the following command:
```
telnet 192.168.100.1
```
The username and password are admin
- Mount the USB flash drive with the following command:
```
mount /dev/sda1 /mnt
```
- Wait a few seconds and verify that you see files
```
ls /mnt
```
You should see your files. Do not go further if you do not see files!

## Installation

- Upgrade uboot - be careful, do not reset router during and after this operation!
```
mtd_write write  /mnt/uboot_usb_256_03.img Bootloader
```
You should see on console
```
#Unlocking Bootloader …
#Writing from /mnt/uboot256.img to Bootloader …  [w]
```
- Upgrade firmware - do not reset router during this operation!
```
mtd_write write /mnt/firmware.bin Kernel
```
You should see on console
```
#Unlocking Kernel …
#Writing from /mnt/firmware.bin to Kernel …  [w]
```
- Reboot router with the following command.
```
reboot
```

# Configuring ExtRoot

- Partition USB flash on your choice, for instance: MBR partition table, 2 primary partitions: 
```
- 1: 128 Mb, swap 
- 2: The rest, extroot 
```
- Plug the USB flash in 
- After plugging the USB drive, it should show up as a storage device under the /dev directory as /dev/sda1, /dev/sda2 etc. 
- Create swap and root fs: 
```
# mkswap /dev/sda1 
# mkfs.f2fs /dev/sda2
``` 
- Mount the USB drive
```
# mount /dev/sda2 /mnt
```
- Copy data from /overlay partition to the USB drive
```
# tar -C /overlay/ -c . -f - | tar -C /mnt/ -xf -
```
- Un-mount the USB drive
```
# sync && umount /dev/sda2
```
- Configure /etc/config/fstab to mount the USB drive as /overlay partition and enable swap
```
# block detect > /etc/config/fstab
```
- Edit the /etc/config/fstab with vi to mount partitions
Refer to a sample fstab configuration:
```
config 'global'
        option  anon_swap       '0'
        option  anon_mount      '0'
        option  auto_swap       '1'
        option  auto_mount      '1'
        option  delay_root      '5'
        option  check_fs        '0'

config 'swap'
        option  device  '/dev/sda1'
        option  enabled '1'

config 'mount'
        option  target  '/overlay'
        option  uuid    'fb6a813b-31d1-4ae2-b4a5-d5362ea4ce2a'
        option  enabled '1'
```

- Enable the fstab service at startup
```
# /etc/init.d/fstab enable
```
- Reboot the router with the reboot command
```
# reboot
```

