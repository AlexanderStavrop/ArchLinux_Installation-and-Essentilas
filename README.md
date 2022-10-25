<!--- neofetch
      cmatrix
-->

# ArchLinux - Installation

A guide for installing arch linux and the programs in need most.

## Before you start
- Donwload the latest version of arch linux from <a href="https://archlinux.org/download/">this</a> link
- Create a bootable usb (recomend using <a href="https://rufus.ie/en/">rufus</a>)
- Boot into the usb and follow the guide
  - MBR for desktop
  - GTP for laptop  
## Installation

### Verify you are in efi
- \# ls /sys/firmware/efi/efivars
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
  
  
  <table>
    <tr>
        <th colspan=3 align="center"> For MBR (My PC) </th> <th> </th> <th colspan=3 align="center"> For GTP (My Laptop) </th>
    </tr>
    <tr>
      <td align="center"> Key </td> <td align="center"> Type </td> <td align="center"> Size 
      </td> <td>
      <td align="center"> Key </td> <td align="center"> Type </td> <td align="center"> Size     
    </tr>
    <tr>
      <td align="center"> o </td> <td> Create a Dos table?!   </td> <td align="center"> - </td> 
      <td> </td>
      <td align="center"> g </td> <td> Create Partition table </td> <td align="center"> - </td>
    </tr>
    <tr>
      <td align="center"> - </td> <td align="center"> - </td> <td align="center">  -  </td>
      <td> </td>
      <td align="center"> n </td> <td> Efi partition    </td> <td align="center"> +1G </td>
    </tr>
    <tr>
      <td align="center"> - </td> <td align="center">   -   </td> <td align="center"> - </td>
      <td> </td>
      <td align="center"> t </td> <td> Efi partition - (1) </td> <td align="center"> - </td>
    </tr>
    <tr>
      <td align="center"> n </td> <td> (p) - Root partition </td> <td align="center"> -1G </td>
      <td> </td>
      <td align="center"> n </td> <td> Root partition       </td> <td align="center"> -1G </td>
    </tr>  
    <tr>
      <td align="center"> t </td> <td> (p) - Linux root x86-64 - (83) </td> <td align="center"> - </td>
      <td> </td>
      <td align="center"> t </td> <td> Linux root x86-64 - (23)       </td> <td align="center"> - </td>
    </tr>
    <tr>
      <td align="center"> n </td> <td> (p) - Swap partition </td> <td align="center"> +1G </td>
      <td> </td>
      <td align="center"> n </td> <td> Swap partition       </td> <td align="center"> +1G </td>
    </tr>  
    <tr>
      <td align="center"> t </td> <td> (p) - Linux swap - (82) </td> <td align="center"> - </td>
      <td> </td>
      <td align="center"> t </td> <td> Linux swap - (19)       </td> <td align="center"> - </td>
    </tr>
</table>
 - Verify all your partitions are ok and exit (w)

### Format the partitions
- \# mkfs.fat -F 32 /dev/[efi partition] (if you created it)
- \# mkfs.ext4 /dev/[linux filesystem partition]
- \# mkswap /dev/[swap partition]

### Mount the file systems
- \# mount /dev/[linux filesystem partition] /mnt <!--(**Only GTP**)-->
- \# mkdir /mnt/efi(**Only GTP**)
  - \# mount /dev/[efi filesystem] /mnt/efi
- \# swapon /dev/[swap]

### Install base system and some extras
- \# pacstrap /mnt base linux linux-firmware networkmanager gvim man-db man-pages texinfo grub efibootmgr os-prober dhcpcd amd-ucode/intel-ucode

### Configure the system
- \# genfstab -U /mnt >> /mnt/etc/fstab
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
- GTP
  - \# grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=GRUB
- MBR
  - \# grub-install --target=i386-pc /dev/"root_partition" (if error try /dev/sda or your drive)
- \# grub-mkconfig -o /boot/grub/grub.cfg

### Install extras
- \# pacman -S xf86-video-amdgpu/intel(for laptop) nvidia nvidia-utils pulseaudio-alsa pulseaudio-bluetooth alsa-utils alsa-firmware 
- \# pacman -S xorg plasma-desktop dolphin dolphin-plugins ark kitty gwenview plasma-nm plasma-pa kdeplasma-addons kde-gtk-config powerdevil sddm sddm-kcm bluez bluedevil kscreen kinfocenter plasma-systemmonitor ffmpegthumbs ntfs gedit sudo
- \# pacman -S --needed base git 

