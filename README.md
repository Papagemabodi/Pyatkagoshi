# Pyatkagoshi

Tutorial is very messy because I'm writing it while being half asleep

Default root password is 9519

Download arch install iso and burn on a usb drive;


Prepare formatted drive(empty);


Find it by typing `lsblk` (for example `/dev/sdX`);


Prepare EFI and main partitions on target drive:

`mkfs.fat -F32 /dev/sdX1
mkfs.ext4 /dev/sdX2`


Mount:

`mount /dev/sdX2 /mnt
mkdir /mnt/boot/efi
mount /dev/sdX1 /mnt/boot/efi`

Download it from releases;


Extract:

`tar -I zstd -xvf whateveristhearchivename.tar.zst -C /mnt`

Make fstab:

`genfstab -U /mnt >> /mnt/etc/fstab`

Chroot into this amalgamation of pure terror:

`arch-chroot /mnt`

FURTHER COMMANDS ARE EXECUTED WHILE CHROOTED, IF YOU BREAK SOMETHING IT'S NOT MY FAULT

Recompile dracut(just to be safe):

`dracut --force --kver 6.15.7-arch1-1 /boot/initramfs-6.15.7.img
`



Grub, bootloader:

`pacman -S grub efibootmgr`

`grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg`


Misc:

Generate host name 

`touch /etc/hostname (if not there)
echo "whateveryouwanthere" > /etc/hostname`

OR:

`hostnamectl set-hostname yourhostname`

Nvidia fix(do only if you have nvidia card):

`pacman -S nvidia nvidia-utils
touch /etc/dracut.conf.d/nvidia.conf
micro /etc/dracut.conf.d/nvidia.conf`

Now insert those lines: 

`force_drivers+=" nvidia nvidia_modeset nvidia_uvm nvidia_drm "
add_dracutmodules+=" drm "
hostonly="yes"`

Save (ctrl S) and exit (ctrl Q)

`dracut --force --kver 6.15.7-arch1-1 /boot/initramfs-6.15.7.img` (every time you append a dracut config you have to recompile it)



`micro /etc/default/grub`

find line GRUB_CMDLINE_LINUX_DEFAULT="..." and append `nvidia_drm.modeset=1 nvidia-drm.fbdev=1` inside brackets
