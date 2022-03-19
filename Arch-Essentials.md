# Installation

## Verify you are in efi
\# ls /sys/firmware/efi/efivars
- If the command is executed without errors continue.

## Connect to the internet
- \# iwctl
  - List stations (Probably wlan0)
    - \# device list
  - Find network ssid
    - \# station [station] get-networks
  - Connect to wifi
    - \# station [station] connect [ssid]
- Check is connected
  - \# ping archlinux.org


## Update the system clock
- \# timedatectl set-ntp true

## Partition the disks
- List disks and choose the correct drive
  - \# fdisk -l
- \# fdisk /dev/[yourname]
  | Key |       Type 	          | Size |
  | :-: |       :-:  	          | :-:  |
  |  g  | Partition table       |  -   | 
  |  n  | Efi partition         | +1G  |
  |  t  | Efi partition(1)      |  -   |
  |  n  | Root partition        | -1G  |
  |  t  | Linux root x86-64 (23)|  -   |
  |  n  | Swap partition        | +1G  |
  |  t  | Linux swap (19)       |  -   |
 - Verify all your partitions are ok and exit (w)

## Format the partitions
- \# mkfs.fat -F 32 /dev/[efi partition] (if you created it)
- \# mkfs.ext4 /dev/[linux filesystem partition]
- \# mkswap /dev/[swap partition]

## Mount the file systems
- \# mount /dev/[linux filesystem partition] /mnt
- \# mkdir /mnt/efi
  - \# mount /dev/[efi filesystem] /mnt/efi
- \# swapon /dev/[swap]

## Install base system and some extras ???
- \# pacstrap /mnt base linux linux-firmware networkmanager gvim man-db man-pages texinfo grub efibootmgr os-prober dhcpcd amd-ucode/intel-ucode

## Configure the system
- \# genfstab -U /mnt >> /mnt/fstab
- \# arch-chroot /mnt	
  - \# ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
  - \# hwclock --systohc
  - Edit locale
    - \# vim /etc/locale.gen 
    	- Uncommnent wanted locales
    	  - el_GR.UTF-8 UTF-8
    	  - en_US.UTF-8 UTF-8	
    	- \# locale-gen
    - \# vim /etc/locale.conf 
      - LANG=en_US.UTF-8
      - LC_TIME=el_GR.UTF-8
  - Network configuration
    - \# vim /etc/hostname
      - "myhostname"
    - \# vim /etc/hosts 
      |  	  |		   |
      |    :-: 	  |       :-: 	   |
      | 127.0.0.1 | localhost      | 
      | :::1      | "myhostname"   | 
      | 127.0.1.1 | "myhostname"   | 
      | ff02::1   | ip6-allnodes   | 
      | ff02::2   | ip6-allrouters | 	 
   - Create a password
     - \# passwd

## Install bootloader
- \# vim /etc/default/grub
  - Add lines/uncomment
    - GRUB_DISABLE_OS_PROBER=false
- \# grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=GRUB
- \# grub-mkconfig -o /boot/grub/grub.cfg

## Install extras
- \# pacman -S xf86-video-amdgpu/intel(for laptop) pulseaudio-alsa pulseaudio-bluetooth alsa-utils alsa-firmware 
- \# pacman -S xorg plasma-desktop dolphin dolphin-plugins ark kitty gwenview plasma-nm plasma-pa kdeplasma-addons kde-gtk-config powerdevil sddm sddm-kcm bluez bluedevil  kscreen kinfocenter plasma-systemmonitor ffmpegthumbs gedit sudo
- \# pacman -S --needed base git 

## Create new user
- \# useradd -m -u 1000 -G wheel,audio,kvm,input,storage [name] 
- \# passwd name
- \# vim /etc/sudoers
  - Remove comment in wheel 

## Enable sddm and nm
- \# systemctl enable sddm
- \# systemctl enable NetworkManager

## Finish up
- Reboot into the system
  - \# reboot

<br></br>
# Setup

## Core
- Bluetooth
  - \# sudo systemctl start bluetooth.service
  - \# sudo systemctl enable bluetooth.service
- Paru
  - \# sudo pacman -S rustup base-devel
  - \# rustup install stable
  - \# rustup default stable
  - \# git clone https://aur.archlinux.org/paru.git
  - \# cd paru
  - \# makepkg -si
  - \# sudo vim /etc/paru.conf 
    - Enable BottomUp
- Network Tools
  - \# paru net-tools
- Nvidia drivers
  - \# sudo pacman -S nvidia
- Pacman configuration
  - \# vim /etc/pacman.conf
    - Uncomment ParallelDownloads = 5
    - Uncomment colors

## Programms
|     App Name    |                                Command                               | .conf file |
|:----------------|:---------------------------------------------------------------------|:----------:|
| Anydesk         | \# paru anydesk                                                      |      -     |
| Arduino         | \# paru arduino                                                      |      -     |
| Chrome          | \# paru Chrome                                                       |            |
| Clion           | \# paru clion &&  \# paru clion-jre                                  |      -     |
| CopyQ           | \# paru copyq                                                        |      -     |
| Discord         | \# paru discord                                                      |            |
| Droidcam        | ????                                                                 |      -     |
| Flameshot       | \# paru flameshot                                                    |    file    |
| Google  Drive   | ????                                                                 |      -     |
| Gestures        | \# paru gestures & \# paru libinput-gestures                         |      ?     |
| Grub Customizer | \# paru grub-customizer                                              |      -     |
| Intellij        | \# paru intellij-idea-ultimate && \# paru intellij-idea-ultimate-jre |      -     |
| Joplin          | ????                                                                 |      -     |
| Matlab          | <a href="https://matlab.mathworks.com/">Download</a>                 |      -     |
| Notepadqq       | \# paru notepadqq                                                    |      -     |
| Obs             | \# paru obs-studio                                                   |      -     |
| Pycharm         | \# paru pycharm                                                      |      -     |
| qbittorrent     | \# paru qbittorrent                                                  |            |
| Rider           | \# paru rider                                                        |      -     |
| Signal          | \# paru signal-desktop                                               |      -     |
| Spotify         | \# paru Spotify                                                      |      -     |
| TexStudio       | \# paru texstudio                                                    |      -     |
| TexLive         | \# paru texlive                                                      |      -     |
| Vlc             | \# paru vlc                                                          |      -     |
| Winrar          |                                                                      |            |
| WizTree         |                                                                      |            |
| Yt-dlp          | \# paru yt-dlp                                                       |    file    |
| Zoom            | \# paru zoom                                                         |      -     |

## General Configuration
- <a href="https://www.reddit.com/r/wallpaper/top/?t=all">Wallpaper</a>

<!-- - Create partition table (g)
     - Create your efi partition of +1G, if does not exist (n)(1)
       - Set partition types to "Efi Partition" (1) 
     - Create your main partition for the "Linux system" and leave -1Gb for the swap (n)(2)
       - Set partition types to Linux root x86-64 (23)
     - Create the swap partition in the end of +1G size (n)(3)
       - Set partition type "Linux swap" (19) 
-->


## Aliases
Create an aliases file and connect with bashrc
vim ~/.bash_aliases
alias command='command'

add this to ~/.bashrc
if [ -f ~/.bash_aliases ]; then
	. ~/.bash_aliases	
fi


paru qt5-tools
