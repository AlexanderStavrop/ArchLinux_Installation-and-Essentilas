
# Table of Contents
- <a href="https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/blob/master/README.md#before-you-start">Before you start</a>
- <a href="https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/blob/master/README.md#installation">Installation</a>
    - <a href="https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/blob/master/README.md#verify-you-are-in-efi">Verify you are in EFI</a>
    - <a href="https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/blob/master/README.md#connect-to-the-internet">Connect to the Internet</a>
    - <a href="https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/blob/master/README.md#update-the-system-clock">Update the system clock</a>
    - <a href="https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/blob/master/README.md#partition-the-disks">Partition the disks</a>
    - <a href="https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/blob/master/README.md#install-base-system-and-some-extras">Install base system and some extras</a>
    - <a href="https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/blob/master/README.md#configure-the-system">Configure the system</a>
- <a href="https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/blob/master/README.md#archlinux---configuration">Archlinux - Configuration</a>
    - <a href="https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/blob/master/README.md#system-settings-configuration">System settings configuration</a>
    - <a href="https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/blob/master/README.md#dolphin-configuration">Dolphin configuration</a>
    - <a href="https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/blob/master/README.md#pacman-and-paru-configuration">Pacman and Paru configuration</a>
    - <a href="https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/blob/master/README.md#wifi-and-bluetooth-configuration">Wifi and Bluetooth configuration</a>
    - <a href="https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/blob/master/README.md#grub-configuration">Grub configuration</a>
    - <a href="https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/blob/master/README.md#formating-and-automounting-disks">Formanting and automounting disks</a>
    - <a href="https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/blob/master/README.md#programs">Programs</a>
    - <a href="https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/blob/master/README.md#task-bar-configuration">Task bar configuration</a>


# Before you start
- Donwload the latest version of arch linux from <a href="https://archlinux.org/download/">this</a> link
- Create a bootable usb 
    - ***Windows*** 
        - <a href="https://rufus.ie/en/">rufus</a>
        - Check if you need an MBR or GPT usb
    - ***Linux***
        - Find the usb device name
        ```
        sudo fdisk -l
        ```
        - Install pv
        - Burn the usb
        ```
        sudo dd if=[arch installer file] | pv | sudo dd of=/dev/[usb device name]
        ```
- Boot into the usb and follow the guide
    - MBR for desktop
    - GTP for laptop  
# Installation

## Verify you are in efi
```
ls /sys/firmware/efi/efivars
```
- If the command is executed without errors continue.
    
## Connect to the internet
- Establish connection
    ```
    iwctl
    ```
    - List stations (Probably wlan0)
        ```
        device list
        ```
    - Find network ssid
        ```
        station [station] get-networks
        ```
    - Connect to wifi
        ```
        station [station] connect [ssid]
        ```
    - Exit
        ```
        exit
        ```
- Check is connected
    ```
    ping 8.8.8.8
    ```

## Update the system clock
```
timedatectl set-ntp true
```

## Partition the disks
- List disks and choose the correct drive
    ```
    fdisk -l
    ```
- Edit the target drive
    ```
    fdisk /dev/[Targe Drive]
    ```  
  
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
      <td align="center"> t </td> <td> (p) - Linux - (83)       </td> <td align="center"> - </td>
      <td> </td>
      <td align="center"> t </td> <td> Linux root x86-64 - (23) </td> <td align="center"> - </td>
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

## Format the partitions
- If an Efi Partition has been created
    ```
    mkfs.fat -F 32 /dev/[efi Partition] 
    ```
```
mkfs.ext4 /dev/[linux filesystem partition]
```
```
mkswap /dev/[swap partition]
```

## Mount the file systems
- Create a mounting point for Efi Partition (**Only GTP**)
    ```
    mkdir /mnt/efi 
    ```
    - Mount the Efi Partition
        ```section
        mount /dev/[efi filesystem Partition] /mnt/efi
        ```
- Mount the Root Partition
    ```
    mount /dev/[linux filesystem Partition] /mnt
    ```
- Set the Swapon Partition
    ```
    swapon /dev/[Swap Partition]
    ```
    
## Install base system and some extras
```
pacstrap /mnt base linux linux-firmware networkmanager gvim man-db man-pages texinfo grub
              efibootmgr os-prober dhcpcd amd-ucode/intel-ucode
```

## Configure the system
- Generate the fstab file
    ```
    genfstab -U /mnt >> /mnt/etc/fstab
    ```
