## 1. Before nVidia drivers installation
1.1 Check is your nVidia card supported
```
lspci |grep -E "VGA|3D"

## Example outputs ##
01:00.0 VGA compatible controller: NVIDIA Corporation GF119 [GeForce GT 610] (rev a1)
```

## 2. Install nVidia proprietary drivers on Fedora 27/26/25/24/23/22/21 and disable the nouveau driver

2.1 Download nVidia Installer Package
Go to http://www.nvidia.com/Download/Find.aspx?lang=en-us and find latest version of installer package. 
When you use browser this is normally downloaded /home/<username>/Downloads/NVIDIA-Linux-xxxx.run location.

**Tested versions:**
<table class="mdl-data-table mdl-js-data-table mdl-shadow--2dp">
<thead>
<tr>
<th class="mdl-data-table__cell--non-numeric">Fedora 27</th>
<th class="mdl-data-table__cell--non-numeric">Fedora 26</th>
<th class="mdl-data-table__cell--non-numeric">Fedora 25</th>
<th class="mdl-data-table__cell--non-numeric">Fedora 24/23/22/21</th>
</tr>
</thead>
<tbody>
<tr>
<td data-label="Fedora 27" class="mdl-data-table__cell--non-numeric mdl-color-text--green">390.25 (January 29, 2018)</td>
<td data-label="Fedora 26" class="mdl-data-table__cell--non-numeric mdl-color-text--green">390.25 (January 29, 2018)</td>
<td data-label="Fedora 25" class="mdl-data-table__cell--non-numeric mdl-color-text--green">390.25 (January 29, 2018)</td>
<td data-label="Fedora 24/23/22/21" class="mdl-data-table__cell--non-numeric mdl-color-text--green">390.25 (January 29, 2018)</td>
</tr>
<tr>
<td data-label="Fedora 27" class="mdl-data-table__cell--non-numeric mdl-color-text--green">387.34 (November 24, 2017)</td>
<td data-label="Fedora 26" class="mdl-data-table__cell--non-numeric mdl-color-text--green">387.34 (November 24, 2017)</td>
<td data-label="Fedora 25" class="mdl-data-table__cell--non-numeric mdl-color-text--green">387.34 (November 24, 2017)</td>
<td data-label="Fedora 24/23/22/21" class="mdl-data-table__cell--non-numeric mdl-color-text--green">387.34 (November 24, 2017)</td>
</tr>
<tr>
<td data-label="Fedora 27" class="mdl-data-table__cell--non-numeric mdl-color-text--green">384.111 (January 4, 2018)</td>
<td data-label="Fedora 26" class="mdl-data-table__cell--non-numeric mdl-color-text--green">384.111 (January 4, 2018)</td>
<td data-label="Fedora 25" class="mdl-data-table__cell--non-numeric mdl-color-text--green">384.111 (January 4, 2018)</td>
<td data-label="Fedora 24/23/22/21" class="mdl-data-table__cell--non-numeric mdl-color-text--green">384.111 (January 4, 2018)</td>
</tr>
<tr>
<td data-label="Fedora 27" class="mdl-data-table__cell--non-numeric mdl-color-text--green">340.106 (January 16, 2018)</td>
<td data-label="Fedora 26" class="mdl-data-table__cell--non-numeric mdl-color-text--green">340.106 (January 16, 2018)</td>
<td data-label="Fedora 25" class="mdl-data-table__cell--non-numeric mdl-color-text--green">340.106 (January 16, 2018)</td>
<td data-label="Fedora 24/23/22/21" class="mdl-data-table__cell--non-numeric mdl-color-text--green">340.106 (January 16, 2018)</td>
</tr>
<tr>
<td data-label="Fedora 27" class="mdl-data-table__cell--non-numeric mdl-color-text--green">304.137 (September 19, 2017)</td>
<td data-label="Fedora 26" class="mdl-data-table__cell--non-numeric mdl-color-text--green">304.137 (September 19, 2017)</td>
<td data-label="Fedora 25" class="mdl-data-table__cell--non-numeric mdl-color-text--green">304.137 (September 19, 2017)</td>
<td data-label="Fedora 24/23/22/21" class="mdl-data-table__cell--non-numeric mdl-color-text--green">304.137 (September 19, 2017)</td>
</tr>
</tbody>
</table>

