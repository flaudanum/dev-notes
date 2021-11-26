<h1>Setting up a new Fedora distribution</h1>

Installation of apps and and miscellaneous setups for a [Fedora Workstation](https://getfedora.org/en/workstation/) distribution.

- [Integrated Development Environments](#integrated-development-environments)
  - [Visual Studio Code](#visual-studio-code)
    - [Installation](#installation)
    - [Useful plugins](#useful-plugins)
- [Git](#git)
- [Drivers for Nvidia GrForce GTX 1060 graphic card](#drivers-for-nvidia-grforce-gtx-1060-graphic-card)
- [Miscellaneous](#miscellaneous)

## Integrated Development Environments

### Visual Studio Code

#### Installation

Official installation process is described [here](https://code.visualstudio.com/docs/setup/linux#_rhel-fedora-and-centos-based-distributions).

```
$ sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
$ sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
$ dnf check-update
Fedora 35 - x86_64 - Updates                                                                                                                                                                                   71 kB/s |  16 kB     00:00    
Fedora 35 - x86_64 - Updates                                                                                                                                                                                  2.7 MB/s | 1.6 MB     00:00    
Fedora Modular 35 - x86_64 - Updates                                                                                                                                                                          275 kB/s |  21 kB     00:00    
Visual Studio Code                                                                                                                                                                                             19 MB/s |  20 MB     00:01    
$ sudo dnf install code
Visual Studio Code                                                                                                                                                                                             20 MB/s |  20 MB     00:00    
Last metadata expiration check: 0:00:04 ago on Sat 13 Nov 2021 03:06:28 PM CET.
Dependencies resolved.
==============================================================================================================================================================================================================================================
 Package                                              Architecture                                           Version                                                               Repository                                            Size
==============================================================================================================================================================================================================================================
Installing:
 code                                                 x86_64                                                 1.62.2-1636665099.el7                                                 code                                                 103 M

Transaction Summary
==============================================================================================================================================================================================================================================
Install  1 Package

Total download size: 103 M
Installed size: 293 M
Is this ok [y/N]: y
Downloading Packages:
code-1.62.2-1636665099.el7.x86_64.rpm                                                                                                                                                                          29 MB/s | 103 MB     00:03    
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                                                                                          29 MB/s | 103 MB     00:03     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                                                                                      1/1 
  Installing       : code-1.62.2-1636665099.el7.x86_64                                                                                                                                                                                    1/1 
  Running scriptlet: code-1.62.2-1636665099.el7.x86_64                                                                                                                                                                                    1/1 
  Verifying        : code-1.62.2-1636665099.el7.x86_64                                                                                                                                                                                    1/1 

Installed:
  code-1.62.2-1636665099.el7.x86_64                                                                                                                                                                                                           

Complete!
```

#### Useful plugins

|                           |     |
| ------------------------- | --- |
| Angular Language Service  |     |
| Git Graph                 |     |
| HTML CSS Support          |     |
| JetBrains IDE Keymap      |     |
| Markdown All in One       |     |
| markdownlint              |     |
| Markdown PDF              |     |
| Prettier - Code formatter |     |
| Python                    |     |
| Python Extension Pack     |     |

## Git

Adding a prompt by adding to `.bash_profile` :

```bash
export PS1='\[\033[0;32m\]\[\033[0m\033[0;32m\]\u\[\033[0;36m\] @ \w\[\033[0;32m\]$(if git rev-parse --git-dir > /dev/null 2>&1; then echo " - ["; fi)$(git branch 2>/dev/null | grep "^*" | colrm 1 2)\[\033[0;32m\]$(if git rev-parse --git-dir > /dev/null 2>&1; then echo "]"; fi)\[\033[0m\033[0;32m\] \$\[\033[0m\033[0;32m\]\[\033[0m\] '
```

## Drivers for Nvidia GrForce GTX 1060 graphic card

Download the proper driver [here](https://www.nvidia.com/Download/Find.aspx?lang=en-us).

Next follow installation steps in [this guide](https://www.if-not-true-then-false.com/2015/fedora-nvidia-guide/).

```
$ lspci |grep -E "VGA|3D"
01:00.0 VGA compatible controller: NVIDIA Corporation GP106 [GeForce GTX 1060 6GB] (rev a1)
```

Install required dependencies:

```
sudo su -
dnf install kernel-devel kernel-headers gcc make dkms acpid libglvnd-glx libglvnd-opengl libglvnd-devel pkgconfig
```

Disabling **nouveau**

```
# ll /etc/modprobe.d/blacklist.conf
ls: cannot access '/etc/modprobe.d/blacklist.conf': No such file or directory
# echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf
# cat /etc/modprobe.d/blacklist.conf
blacklist nouveau
```

Edit `/etc/default/grub` and append `rd.driver.blacklist=nouveau` to end of `GRUB_CMDLINE_LINUX=”…”`.

```ini
GRUB_TIMEOUT=5
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="resume=UUID=6e29face-23c6-40d7-8192-c96417d41b6a rhgb quiet rd.driver.blacklist=nouveau"
GRUB_DISABLE_RECOVERY="true"
GRUB_ENABLE_BLSCFG=true
```

Update grub2 conf (BIOS and UEFI).

```
# grub2-mkconfig -o /boot/grub2/grub.cfg
Generating grub configuration file ...
Found Windows Boot Manager on /dev/sdc1@/efi/Microsoft/Boot/bootmgfw.efi
Adding boot menu entry for UEFI Firmware Settings ...
done
```

Remove xorg-x11-drv-nouveau.

```
dnf remove xorg-x11-drv-nouveau
```

Backup old initramfs nouveau image:

```
mv /boot/initramfs-$(uname -r).img /boot/initramfs-$(uname -r)-nouveau.img
```

Create new initramfs image (~45s):

```
dracut /boot/initramfs-$(uname -r).img $(uname -r)
```

Reboot to runlevel 3 :

```
# systemctl set-default multi-user.target
Removed /etc/systemd/system/default.target.
Created symlink /etc/systemd/system/default.target → /usr/lib/systemd/system/multi-user.target.
# reboot
```

Next run the driver installer.

Then Reboot Back to Runlevel 5:

```
systemctl set-default graphical.target
reboot
```

## Miscellaneous

```
dnf install openssl.x86_64
```