- Enter root
    ```
    arch-chroot /mnt
    ```
    - ### Time and time-zone configuration
        ```
        ln -sf /usr/share/zoneinfo/Europe/Athens /etc/localtime
        ```
        ```
        hwclock --systohc
        ```
    
    - ### Locale configuration
        - Edit the locale.gen file
            ```
            vim /etc/locale.gen 
            ```
            - Uncommnent wanted locales
                - el_GR.UTF-8 UTF-8
                - en_US.UTF-8 UTF-8	
        - Generate the locale file
            ```
            locale-gen
            ```
        - Edit the locale.conf file
            ```
            vim /etc/locale.conf 
            ```
            - Add lines
                ```
                LANG=en_US.UTF-8
                LC_TIME=el_GR.UTF-8
                ```
   
    - ### Network configuration
        - Edit the hostname 
            ```
            vim /etc/hostname
            ```
            - Add line
                ```
                [myhostname]
                ```
        - Edit the hosts
            ```
            vim /etc/hosts 
            ```
            |  	        |		           |
            |    :-: 	|        :-: 	   |
            | 127.0.0.1 | localhost        | 
            | :::1      | **[myhostname]** | 
            | 127.0.1.1 | **[myhostname]** | section
            | ff02::1    | ip6-allnodes     | 
            | ff02::2    | ip6-allrouters   | 	 
    
    - ### Create a password
        ```
        passwd
        ```
        
    - ### Install bootloader
        - Edit the grub file
            ```
            vim /etc/default/grub
            ```
            - Change **GRUB_TIMEOUT** to 2 
            - Uncomment
                ```
                GRUB_DISABLE_OS_PROBER=false
                ```
        - Install grub
            - GTP
                ```
                grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=GRUB
                ```
            - MBR
                ```
                grub-install --target=i386-pc /dev/[root_partition]
                ```
                - (if an error occurs, try /dev/[Target Drive])
        - Generate the grub.cfg file
            ```
            grub-mkconfig -o /boot/grub/grub.cfg
            ```
            
    - ### Install some more extras
        ```
        pacman -S (for laptop)xf86-video-amdgpu/intel nvidia nvidia-utils pulseaudio-alsa pulseaudio-bluetooth
                   alsa-utils alsa-firmware xorg plasma-desktop plasma-systemmonitor plasma-nm plasma-pa dolphin
                   dolphin-plugins kscreen kinfocenter kdeplasma-addons kde-gtk-config ark konsole kitty gwenview
                   powerdevil sddm sddm-kcm bluez bluedevil ffmpegthumbs ntfs-3g gedit sudo btop
        ```
        ```
        pacman -S --needed base git 
        ```
        
    - ### Create new user
        - Create user
            ```
            useradd -m -u 1000 -G wheel,audio,kvm,input,storage [name]
            ```
        - Create user password
            ```
            passwd [name]
            ```
        - Add user to sudoers
            ```
            vim /etc/sudoers
            ```
            - Remove comment in the **first** wheel 

    - ### Enable sddm and nm
        ```
        systemctl enable sddm
        ```
        ```
        systemctl enable NetworkManager
        ```
              
   - ### Exit Chroot    
      ```
      exit
      ```
      
- Reboot and remove the usb
  ```Behavior
  reboot
  ```
<br></br>
# ArchLinux - Configuration

## System settings configuration
Open **System Settings**
- ***Quick Settings***
    - In **Quick Settings** section
        - *Theme* &rarr; Select "Breeze Dark"
        - ![image](https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/assets/56675566/7ccf51c8-c312-460c-9138-ee43ab58b010)
        - *Clicking flies or folders* &rarr; Select "Selects them"
- ***Window Decorations***
    - In the **Appearance** section
        - In **Window Decorations**
            - In the **Titlebar Buttons** tab
                - Remove "Pin icon"
                - Remove "Question mark"
- ***Splash Screen***
    - In the **Appearance** section
        - In **Splace screen** tab
            - Press "Get New Splace screens"
            - Search for "WatchDogsBootSplash"
- ***Change alt-tab look***
    - In the **Window Management** section
        - In the **Task Switcher** tab
            - *Visualization* &rarr; "Thubnail Grid"
- ***Login Screen and logout behavior***
    - In the **Startup and Shutdown** section
        - In **Login Screen (SDDM)** tab
            - Select "Breeze"  
        - In **Desktop session** tab 
            - *Show* &rarr; Uncheck
            - *On login* &rarr; Select "Start with an empty session"
- ***File Association***
    - In the **Applications** section
        - In the **File Associations** tab
            - *text (Plain)* 
                - *Application Preference Order* &rarr; "Gedit", "Notepadqq", "Vim"
            - *mp3*
                - *Application Preference Order* &rarr; "mpv Media Player", "Vlc"
            - *mp4*
                - *Application Preference Order* &rarr; "mpv Media Player", "Vlc"
            - *mkv*
                - *Application Preference Order* &rarr; "mpv Media Player", "Vlc" 
        - In **Default Applications**
            - Change "Terminal emulator" to "kitty"
