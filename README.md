## Arch-cheatsheet
original: https://pastebin.com/iU72RkQ2

Arch Linux uefi boot install for "dummies". It's literally that easy *in progress (being/adding more to it still) only use if you already know what you are doing*

# Base system install (have google available for this:https://wiki.archlinux.org/title/Installation_guide)
 
1. timedatectl set-ntp true
2. parted -a optimal /dev/sdX (X is the drive type e.g hdd= /dev/sda nvme = nvmeXnXp) 
3. mklabel gpt
4. mkpart esp fat32 1 512
5. mkpart rootfs ext4 XXXX 100%
6. mkpart swap linux-swap 512 XXXX (X the size in MiB you want for each partition or section for linux functions this is also why you need google)
7. set 1 boot on
8. quit
9. mkfs.vfat -F32 /dev/sdX1 (nvme = nvmeXnXpX)
10. mkswap /dev/sdX3
11. swapon /dev
12. mkfs.ext4 /dev/sdX2 
13. mount /dev/sdX2 /mnt
14. mkdir -p /mnt/boot/efi
15. mount /dev/sdX1 /mnt/boot/efi
16. pacstrap /mnt base base-devel linux linux-firmware
17. genfstab -U /mnt >> /mnt/etc/fstab
18. arch-chroot /mnt
19. pacman -Sy grub efibootmgr dosfstools e2fsprogs dhcpcd networkmanager bash-completion git curl psutils pciutils ed xz bzip2 lz4 lzop ca-certificates
systemctl enable NetworkManager 
grub-install --target=x86_64-efi --efi-directory=/boot/efi
grub-mkconfig -o /boot/grub/grub.cfg
passwd root
