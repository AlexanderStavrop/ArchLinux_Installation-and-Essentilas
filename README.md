# ArchLinux - Installation

A guide for installing arch linux and the programs in need most.

## Before you start
- Donwload the latest version of arch linux from <a href="https://archlinux.org/download/">this</a> link
- Create a bootable usb (recomend using <a href="https://rufus.ie/en/">rufus</a>)
- Boot into the usb and follow the guide
## Installation

### Verify you are in efi
\# ls /sys/firmware/efi/efivars
- If the command is executed without errors continue.

### Connect to the internet
- \# iwctl
  - List stations (Probably wlan0)
    - \# device list
  - Find network ssid
    - \# station [station] get-networks
  - Connect to wifi
    - \# station [station] connect [ssid]
- Check is connected
  - \# ping archlinux.org


### Update the system clock
- \# timedatectl set-ntp true

### Partition the disks
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

### Format the partitions
- \# mkfs.fat -F 32 /dev/[efi partition] (if you created it)
- \# mkfs.ext4 /dev/[linux filesystem partition]
- \# mkswap /dev/[swap partition]

### Mount the file systems
- \# mount /dev/[linux filesystem partition] /mnt
- \# mkdir /mnt/efi
  - \# mount /dev/[efi filesystem] /mnt/efi
- \# swapon /dev/[swap]

### Install base system and some extras
- \# pacstrap /mnt base linux linux-firmware networkmanager gvim man-db man-pages texinfo grub efibootmgr os-prober dhcpcd amd-ucode/intel-ucode

### Configure the system
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

### Install bootloader
- \# vim /etc/default/grub
  - Add lines/uncomment
    - GRUB_DISABLE_OS_PROBER=false
- \# grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=GRUB
- \# grub-mkconfig -o /boot/grub/grub.cfg

### Install extras
- \# pacman -S xf86-video-amdgpu/intel(for laptop) pulseaudio-alsa pulseaudio-bluetooth alsa-utils alsa-firmware 
- \# pacman -S xorg plasma-desktop dolphin dolphin-plugins ark kitty gwenview plasma-nm plasma-pa kdeplasma-addons kde-gtk-config powerdevil sddm sddm-kcm bluez bluedevil kscreen kinfocenter plasma-systemmonitor ffmpegthumbs gedit sudo
- \# pacman -S --needed base git 

### Create new user
- \# useradd -m -u 1000 -G wheel,audio,kvm,input,storage [name] 
- \# passwd name
- \# vim /etc/sudoers
  - Remove comment in wheel 

### Enable sddm and nm
- \# systemctl enable sddm
- \# systemctl enable NetworkManager

### Finish up
- Reboot into the system
  - \# reboot

<br></br>
# ArchLinux - Essentials

## Core
- Aliases
  - \# vim ~/.bash_aliases
    - Add "command='command'"
  - \# vim ~/.bashrc
    - if [ -f ~/.bash_aliases ]; then \\. ~/.bash_aliases \\ fi
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
- Set timezone for clock to work
- Find windows in grub
  - \# paru ntfs-3g
  - Create mount point 
    - \# sudo mkdir /mnt/windows
  - Mount the windows drive
    - \# sudo mount /dev/sda3 /mnt/windows/
  - \# os-prober
  - Edit grub-customizer
- Chinese characters
  - \# paru noto-fonts-cjkD
- Emoji
  - \# paru emoji   

## Programms
|     App Name    |                                Command                               | .conf file |
|:----------------|:---------------------------------------------------------------------|:----------:|
| Anydesk         | \# paru anydesk                                                      |      -     |
| Arduino         | \# paru arduino                                                      |      -     |
| Chrome          | \# paru Chrome                                                       |      -     |
| Clion           | \# paru clion &&  \# paru clion-jre                                  |      -     |
| CopyQ           | \# paru copyq                                                        |[copyq.txt](https://github.com/AlexanderStavrop/Arch-Essentials/files/8457083/copyq.txt)|
| Discord         | \# paru discord                                                      |      -     |
| Flameshot       | \# paru flameshot                                                    |[flameshot.txt](https://github.com/AlexanderStavrop/Arch-Essentials/files/8308980/flameshot.txt)|
| Google  Drive   | ???????????????????                                                  |      -     |
| Gestures        | \# paru gestures & <br /><br /><br />   \# paru libinput-gestures                         |      ?     |
| Grub Customizer | \# paru grub-customizer                                              |      -     |
| Intellij        | \# paru intellij-idea-ultimate && \# paru intellij-idea-ultimate-jre |      -     |
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
| Winrar          | \# paru unrar                                                        |      -     |
| WizTree         | \# paru filelight                                                    |      -     |
| Yt-dlp          | \# paru yt-dlp                                                       |    file    |
| Zoom            | \# paru zoom                                                         |      -     |

## General Configuration
- <a href="https://www.reddit.com/r/wallpaper/top/?t=all">Wallpaper</a>
- Desktop Entyties.