- ***Touchpad and keyboard settings***            
    - In the **Input Devices** section
        - In the **Keyboard** tab
            - Navigate to **Layouts**
                - Enable "Configure Layouts"
                - Press add layout and add "Greek"
            - Navigate to **Advanced**
                - Enable "Configure keyboard options"
                - Expand "Switching to another layout" &rarr; Select "alt + shift" and "Win + Space"
        - In the **Touchpad** tab
            - *Tapping*
                - Enable **Tap-to-click**
                - Enable **Tap-and-drag**      
            - *Scrolling* &rarr; Enable "Invert scroll direction (Natural scrolling)"
            - *Right-click* &rarr; Enable "Press anywhere with two fingers"
- ***Screen dimming***
    - In the **Power Management** section
        - In the **Energy Saving** tab
            - *On Battery*
                - *Dim screen* &rarr; "5 min"
                - *Screen Energy Saving* &rarr; "10 min"
            - *On Low Battery*
                - Uncheck "Screen brightness" 
                - *Dim screen* &rarr; "2 min"
                - *Screen brightness* &rarr; "5 min" 

## Dolphin configuration
- ***Edit entries on the left***
    - In **places** 
        - *leave only*
            - "Home"
            - "Desktop"
            - "Documents"
            - "Downloads"
            - "Trash"
        - *Hide*
            - "Remote" section
            - "Recent" section
- ***Settings***
    - Click on the **Burget Menu** on the top right corner 
        - Enable "Show Hidden Files"
        - Hover over **Show Additional Information** and enable "Size"
        - Hover over **configure** and select "Configure Toolbars" 
            - *Remove*
                - "Separator"  
                - "Icons"
                - "Compact"
                - "Details"
                - "Splits"
                - "Stash"
            - *Add*
                - "Home" after "forward"
        - Hover over **configure** and select "Configure Dolphin"
            - In the **General** section
                - In the **Behavior** tab
                    - *View* &rarr; "Remember display style for each folder"
                - In the **confirmation** tab
                    - *Ask for confimation in Dolphin when* &rarr; uncheck "Closing windows with multiple tabs"
            - In the **Startup** section 
                - *Show on startup* &rarr; Change path to ***~/Documents***
            - In **View Modes** section
                - In **Icons** tab        
                    - *Label width* &rarr; "small"
            - In the **Context Menu** section
                - *Uncheck*
                    - "Add to places"
                    - "Duplicate Here"
                    - "Forget items from Recent Documents and Recent Locations"
                    - "Install.."
                    - "Link Files to Activities"
                    - "Start a Slideshow"
                    - "Tags"
                    - "View modes"
            - In the **Trash** section
                - *Cleanup* 
                    - *Delete files older than* &rarr; "15"


## Pacman and Paru configuration
- ***Pacman***
    ```
    sudo vim /etc/pacman.conf
    ```
    - *Uncomment* 
        - "color"
        - "ParallelDownloads=5"
        - "Multilib"

- ***Paru***
    ```
    sudo pacman -S rustup base-devel
    ```
    ```
    rustup install stable
    ```
    ```
    rustup default stable
    ```
    ```
    git clone https://aur.archlinux.org/paru.git
    ```
    ```
    cd paru
    ```
    ```
    makepkg -si
    ```
    ```
    sudo vim /etc/paru.conf 
    ```
    - Enable "BottomUp"
    ```
    paru reflector
    ```


## Wifi and Bluetooth configuration
- ***Change to Cloudflare***
    - Click on the **internet icon** (Wifi or Ethernet) and press **configure**
        - In the *IPV4*
            - *Method* &rarr; "Automatic (Only Addresses)"
            - *DNS Servers* &rarr; ```1.1.1.1```
            - *Search Domains* &rarr; ```1.0.0.1```
        - In the *IPV6*
            - *Method* &rarr; "Automatic (Only Addresses)"
            - *DNS Servers* &rarr; ```::1.1.1.1```
            - *Search Domains* &rarr; ```::1.0.0.1```
        - Logout and login back again
        - <a href="https://1.1.1.1/help">Check if you are using Cloudflare</a>

- ***Network Tools***
    ```
    paru net-tools
    ```

- ***Bluetooth***
    ```
    sudo systemctl start bluetooth.service
    ```
    ```
    sudo systemctl enable bluetooth.service
    ```


## Grub Configuration
- ***Find windows on different disk in grub***
    - Create mount point 
        ```
        sudo mkdir /mnt/windows
        ```
    - Mount the windows drive
        ```
        sudo mount /dev/[Windows Partition] /mnt/windows/
        ```
    - Find windows
        ```
        sudo os-prober
        ```
    - Update grub
        ```
        sudo grub-mkconfig -o /boot/grub/grub.cfg
        ```
        
- ***Find another Linux installation in grub***
    - Find the other distro
        ```
        sudo os-prober
        ```
    - Update grub
        ```
        sudo grub-mkconfig -o /boot/grub/grub.cfg
        ```

