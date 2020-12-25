# Migrating from Ubuntu on WSL to ArchLinux on WSL2

## Upgrading to WSL 2

-   Download [WSL2 Kernel](https://docs.microsoft.com/en-us/windows/wsl/wsl2-kernel)
-   run `wsl --set-default-version 2` in windows command line, so that all future WSL machine will use WSL2.

## Installing Arch Linux

### Basic installation

-   Download [ArchWSL](https://github.com/yuk7/ArchWSL) installer zip, pick a folder (for example, `c:\archlinux`) for Arch Linux and run installer .exe in that folder.

### Initialize package manager

-   Add custom pacman repository with additional packages: `nano /etc/pacman.conf` Then add following to the bottom:

```
[archlinuxfr]
SigLevel = Never
Server = http://repo.archlinux.fr/$arch
```

-   Refresh Pacman GPG keys:

```
pacman-key --init
pacman-key --populate
pacman-key --refresh-keys
pacman -Sy archlinux-keyring
```

-   Run `pacman -Syyu` to update all packages to the latest versions

### Install zsh, optional

-   Install zsh: `pacman -S zsh`

### Create a user

-   Add a sudo group: `groupadd sudo`
-   Enable sudoers: `nano /etc/sudoers` and uncomment lines `%wheel ALL=(ALL) NOPASSWD: ALL` and `%sudo ALL=(ALL) ALL`
-   Add new admin user: `useradd -m -G wheel,sudo -s /bin/zsh <username>`, use `-s /bin/bash` if you want bash instead of zsh.
-   Set password for the new user: `passwd <username>`
-   Run Windows command shell, go to the directory with Arch Linux, run `Arch.exe config --default-user <username>`. Now you have basic ArchLinux with user.

### Install AUR helper

-   Just follow [How to Install Yay AUR Helper in Arch Linux and Manjaro](https://www.tecmint.com/install-yay-aur-helper-in-arch-linux-and-manjaro/) guide:
-   `sudo pacman -S git openssh`
-   `sudo pacman -S base-devel` - when asked question on fakeroot and fakeroot-tcp choose to leave fakeroot-tcp and not install fakeroot
-   `git clone https://aur.archlinux.org/yay-git.git`
-   `cd yay-git`
-   `makepkg -si`
-   Run `yay -Syu` to update all AUR packages and reinstall fakeroot-tcp (will install the latest version)
-   Remove the leftovers: `rm -rf ~/yay-git`

### Goodies

-   Install useful console tools: `sudo pacman -S mc wget htop pv ccze`

### Preserving config files from previous WSL installation

Copy .ssh, .kube, .aws from original installation home directory to the Arch WSL2 one.

Fix file permissions for SSH keys, run from the home directory:

```
chmod 0644 .ssh/id_rsa.pub
chmod 0600 .ssh/id_rsa
```

## Installing additional software

### Install Kubernetes

`sudo pacman -S kubectl kubectx`

### Install Docker

Uninstall Windows docker to prevent potential naming conflicts.

`sudo pacman -S docker docker-compose`

Use [https://github.com/imjonos/wsl2-docker-start](wsl2-docker-start) scripts to run docker exposed to Windows:

```
git clone git@github.com:imjonos/wsl2-docker-start.git
cd wsl2-docker-start
chmod +x docker-start prepare-hosts
sudo cp docker-start prepare-hosts /usr/local/bin
```

In Windows you should make sure `C:\Windows\System32\drivers\etc\hosts` file has write access for your user.
Always run docker in a dedicated terminal tab using `docker-start`

### ld100 dotfiles & zsh config

```
git clone https://github.com/ld100/dotfiles.git ~/.dotfiles
zsh ~/.dotfiles/install.sh
```

### Install programming languages

`sudo pacman -S ruby nodejs python go crystal php jre-openjdk-headless`

### Installing protocol buffers

Main binaries/libraries: `sudo pacman -S protobuf protobuf grpc grpc-cli`

gRPC for Python and PHP: `sudo pacman -S python-grpcio php-grpc`

gRPC & Protobuf for Go: `yay -S protobuf-go protoc-gen-go-grpc`

gRPC & Protobuf for Ruby: `gem install google-protobuf grpc grpc-tools`

## Setting ArchLinux as default WSL

Just run `wsl --set-default Arch` in windows command shell.

## Accessing WSL from Windows

All WSL machines are available in Windows Explorer on `\\wsl$\` URL, Arch linux is: `\\wsl$\Arch`.
For additional convenience you could map `\\wsl$\Arch` to a 1-Leter Windows disk share, for example having `Z:\` pointing to `\\wsl$\Arch`
