## Arch-terminal-install-cheatsheet
original: https://pastebin.com/iU72RkQ2

Arch Linux uefi boot install for "dummies". It's literally that easy *in progress (being/adding more to it still) only use if you already know what you are doing*

# Base system install (have google available for ![this](https://wiki.archlinux.org/title/Installation_guide))
 X the size in MiB you want for each partition or section for linux functions this is also why you need google to make sure you're transfering the correct GiB into MiB
 
1. timedatectl set-ntp true

2. parted -a optimal /dev/sdX (X is the drive type e.g hdd= /dev/sda nvme = nvmeXnXp) 

3. mklabel gpt

4. mkpart esp fat32 1 512 (just leave it at 512 i didn't change it for a reason)

5. mkpart rootfs ext4 XXXX 100%

6. mkpart swap linux-swap 512 XXXX

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

20. systemctl enable NetworkManager 

21. grub-install --target=x86_64-efi --efi-directory=/boot/efi

22. grub-mkconfig -o /boot/grub/grub.cfg``

23. passwd     (leaving this blank creates roots/su's password should you need to ever *sudo su*)

# User creation and root (su or sudo) password

(we are doing root first)

1. passwd
(step one would have asked for a password and when you typed it in the terminal is not designed to show it)

2. useradd -m theusernameyouwant

3. passwd theusername you just created (to make a password for them specifically)

# DE (looks like a lot but i am getting the basics for gaming/office/coding btw steam will prompt drivers so select the appropriate driver)
my favourite it plasma so i am going to us that as an example

1. sudo nano /etc/pacman.conf     (remove if it isn't already the *#* on the multilib repo, extra repo and community repo it will save you later on *don't touch the testing repos*) 

2. pacman -Syyu

3. pacman -Syu

4. pacman -S plasma-desktop dolphin firefox steam discord kate obs-studio kdenlive libreoffice gimp flatpak wine-staging giflib lib32-giflib libpng lib32-libpng libldap lib32-libldap gnutls lib32-gnutls mpg123 lib32-mpg123 openal lib32-openal v4l-utils lib32-v4l-utils libpulse lib32-libpulse libgpg-error lib32-libgpg-error alsa-plugins lib32-alsa-plugins alsa-lib lib32-alsa-lib libjpeg-turbo lib32-libjpeg-turbo sqlite lib32-sqlite libxcomposite lib32-libxcomposite libxinerama lib32-libgcrypt libgcrypt lib32-libxinerama ncurses lib32-ncurses opencl-icd-loader lib32-opencl-icd-loader libxslt lib32-libxslt libva lib32-libva gtk3 lib32-gtk3 gst-plugins-base-libs lib32-gst-plugins-base-libs vulkan-icd-loader lib32-vulkan-icd-loader lutris git wget xsel iphython python linux-zen linux-zen-headers linux-lts linux-lts-headers vim neofetch htop i2c-tools plank xorg

# enjoy, after this last step use this part of the wiki for ![drivers](https://wiki.archlinux.org/title/External_GPU#Installation)
sudo reboot