- ***Grub menu entries***
    - Backup grub.cfg
        ```
        sudo cp /boot/grub/grub.cfg ~/Desktop/
        ```
    - Edit the grub.cfg
        ``` 
        sudo vim /boot/grub/grub.cfg
        ```
        - Search for "menuentry"
            ```
            /menuentry
            ```
        - comment (or delete) any entries you don't want to see in your grub boot menu
        - Save and exit
        
## Formating and automounting disks
- ***Formating*** 
    - List the available disks
        ```
        sudo fdisk -l
        ```
    - Format the disk (if needed)
        ```
        sudo fdisk /dev/[Target Disk]
        ```
        - Delete old partition (d)
        - Create new partition (n)
        - Write the partition  (w)
    - Create the filesystem
        ```
        sudo mkfs.ext4 /dev/[Target Partition]
        ```
    - Add disk label
        ```
        sudo e2label /dev/[Target Partition] [label]
        ```

- ***Automounting***
    - Make mount directoty
        ```
        sudo mkdir /media/[dir name]
        ```
        ```
        sudo chown -R [usr] /media/[dir name]
        ```
    - Copy the UUID
        ```
        sudo blkid /dev/[Target Partition]  
        ```
    - Edit the fstab file !!!
         ```
        sudo vim /etc/fstab
        ```
        - Add line
            ```
            UUID=[UUID] \t /media/[dir name] \t ext4 \t defaults \t 0 \t 0
            ```
            
