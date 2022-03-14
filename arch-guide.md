# Installation

## Verify you are in efi
\# ls /sys/firmware/efi/efivars
- If the command is executed without errors continue.

# Connect to the internet
- \# iwctl
  - List stations (Probably wlan0)
    - \# device list
  - Find network ssid
    - \# station [station] get-networks
  - Connect to wifi
    - \# station [station] connect [ssid]
  - Check is connected
    - \# ping archlinux.org


# Update the system clock
- \# timedatectl set-ntp true

# Partition the disks
- List disks and choose the correct drive
  - \# fdisk -l
- \# fdisk /dev/[yourname]

crate your efi partition if does not exist 1G
create your main partition for the linux system and leave 1G for the swap
create the swap partition in the end of 1G size

set partition types to Linux root x86-64 (23) and Linux swap (swap) respectively

verify all your partitions are ok

exit with 
w

# Format the partitions
mkfs.fat -F 32 /dev/[efi partition] (if you created it)
mkfs.ext4 /dev/[linux filesystem]
mkswap /dev/[swap partition]

# Mount the file systems
mount /dev/[linux filesystem] /mnt
mkdir /mnt/efi
mount /dev/[efi filesystem] /mnt/efi
swapon /dev/[swap]

# Install base system and some extras
pacstrap /mnt base linux linux-firmware networkmanager gvim man-db man-pages texinfo grub efibootmgr os-prober dhcpcd amd-ucode/intel-ucode

# Configure the system
genfstab -U /mnt >> /mnt/fstab

arch-chroot /mnt

ln -sf /usr/share/zoneinfo/Region/City /etc/localtime

hwclock --systohc

# Edit locale
vim /etc/locale.conf
LANG=en_US.UTF-8
LC_TIME=el_GR.UTF-8

# Uncomment your wanted locales
vim /etc/locale.gen

locale-gen

vim /etc/hostname
myhostname

vim /etc/hosts
127.0.0.1        localhost
::1              localhost
127.0.1.1        myhostname
ff02::1			 ip6-allnodes
ff02::2          ip6-allrouters

passwd

# Install bootloader
add line in /etc/default/grub
GRUB_DISABLE_OS_PROBER=false
grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg

# Install extras
pacman -S xf86-video-amdgpu/intel(for laptop) pulseaudio-alsa alsa-utils alsa-firmware 
pacman -S xorg plasma-desktop dolphin dolphin-plugins ark kitty gwenview plasma-nm plasma-pa kdeplasma-addons kde-gtk-config powerdevil sddm sddm-kcm (bluez bluedevil for bluetooth) kscreen kinfocenter plasma-systemmonitor ffmpegthumbs firefox gedit sudo
pacman -S --needed base git 

# Create new user
useradd -m -u 1000 -G wheel,audio,kvm,input,storage [name] 
passwd name
edit /etc/sudoers and remove comment in wheel
# Enable sddm and nm
systemctl enable sddm
systemctl enable NetworkManager

# Reboot into the system
pacman -S nvidia (for nvidia gpu)
edit /etc/pacman.conf to enable color and parallel downloads


# Install paru
pacman -S rustup base-devel
rustup install stable
rustup default stable

git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
vim /etc/paru.conf and enable BottomUp

# Aliases
Create an aliases file and connect with bashrc
vim ~/.bash_aliases
alias command='command'

add this to ~/.bashrc
if [ -f ~/.bash_aliases ]; then
	. ~/.bash_aliases
fi


# Extras
paru net-tools