### 2.2 Make nVidia installer executable
```
chmod +x /path/to/NVIDIA-Linux-*.run
```
### 2.3 Change root user
```
su -
## OR ##
sudo -i
```
### 2.4 Make sure that you system is up-to-date and you are running latest kernel
```
## Fedora 27/26/25/24/23/22 ##
dnf update

## Fedora 21 ##
yum update
```
After update reboot your system and boot using latest kernel:
```
reboot
```
### 2.5 Install needed dependencies
```
## Fedora 27/26/25/24/23/22 ##
dnf install kernel-devel kernel-headers gcc dkms acpid libglvnd-glx libglvnd-opengl libglvnd-devel pkgconfig

## Fedora 21 ##
yum install kernel-devel kernel-headers gcc dkms acpid
```
### 2.6 Disable nouveau
#### 2.6.1 Create or edit /etc/modprobe.d/blacklist.conf
**Append ‘blacklist nouveau’**
```
echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf
```
#### 2.6.2 Edit /etc/sysconfig/grub
**Append ‘rd.driver.blacklist=nouveau’ to end of ‘GRUB_CMDLINE_LINUX=”…”‘**
```
## Example row ##
GRUB_CMDLINE_LINUX="rd.lvm.lv=fedora/swap rd.lvm.lv=fedora/root rhgb quiet rd.driver.blacklist=nouveau"
```
#### 2.6.3 Update grub2 conf
```
## BIOS ##
grub2-mkconfig -o /boot/grub2/grub.cfg

## UEFI ##
grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg
```
#### 2.6.4 Remove xorg-x11-drv-nouveua
```
## Fedora 27/26/25/24/23/22 ##
dnf remove xorg-x11-drv-nouveau

## Fedora 21 ##
yum remove xorg-x11-drv-nouveau
```
If you have following row on /etc/dnf/dnf.conf file, then you can remove it:
```
exclude=xorg-x11*
```
#### 2.6.5 Generate initramfs
```
## Backup old initramfs nouveau image ##
mv /boot/initramfs-$(uname -r).img /boot/initramfs-$(uname -r)-nouveau.img
 
## Create new initramfs image ##
dracut /boot/initramfs-$(uname -r).img $(uname -r)
```
### 2.7 Reboot to runlevel 3
*Note: You don’t have Desktop/GUI on runlevel 3. Make sure that you have some access to end of guide. (Print it, use lynx/links/w3m, save it to text file).*
```
systemctl set-default multi-user.target

reboot
```
## 2.8 Install nVidia proprietary drivers for GeForce 6/7 & GeForce 8/9/200/300 & GeForce 400/500/600/700/800/900/10 series cards

### 2.8.1 Log in as root user
```
su -
## OR ##
sudo -i
```
### 2.8.2 Run NVIDIA Binary
**Following command executes driver install routine. Use full file name command if you have multiple binaries on same directory.**

```

./NVIDIA-Linux-*.run

## OR full path / full file name ##

./NVIDIA-Linux-x86_64-390.12.run

/path/to/NVIDIA-Linux-x86_64-387.34.run

/path/to/NVIDIA-Linux-x86_64-384.111.run

/path/to/NVIDIA-Linux-x86_64-340.106-patched.run

/home/<username>/Downdloads/NVIDIA-Linux-x86_64-304.137-patched.run
```
### 2.8.3 nVidia Installer Accept License
![01-accept-nvidia-license](https://raw.githubusercontent.com/CGQAQ/BuggyLife/master/fedora%20install%20nvidia%20drivers/01-accept-nvidia-license-748x561.png)
### 2.8.4 nVidia Installer Register the Kernel Source Modules with DKMS
![02-nvidia-installer-dkms](https://raw.githubusercontent.com/CGQAQ/BuggyLife/master/fedora%20install%20nvidia%20drivers/02-nvidia-installer-dkms-748x561.png)
### 2.8.5 nVidia Installer 32-bit Compatibility Libraries
![03-nvidia-installer-32-bit-compatibility](https://raw.githubusercontent.com/CGQAQ/BuggyLife/master/fedora%20install%20nvidia%20drivers/03-nvidia-installer-32-bit-compatibility-748x561.png)
### 2.8.6 nVidia Installer Installing Drivers
**Note: If you get libglvnd error, then abort installation and try this. Also “Install and overwrite existing files” works, but fixing this error is more clean way to install NVIDIA Drivers.**
![04-nvidia-installer-installing](https://raw.githubusercontent.com/CGQAQ/BuggyLife/master/fedora%20install%20nvidia%20drivers/04-nvidia-installer-installing-748x561.png)
### 2.8.7 nVidia Installer Xorg Backup
![05-nvidia-installer-xorg-backup](https://raw.githubusercontent.com/CGQAQ/BuggyLife/master/fedora%20install%20nvidia%20drivers/05-nvidia-installer-xorg-backup-748x561.png)
### 2.8.8 nVidia Drivers Installation Complete
![06-nvidia-installation-complete](https://raw.githubusercontent.com/CGQAQ/BuggyLife/master/fedora%20install%20nvidia%20drivers/06-nvidia-installation-complete-748x561.png)
## 2.9 All Is Done and Then Reboot Back to Runlevel 5
```
systemctl set-default graphical.target

reboot
```
## 2.10 VDPAU/VAAPI support
**To enable video acceleration support for your player (Note: you need Geforce 8 or later).**
```
## Fedora 27/26/25/24/23/22 ##
dnf install vdpauinfo libva-vdpau-driver libva-utils

## Fedora 21 ##
yum install vdpauinfo libva-vdpau-driver libva-utils
```