## Programs
### Tools
|     App Name    |                                                  Command                                                          |  .conf file  |
|:----------------|:------------------------------------------------------------------------------------------------------------------|:------------:|
| CopyQ           | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru copyq<br />                                                                                                                                                                                                                                                                                                                                 ***Configuration File***<br />                                                                                                                                                                                                                                                                                                                      Add the config file in the **~/.cofig/copyq/** directory<br />                                                                                                                                                                                                                                                                                      ***Shortcut***<br />                                                                                                                                                                                                                                                                                                                                Open copyQ and navigate to **File -> Preferences -> Shortcuts**<br />                                                                                                                                                                                                                                                                               Add **ctrl + `** as **show/hide main menu**<br />                                                                                                                                                                                                                                                                                                   ***If copyQ doesn't Autostart***<br />                                                                                                                                                                                                                                                                                                              Open copyQ and navigate to **File -> Preferences**<br />                                                                                                                                                                                                                                                                                            In the **General** tab check the **Autostart**                                                                    |[copyq.conf](https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/files/12137321/copyq.txt)                                                                                                                                                                                                                                                  |
| Filelight       | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru filelight                                                                                                 |       -      |        
| Flameshot       | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru flameshot<br />                                                                                                                                                                                                                                                                                                                             ***Configuration File***<br />                                                                                                                                                                                                                                                                                                                      Run the application and import the config file<br />                                                                                                                                                                                                                                                                                                ***Shortcut***<br />                                                                                                                                                                                                                                                                                                                                Navigate to **System settings -> Shortcuts**<br />                                                                                                                                                                                                                                                                                                  Select **Add application** and search for **flameshot**<br />                                                                                                                                                                                                                                                                                       Set **prtsc** as shortcut for **Take screenshot**<br />                                                                                                                                                                                                                                                                                             ***If flameshot doesn't Autostart***<br />                                                                                                                                                                                                                                                                                                          Navigate to **System Settings -> Startup and Shutdown -> Autostart**<br />                                                                                                                                                                                                                                                                          On the bottom, select **add** and then **add application** and search for flameshot                               |[flameshot.conf](https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/files/10970695/flameshot.txt)                                                                                                                                                                                                                                          |
| Github          | \# git config --global credential.helper store<br />                                                                                                                                                                                                                                                                                                \# git clone "a private repository"<br />                                                                                                                                                                                                                                                                                                           Enter the username and password<br />                                                                                                                                                                                                                                                                                                               \# git config --global user.email "you@example.com"<br />                                                                                                                                                                                                                                                                                           \# git config --global user.name "Your Name"                                                                      |       -      |
| Kde Connect     | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru kdeconnect                                                                                                |       -      |
| Kitty           | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru kitty                                                                                                     | |
| Libre office    | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru libreoffice-fresh                                                                                         |       -      |
| Notion          | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru notion-app (Download the **Aur** package)                                                                 |       -      |
| Okular          | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru okular                                                                                                    |       -      | 
| qbittorrent     | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru qbittorrent<br />                                                                                                                                                                                                                                                                                                                           ***Configuration***<br />                                                                                                                                                                                                                                                                                                                           Open the application and navigate to **Tools -> preferences -> Downloads**<br />                                                                                                                                                                                                                                                                    Enable **Delete .torrent files afterwards**<br />                                                                                                                                                                                                                                                                                                   Change **Default Save Path**                                                                                      |       -      |
| Winrar and zips | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru unrar<br />                                                                                                                                                                                                                                                                                                                                 \# paru unzip<br />                                                                                                                                                                                                                                                                                                                                 \# paru p7zip                                                                                                     |       -      |
| Xow             | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru xow<br />                                                                                                                                                                                                                                                                                                                                   \# sudo xow-get-firmware.sh                                                                                       |       -      |       
| Yt-dlp          | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru yt-dlp<br />                                                                                                                                                                                                                                                                                                                                ***Configuration***<br />                                                                                                                                                                                                                                                                                                                           Remove the **.txt** extension<br />                                                                                                                                                                                                                                                                                                                 \# sudo mkdir ~/.config/yt-dlp<br />                                                                                                                                                                                                                                                                                                                \# sudo mv ~/Downloads/config ~/.config/yt-dlp/                                                                   |[config](https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/files/10914543/config.txt)                                                                                                                                                                                                                                                     |

### Programming / Editors
|     App Name       |                                                  Command                                                                  |
|:-------------------|:--------------------------------------------------------------------------------------------------------------------------|
| Arduino            | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru arduino<br />                                                                                                                                                                                                                                                                                                                               ***Cannot open port /dev/ttyXXXX***<br />                                                                                                                                                                                                                                                                                                           \# sudo chmod a+rw /dev/ttyXXXX<br />                                                                                                                                                                                                                                                                                                               ***avrdude: stk500_recv(): programmer is not responding***<br />                                                                                                                                                                                                                                                                                                                                                                                                             |
| Clion              | ***Download***<br />                                                                                                                                                                                                                                                                                                                                <a href="https://www.jetbrains.com/clion/download/#section=linux">Download</a><br />                                                                                                                                                                                                                                                                ***Desktop Entity***<br />                                                                                                                                                                                                                                                                                                                          Tools -> Create Desktop Entity                                                                                            |
| Intellij           | ***Download***<br />                                                                                                                                                                                                                                                                                                                                <a href="https://www.jetbrains.com/idea/download/#section=linux">Download</a><br />                                                                                                                                                                                                                                                                 ***Desktop Entity***<br />                                                                                                                                                                                                                                                                                                                          Tools -> Create Desktop Entity<br />                                                                                                                                                                                                                                                                                                                ***Java***<br />                                                                                                                                                                                                                                                                                                                                    When creating a new project, select java as **Language** and download latest jdk from the **JDK** drop down menu<br />                                                                                                                                                                                                                              ***Python***<br />                                                                                                                                                                                                                                                                                                                                  When creating a new project, press the **+** button in **Language**,select python and download the python module <br />   | 
| Ise                | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru ncurses5-compat-libs<br />                                                                                                                                                                                                                                                                                                                  <a href="https://www.xilinx.com/member/forms/download/xef.html?filename=Xilinx_ISE_DS_Lin_14.7_1015_1.tar">Download</a><br />                                                                                                                                                                                                                       ***Installation***<br />                                                                                                                                                                                                                                                                                                                            <a href="https://youtu.be/aDPWtv_CC0Y?t=203">Installation Video</a><br />                                                                                                                                                                                                                                                                           \# sudo chmod +x settings64.sh<br />                                                                                                                                                                                                                                                                                                                \# ./settings64.sh<br />                                                                                                                                                                                                                                                                                                                            \# cd /opt/Xilinx/14.7/ISE_DS/ISE/bin/lin64/<br />                                                                                                                                                                                                                                                                                                  \# ./ise<br />                                                                                                                                                                                                                                                                                                                                      <a href="https://xilinx.entitlenow.com/AcrossUser/main.gsp?product=&tab=&req_hash=&"> Aquire the license file </a><br />                                                                                                                                                                                                                            ***Desktop Entity***<br />                                                                                                                                                                                                                                                                                                                          <a href="https://wiki.archlinux.org/title/Xilinx_ISE_WebPACK#Launching_via_desktop_icons"> Create Desktop Entity </a><br />                                                                                                                                                                                                                         ***Font***<br />                                                                                                                                                                                                                                                                                                                                    \# paru jetbrains-mono<br />                                                                                                                                                                                                                                                                                                                        Navigate to **Edit -> Preferences -> Ise Text Editor** and Change font and the tab width                                  |
| Vitis              | ***Required Libraries***<br />                                                                                                                                                                                                                                                                                                                      \# paru libxcrypt-compat<br />                                                                                                                                                                                                                                                                                                                      \# paru ncurses5-compat-libs<br />                                                                                                                                                                                                                                                                                                                  ***Download***<br />                                                                                                                                                                                                                                                                                                                                <a href="https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vitis.html">Download the installer</a><br />                                                                                                                                                                                                              ***If QEMU fails to start***<br />                                                                                                                                                                                                                                                                                                                  \# paru time    
| Vivado             | ***Download***<br />                                                                                                                                                                                                                                                                                                                                <a href="https://www.xilinx.com/support/download.html">Download the installer</a><br />                                                                                                                                                                                                                                                             ***Required Libraries***<br />                                                                                                                                                                                                                                                                                                                      \# paru libxcrypt-compat<br />                                                                                                                                                                                                                                                                                                                      \# paru ncurses5-compat-libs                                                                                              |
| Diligence Adept    | ***Download***<br />                                                                                                                                                                                          \# paru digilent.adept.runtime<br />                                                                                                                                                                          \# paru digilent.adept.utilities<br />                                                                                                                                                                        ***Example txt***<br />                                                                                                                                                                                       [example.txt](https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/files/8865697/example.txt)        |
| Matlab             | ***Download***<br />                                                                                                                                                                                                                                        <a href="https://www.mathworks.com/downloads/web_downloads/">Download</a><br />                                                                                                                                                                             ***Installation***<br />                                                                                                                                                                                                                                    \# sudo unzip -X -K "matlab.zip" -d ~/Downloads/matlab<br />   \# cd matlab<br />                                                                                                                                                                           \# ./install<br />                                                                                                                                                                                                                                          ***Desktop Entity***<br />                                                                                                                                                                                                                                  \# touch ~/.local/share/applications/matlab.desktop<br />                                                                                                                                                                                                   \# sudo vim ~/.local/share/applications/matlab.desktop<br />                                                                                                                                                                                                Copy paste the contents of **[MatlabDesktopEntity](https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/files/10944798/MatlabDesktopEntity.txt)**<br />                                                                                \# chmod +x ~/.local/share/applications/matlab.desktop                                                                                                                                                                                                      ***In case the installer fails to open***<br />                                                                                                                                                                                                             \# sudo rm ./bin/glnxa64/libfreetype.so*<br />                                                                                                                                                                                                              \# export LD_PRELOAD=/lib64/libfreetype.so<br />                                                                                                                                                                                                            \# ./install<br />                                                                                                                                                                                                                                          \# Select to download **every** products that has **MATLAB** in and also every other necessary<br />                                                                                                                                                        ***If matlab cannot create new Script***<br />                                                                                                                                                                                                              \# cd "matlab"/bin/glnxa64/<br />                                                                                                                                                                                                                           \# mv libfreetype.so.6 libfreetype.so.6.old<br />                                                                                                                                                                                                           ***Change Matlab commands to normal ones***<br />                                                                                                                                                                                                           Navigate to **HOME -> Preferences -> Keyboard -> Shortcuts**<br />                                                                                                                                                                                          Change **Active Settings** to **Windows Default Set**<br />                                                                    |
| TexStudio          | ***Download***<br />                                                                                                                                                                                          \# paru texstudio<br />                                                                                                                                                                                       texlive-pictures (39)<br />                                                                                                                                                                                   texlive-mathscience (35)<br />                                                                                                                                                                                texlive-latexextra (32)<br />                                                                                                                                                                                 texlive-langgreek (23)<br />                                                                                                                                                                                  texlive-fontutils (10)<br />                                                                                                                                                                                  texlive-bin (4)<br />                                                                                                                                                                                         texlive-bibtexextra (3)<br />                                                                                                                                                                                 texlive-basic (2)<br />                                                                                                                                                                                       \# paru texlive<br />                                                                                                                                                                                         ***Greek Dictionary*** <br />                                                                                                                                                                                 Download the <a href="https://extensions.libreoffice.org/en/extensions/show/orthos-greek-speller-dictionary-and-thesaurus">dictionary</a> and save the file as .zip<br />                                     \# sudo mkdir ~/Documents/"Latex dic"<br />                                                                                                                                                                   \# sudo unzip ~/Downloads/orthos-el_GR-0.4.0-87.zip -d ~/Documents/"Latex dic"/ <br />                                                                                                                        Navigate to **Options -> Configure TexStudio -> Language Checking**<br />                                                                                                                                     Change the directory of **Spelling Dictionary Directories** to **Latex dic**<br />                                                                                                                            Restart TexStudio<br />                                                                                                                                                                                       Navigate again to **Language Checking** and change **Default Languages** to **el_GR**<br />                                                                                                                   ***Font Size***<br />                                                                                                                                                                                         Navigate to **Options -> Configure TexStudio -> Editor** and change the **Font size** to 11                               |

### Other
|     App Name    |                                                  Command                                                                   |
|:----------------|:---------------------------------------------------------------------------------------------------------------------------|
| Anydesk            | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru anydesk-bin                                                                                                     |
| Chromium           | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru chromium<br />                                                                                                                                                                                                                                                                                                                              ***Extensions***<br />                                                                                                                                                                                                                                                                                                                              <a href="https://chrome.google.com/webstore/detail/bitwarden-free-password-m/nngceckbapebfimnlniiiahkandclblb/related?gclid=EAIaIQobChMIyc2W5OPIgAMVVdYWBR3pCw4aEAAYASAAEgKqrvD_BwE">BitWarden</a>                                                                                                                                                                                                                                                                          |
| Discord            | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru discord<br />                                                                                                                                                                                                                                                                                                                               ***Better Discord***<br />                                                                                                                                                                                                                                                                                                                          \# paru betterdiscord-installer-bin<br />                                                                                                                                                                                                                                                                                                           \# betterdiscord-installer<br />                                                                                                                                                                                                                                                                                                                    ***BetterDiscord PluginRepp***<br />                                                                                                                                                                                                                                                                                                                <a href="https://betterdiscord.app/plugin/PluginRepo">PluginRepo</a><br />                                                                                                                                                                                                                                                                          Required library for PluginRepo (<a href="https://betterdiscord.app/plugin/BDFDB">BDFDB</a>)<br />                                                                                                                                                                                                                                                  Open discord and navigate to **Settings -> Plugins** and press **Open Plugins Folder** and add the previously downloaded files<br />                                                                                                                                                                                                                ***Other Plugsins***<br />                                                                                                                                                                                                                                                                                                                          Open discord and navigate to **Settings -> Plugin Repo**<br />                                                                                                                                                                                                                                                                                      Sort by **Likes** and download all the favorite plugsins:<br />                                                                                                                                                                                                                                                                                     1)  CallTimeCounter<br />                                                                                                                                                                                                                                                                                                                           2)  SplitLargeMessage<br />                                                                                                                                                                                                                                                                                                                         3)  BetterVolume<br />                                                                                                                                                                                                                                                                                                                              4)  ImageUtilities<br />                                                                                                                                                                                                                                                                                                                            5)  DoubleClicktoEdit<br />                                                                                                                                                                                                                                                                                                                      6)  HideDisabledEmojis<br />                                                                                                                                                                                                                                                                                                                        7)  FileViewer<br />                                                                                                                                                                                                                                                                                                                                8)  Do not track<br />                                                                                                                                                                                                                                                                                                                              9)  SpellCheck<br />                                                                                                                                                                                                                                                                                                                                10) BetterMediaPlayer                                                                                                   |
| Firefox            | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru firefox<br />                                                                                                                                                                                                                                                                                                                               ***Compact Look***<br />                                                                                                                                                                                                                                                                                                                            Inside Firefox search for **about:config -> browser.compactmode.show** and change to **true**<br />                                                                                                                                                                                                                                                 Right click on toolbar, select **Customize Toolbar** and in the bottom choose **compact** for **Density**<br />                                                                                                                                                                                                                                     ***Toolbar***<br />                                                                                                                                                                                                                                                                                                                                 Right click on the top left firefox icon and select **Remove from Toolbar**<br />                                                                                                                                                                                                                                                                   Click on the **tile** icon and pin the nessesary extensions<br />                                                                                                                                                                                                                                                                                   Right click on toolbar and select **Customize Toolbar**<br />                                                                                                                                                                                                                                                                                       On the bottom left corner tick **Title bar**<br />                                                                                                                                                                                                                                                                                                  Remove and items until the task bask bar looks like this :<br />                                                                                                                                                                                                                                                                                    ![image](https://user-images.githubusercontent.com/56675566/208410781-cee56941-3b5a-4180-8062-8431ba8d2cf0.png)<br />                                                                                                                                                                                                                               Click the **Downloads** icon and uncheck the **Hide when empty**<br />                                                                                                                                                                                                                                                                              ***Alt Autism***<br />                                                                                                                                                                                                                                                                                                                              Inside Firefox search for **about:config -> ui.key.menuAccessKeyFocuses** and change to false<br />                                                                                                                                                                                                                                                 ***Theme***<br />                                                                                                                                                                                                                                                                                                                                   <a href="https://addons.mozilla.org/en-US/firefox/addon/nicothin-dark-magic/?utm_source=addons.mozilla.org&utm_medium=referral&utm_content=search">Prefered theme</a><br />                                                                                                                                                                         ***Touchpad Gestures***<br />                                                                                                                                                                                                                                                                                                                       \# sudo vim /etc/enviroment<br />                                                                                                                                                                                                                                                                                                                   Add "MOZ_USE_XINPUT2=1"<br />                                                                                                                                                                                                                                                                                                                       ***Enhanced Youtube***<br />                                                                                                                                                                                                                                                                                                                        Open a video in youtube, press the gear button unber the video player and copy paste the [settings](https://github.com/AlexanderStavrop/ArchLinux_Installation-and-Essentilas/files/9945970/Enhanced-Yt.txt) in the import section.                                                                                                                                                                                                                                         | 
| MPV                | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru mpv                                                                                                             |
| Notepadqq          | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru notepadqq<br />                                                                                                                                                                                                                                                                                                                             ***Overall Look***<br />                                                                                                                                                                                                                                                                                                                            Navigate to **settings -> Preferences -> Appearance** and change the **Color scheme** to **base-16-dark**<br />                                                                                                                                                                                                                                     ***Disable Indentation Messages***<br />                                                                                                                                                                                                                                                                                                            Navigate to **settings -> Preferences -> General** and untick **Warn when the indentation doesn't match the settings**  |
| Obs                | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru obs-studio<br />                                                                                                                                                                                                                                                                                                                            ***Basic Configuration***<br />                                                                                                                                                                                                                                                                                                                     Open the application and choose **Optimize just for recording**<br />                                                                                                                                                                                                                                                                               Select **30** in the **Fps** section<br />                                                                                                                                                                                                                                                                                                          Navigate to **File -> Settings -> Output** and change the **Recording Path**<br />                                                                                                                                                                                                                                                                  Navigate to **File -> Settings -> Video** and change the **Base (canvas) Resolution** to max resolution<br />           |
| Signal             | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru signal-desktop<br />                                                                                                                                                                                                                                                                                                                        ***Appearance***<br />                                                                                                                                                                                                                                                                                                                              Open the app and navigate to **File -> Preferences -> Appearance** and select **Dark** in **Theme**                    |               
| Spotify            | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru Spotify                                                                                                         |
| Stremio            | ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru stremio<br />                                                                                                                                                                                                                                                                                                                               ***Close button actually closes the app***<br />                                                                                                                                                                                                                                                                                                    Navigate to **Settings** and in the **General** tab enable **Close app when window is closed**<br />                                                                                                                                                                                                                                                ***Torrentio***<br />                                                                                                                                                                                                                                                                                                                               <a href="https://torrentio.strem.fun/configure">Download</a>                                                            |
| Special characters | ***Download Emoji***<br />                                                                                                                                                                                                                                                                                                                          \# paru noto-fonts-emoji<br />                                                                                                                                                                                                                                                                                                                      ***Download Chinise characters***<br />                                                                                                                                                                                                                                                                                                             \# paru noto-fonts-cjk                                                                                                  |
| Steam              | ***Enable Multilib (if not enabled)***<br />                                                                                                                                                                                                                                                                                                        \# sudo vim /etc/pacman.conf<br />                                                                                                                                                                                                                                                                                                                  Uncomment multilib and Include<br />                                                                                                                                                                                                                                                                                                                ***Download***<br />                                                                                                                                                                                                                                                                                                                                \# paru steam<br />                                                                                                                                                                                                                                                                                                                                 \# paru arial<br />                                                                                                                                                                                                                                                                                                                                 ***Enabling Proton***<br />                                                                                                                                                                                                                                                                                                                         steam -> settings -> steam play -> Enable steam play, select latest proton<br />                                                                                                                                                                                                                                                                    If games don't start -> \# paru -Rsn amdvlk && \# paru -Rsn lib32-amdvlk                                                |       



## Task bar configuration

Right click the **task bar** and select "Enter edit mode"    
- ***System tray icons***
    - Right click the task bar and select "Add widgets"
        - *Add*
            - "Individual core usage"
            - "Memory usage"
            - "Network speed"
            - "Keyboard layout"
        - *Win 7 show desktop*
            - On the top right corner, press "Get new widgets"
            - Search for "Win 7 show desktop"
            - Add it to the task bar on the far right corner
    - Right click the task bar and select "Enter edit mode". then hover over system tray icons and select "Configure"
        - In the **Entries** tab
            - *Always Shown*
                - "CopyQ"
                - "Flameshot"
                - "Audio Volume"
                - "Battery and Brightness"
                - "Bluetooth"
                - "Disk and Devices"
                - "Networks"
                - "Notifications"
                - "Weather"
                    - In the **Weather Station** section
                        - Add your current city
                    - In the **Appearance** section
                        - *Show in tooltip* &rarr; Enable everything
                    - In **Units** section
                        - Change everything from freedom units to normal ones
            - *Shown when relevant*
                - "Display configuration"
                - "KDE Connect"
            - *Disable*
                - Everything else
            - If the clock takes too much space move it a little to the left 
- ***Application shortcuts***
    - Hover over the applications icons and select "Configure"
        - In **Appearance** section
            - *General* &rarr; Uncheck "Mark applications that play audio"
        - In **Behavior** section
            - *Mouse wheel* &rarr; uncheck "Cycles through tasks"
- ***Task bar size***
    - *Panel width* &rarr; "34"
- **Overall look***
    - Re arrange the icons unlil the task bar looks like this:
      ![image](https://user-images.githubusercontent.com/56675566/224395097-f0d9b573-666b-4151-a27b-8e2f14173b71.png)         
