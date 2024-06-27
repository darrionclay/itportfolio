To showcase familiarity and comfortableness in a command line interface by installing a Linux operating system


Hyperbola OS is an operating system geared towards having the users maintain control over the component/packages in their system.They utilize community projects while also respecting their users' privacy and respecting the stability of their system. It utilizes the pacman package manager and the openrc init system.

With that being said, Hyperbola OS is a great alternative to the bleeding-edge and unstable Windows 11 and other operating systems that may not having the best record of maintaining a stable environment for their users. 

To start, I will be doing the entire installation from Virt-manager(with qemu)following the guide provided by the creator of the operating system. After booting into the OS:


checking disk space
[hypos1.png]

formatting disk using fdisk
[hypos2.png]

[hypos3.png]
[hypos4.png]
[hypos5.png]


The Start screen after inital boot:
![[hyp1.png]]

Checking and formatting disk space:
![[hyp2.png]]

```
fdisk -l
fdisk /dev/vda
n
p
1
..
+18G

n
p
2
..
..

a
1
t
2
82
w
```



```
mkfs.ext4 /dev/vda1
mkswap /dev/vda2
swapon /dev/vda2

mount /dev/vda1 /mnt 
pacstrap /mnt base
```



```
genfstab -U -p /mnt  >>  /mnt/etc/fstab
arch-chroot  /mnt
nano /etc/locale.gen
locale-gen
echo LANG=en_US.UTF-8
export LANG=en_US.UTF-8
rc-update add keymaps default
```

```
hwclock --systohc --utc
echo localhost > /etc/hostname
passwd
```

```
pacman -S grub
grub-install --target=i386-pc --recheck /dev/vda
grub-mkconfig -o /boot/grub/grub.cfg
exit
umount -R /mnt
reboot
```

```
rc-service dhcpcd start
pacman -S doas
useradd -g users -m -s /bin/bash clay
passwd clay
```

```
usermod -aG video clay && usermod -aG wheel clay && usermod -aG audio clay && usermod -aG sys clay && usermod -aG storage clay && usermod -aG optical clay && usermod -aG power clay && usermod -aG games clay

exit
```

```
doas pacman -S xenocara xenocara-video-vesa lumina lumina-extra slim xterm awesome

nano .xinitrc

exec start-lumina-desktop

doas rc-update add dhcpcd default 

doas reboot
```