### Create new user
- \# useradd -m -u 1000 -G wheel,audio,kvm,input,storage [name] 
- \# passwd name
- \# vim /etc/sudoers
  - Remove comment in the first wheel 

### Enable sddm and nm
- \# systemctl enable sddm
- \# systemctl enable NetworkManager

### Finish up
- Exit Chroot
  - \# exit   
- Reboot into the system
  - \# reboot

<br></br>
# ArchLinux - Essentials

## Core
- Pacman configuration
  - \# sudo vim /etc/pacman.conf
    - Uncomment ParallelDownloads = 5
    - Uncomment colors
- Paru
  - \# sudo pacman -S rustup base-devel
  - \# rustup install stable
  - \# rustup default stable
  - \# git clone https://aur.archlinux.org/paru.git
  - \# cd paru
  - \# makepkg -si
  - \# sudo vim /etc/paru.conf 
    - Enable BottomUp
  - \# paru reflector
- Bluetooth
  - \# sudo systemctl start bluetooth.service
  - \# sudo systemctl enable bluetooth.service
- Network Tools
  - \# paru net-tools
- Emoji
  - \# paru emoji   
- Chinese characters
  - \# paru noto-fonts-cjk
- Find windows on different disk in grub
  - Create mount point 
    - \# sudo mkdir /mnt/windows
  - Mount the windows drive
    - \# sudo mount /dev/sda3 /mnt/windows/
  - \# os-prober
  - Edit grub-customizer
- Aliases
  - \# vim ~/.bash_aliases
    - Add "command='command'"
  - \# vim ~/.bashrc
    - if [ -f ~/.bash_aliases ]; then \\. ~/.bash_aliases \\ fi

## Formating and automounting disks
### Formating 
  - List the available disks
    - \# sudo fdisk -l
  - Format the disk (if needed)
    - \# sudo fdisk /dev/sdX
      - Delete old partition (d)
      - Create new partition (n)
      - Write the partition  (w)
  - Create the filesystem
    - \# sudo mkfs.ext4 /dev/sdXY
  - Add disk label
    - \# sudo e2label /dev/sdXY "label"    

### Automounting
  - Make mount directoty
    - \# sudo mkdir /media/"dir name"
    - \# sudo chown -R "usr" /media/"dir name" 
  - Copy the UUID
    - \# sudo blkid /dev/sdXY  
  - Edit the fstab file !!!
    - \# sudo vim /etc/fstab
      - Add entry UUID="UUID" \t /media/"dir name" \t ext4 \t defaults \t 0 \t 0

## Programms

### Tools
|     App Name    |                                                  Command                                                          | .conf file |
|:----------------|:------------------------------------------------------------------------------------------------------------------|:----------:|
| CopyQ           | \# paru copyq                                                                                                     |[copyq.txt](https://github.com/AlexanderStavrop/Arch-Essentials/files/8457083/copyq.txt)                                                                      |
| Filelight       | \# paru filelight                                                                                                 |      -     |
| Flameshot       | \# paru flameshot                                                                                                 |[flameshot.txt](https://github.com/AlexanderStavrop/Arch-Essentials/files/8308980/flameshot.txt)                                                                  |
| Github          | \# git config --global credential.helper store <br />                                                                                                       \# git pull "a private repository" <br />                                                                                                                   Enter the username and password <br />                                                                                                                     \# git config --global user.email "you@example.com" <br />                                                                                                 \# git config --global user.name "Your Name"                                                                      |      -     |
| Grub Customizer | \# paru grub-customizer                                                                                           |      -     |
| Libre office    | \# paru libreoffice-fresh                                                                                         |      -     |
| Obsidian        | \# paru obsidian                                                                                                  |      -     |
| Okular          | \# paru okular                                                                                                    |      -     |
| Virtual mic     | \# paru pulseaudio-virtualmic <br />                                                                                                                       install Lanmic on your android phone <br />                                                                                                                 Open the app, select RTSP and press the on button <br />                                                                                                   \# virtualmic rtsp//"ip"                                                                                          |      -     |
| Winrar          | \# paru unrar                                                                                                     |      -     |
| Xow             | \# paru xow <br />                                                                                                                                         \# sudo xow-get-firmware.sh                                                                                       |      -     |       
| Yt-dlp          | \# paru yt-dlp  <br /> Add configuration file at /home/alex/.config/yt-dlp/                                       |[config.txt](https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/files/9861099/config.txt)                                               |

