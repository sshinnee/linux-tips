# linux-tips
Tips on setting up a linux workstation /server.

## Fedora Workstation

### 1. Install VirtualBox Guest Additions for Virtual Machines
- Insert Guest Additions DVD to virtual machine.
- Install the following dependencies:
```
sudo dnf install kernel-devel-$(uname -r)
sudo dnf install gcc make perl -y
```
- Install Guest Additions:
```
cd /run/media/$USER/VBox_GAs_5.2.8/
sudo ./VBoxLinuxAdditions.run
```

### 2. RPMFusion Repository
Run:
```
sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm -y
```

### Support for Fractional DPI
Run:
```
gsettings set org.gnome.mutter experimental-features "['scale-monitor-framebuffer']"
```

### 3. Block Kernel Updates (Optional)
- Block kernel updates from installing:
```
sudo echo "exclude=kernel*" >> /etc/dnf/dnf.conf 
```

- List all available boot entries:
```
sudo grep -P "submenu|^menuentry" /boot/efi/EFI/fedora/grub.cfg | cut -d "'" -f2
```

- Boot the desired kernel at startup:
```
sudo grub2-set-default "Fedora (4.13.9-300.fc27.x86_64) 27 (Twenty Seven)"
```

### 4. Fix blank Wine applications (PlayOnLinux)
- Install PlayOnLinux:
```
sudo dnf install playonlinux -y
```
- Fix blank screen while running Windows applications. Problem as reported [here](https://askubuntu.com/questions/976300/installing-microsoft-office-2010-in-ubuntu-17-10-with-playonlinux-does-not-proce):

- Remove outdated libraries that ship with wine on playonlinux
```
rm ~/.PlayOnLinux/wine/linux*/*/lib*/libz*
```

### 5. WhatsApp Desktop Client
- Add [3rd party](https://github.com/Enrico204/Whatsapp-Desktop) repo:
```
sudo bash -c 'cat << EOF > /etc/yum.repos.d/Enrico204_Whatsapp-Desktop.repo
[Enrico204_Whatsapp-Desktop]
name=Enrico204_Whatsapp-Desktop
baseurl=https://packagecloud.io/Enrico204/Whatsapp-Desktop/el/6/$basearch
repo_gpgcheck=1
gpgcheck=0
enabled=1
gpgkey=https://packagecloud.io/Enrico204/Whatsapp-Desktop/gpgkey
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300
EOF'
```
- Enable and install app:
```
sudo dnf config-manager --set-enabled Enrico204_Whatsapp-Desktop -y
sudo dnf install  WhatsApp -y
```

### 6. Remmina Remote Desktop Client
- Ensure RPMFusion repo is installed.
- Install Remmina:
```
sudo dnf install remmina* -y
```

### 7. VirtualBox
Run:
```
sudo dnf install VirtualBox kernel-devel-$(uname -r) akmod-VirtualBox
sudo akmods
```
### 8. ExFAT Support
Run:
```
sudo dnf install fuse-exfat -y
```

### 9. Visual Studio Code
Run:
```
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
sudo dnf check-update
sudo dnf install code -y
```

### 10. Telegram Desktop
Run:
```
sudo dnf install telegram-desktop -y
```

### 11. Google Chrome Browser
Run:
```
sudo bash -c 'cat << EOF > /etc/yum.repos.d/google-chrome.repo
[google-chrome]
name=google-chrome - \$basearch
baseurl=http://dl.google.com/linux/chrome/rpm/stable/\$basearch
enabled=1
gpgcheck=1
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub
EOF'

sudo dnf install google-chrome-stable -y
```
### 12. Enable Intel GUC Firmware
For Skylake, Kabylake and later chipsets:
- Add the following line:
```
echo "options i915 enable_guc_loading=1 enable_guc_submission=1" | sudo tee /etc/modprobe.d/i915.conf
```

- Make changes boot:
```
sudo dracut --force
```

- Reboot:
```
sudo reboot now
```
- Check settings take effect:
```
sudo cat /sys/kernel/debug/dri/0/i915_guc_load_status
```

### 13. Fedora Mediawriter
For writing isos to USB drive, install Mediawriter tool:
```
sudo dnf install mediawriter -y
```