### Programming / Editors
|     App Name    |                                                  Command                                                          |
|:----------------|:------------------------------------------------------------------------------------------------------------------|
| Arduino         | \# paru arduino                                                                                                   |
| Clion           | <a href="https://www.jetbrains.com/clion/download/#section=linux">Download</a> <br />                                                                       Tools -> Create Desktop Entity                                                                                    |
| Diligence Adept | \# paru digilent.adept.runtime <br />                                                                                                                       \# paru digilent.adept.utilities <br />                                                                                                                     [example.txt](https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/files/8865697/example.txt)|
| Intellij        | <a href="https://www.jetbrains.com/idea/download/#section=linux">Download</a> <br />                                                                       Tools -> Create Desktop Entity <br />                                                                                                                       **Java** <br />                                                                                                                                             Download latest jdk <br />                                                                                                                                 **Python** <br />                                                                                                                                           Cntrl + alt + shift + s -> Plugins -> Search for python -> Download -> Apply   <br />                                                                       File -> new -> Project -> Python -> Select Interpreter                                                            |       
| Ise             | \# paru ncurses5-compat-libs <br />                                                                                                                         <a href="https://www.xilinx.com/member/forms/download/xef.html?filename=Xilinx_ISE_DS_Lin_14.7_1015_1.tar">Download</a> <br />                             <a href="https://youtu.be/yzEIQLQZYpk?t=203">Installation Video</a> <br />                                                                                 \# sudo chmod +rwx settings64.sh <br />                                                                                                                     \# ./settings64.sh <br />                                                                                                                                   \# cd /opt/Xilinx/14.7/ISE_DS/ISE/bin/lin64/ <br />                                                                                                         \# ./ise <br />                                                                                                                                             <a href="https://xilinx.entitlenow.com/AcrossUser/main.gsp?product=&tab=&req_hash=&"> Aquire the license file </a> <br />                                   <a href="https://wiki.archlinux.org/title/Xilinx_ISE_WebPACK#Launching_via_desktop_icons"> Create Desktop Entity </a> <br />                               \# paru jetbrains-mono <br />                                                                                                                               Edit -> Preferences -> Ise Text Editor -> Change font/tab width                                                   |
| Matlab          | ???????????????????                                                                                               |
| TexStudio       | \# paru texlive-science     <br />                                                                                                                         \# paru texlive-latexextra  <br />                                                                                                                         \# paru texlive-langgreek   <br />                                                                                                                         \# paru texlive-core        <br />                                                                                                                         \# paru texlive-bin         <br />                                                                                                                         \# paru texlive-bibtexextra <br />                                                                                                                         \# paru texstudio                                                                                                 |  

### Other
|     App Name    |                                                  Command                                                          |
|:----------------|:------------------------------------------------------------------------------------------------------------------|
| Anydesk         | \# paru anydesk                                                                                                   |
| Discord         | \# paru discord                                                                                                   |
| Firefox         | \# paru firefox <br />                                                                                                                                     Inside Firefox search for **about:config** -> Accept the risk -> Change **ui.key.menuAccessKeyFocuses** to false  |
| Kate            | \# paru kate                                                                                                      |
| Obs             | \# paru obs-studio                                                                                                |
| Psensor         | \# paru psensor                                                                                                   |
| qbittorrent     | \# paru qbittorrent                                                                                               |
| Signal          | \# paru signal-desktop                                                                                            |
| Spotify         | \# paru Spotify                                                                                                   |
| Steam           | \# sudo vim /etc/pacman.conf <br />                                                                                                                         Uncomment multilib and Include <br />                                                                                                                       \# paru steam <br />                                                                                                                                       \# paru arial <br />                                                                                                                                       steam -> settings -> steam play -> Enable steam play, select latest proton  <br />                                                                         If games don't start -> \# paru -Rsn amdvlk && \# paru -Rsn lib32-amdvlk                                          |
| Vlc             | \# paru vlc                                                                                                       |

## General Configuration
- <a href="https://www.reddit.com/r/wallpaper/top/?t=all">Wallpaper</a>
- Desktop Entyties.

- Make kitty pretty
  - Copy content of [bashrc.txt](https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/files/8854679/bashrc.txt)
  - \# notepadqq .bashrc
