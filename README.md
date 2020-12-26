# Windows 10 Post-Installation setup and configuration Guide 2021

<br>

A selection of notes, bits and pieces that became an overview of my Windows 10 development setup, finally i gathered them all here in one place, for my own reference but it might be also useful to others.

<br>

enjoy 🤿

## Table of Contents

-   [Getting Started](#getting-started)

    -   [Automatic Updates & Telemetry](#Automatic-Updates-&-Telemetry)
    -   [Chocolatey Package Manager](#chocolatey-package-manager)
        -   [Chocolatey Installation](#chocolatey-installation)
        -   [Choco Packages](#choco-packages)
        -   [Choco Commands](#choco-commands)
    -   [PowerShell](#powershell)
        -   [Basic Commands](#basic-commands)
        -   [$PROFILE](#profile)
        -   [Autocompletion](#autocompletion)
            -   [Chocolatey Autocompletion](#chocolatey-autocompletion)
        -   [Oh-My-Posh](#oh-my-posh)
            -   [Installation](#ohmyposh-installation)
            -   [Terminal Icons](#terminal-icons)
            -   [Fonts & Glyphs](#fonts-&-glyphs)
        -   [$PROFILE Example](#profile-example)
    -   [Windows Terminal](#windows-terminal)
        -   [Customization](#customization)
    -   [Windows Subsystem for Linux](#windows-subsystem-for-linux)
        -   [WSL Installation](#wsl-installation)
            -   [Accessibility](#accessibility)
            -   [Global Configuration](#global-configuration)
            -   [Import / Export](#import-/-export)
            -   [Basic Commands](#wsl-basic-commands)
        -   [WSL Ubuntu](#wsl-ubuntu)
            -   [First Steps](#first-steps)
            -   [Apt Packages](#apt-packages)
            -   [git](#git)
                -   [GitHub CLI](#github-cli)
                -   [SSH with git](#ssh-with-git)
                -   [First Commit](#first-commit)
                -   [.gitignore](#.gitignore)
            -   [SSH](#ssh)
            -   [Shell](#shell)
                -   [Aliases](#Aliases)
                -   [Oh-My-Zsh](#oh-my-zsh)
                -   [Plugins](#plugins)
                    -   [Antigen Plugin Manager](#antigen-plugin-manager)
                -   [Powerlevel10k Theme](#powerlevel10k-theme)
            -   [Development](#development)
                -   [Javascript](#javascript)
                    -   [node.js](#nodejs)
                    -   [nvm](#nvm)
                    -   [npm](#npm)
                    -   [yarn](#yarn)
                    -   [deno](#deno)
                -   [Python](#python)
                -   [Docker](#docker)
                    -   [Installation](#docker-installation)
                    -   [Docker Compose](#docker-compose)
                    -   [Basic Commands](#docker-commands)
                -   [Rust](#rust) - [Nginx](#nginx)
                -   [\*Apache2](#apache2)
                -   [\*MongoDB](#mongodb)
                -   [\*MySQL](#mysql)
                    -   [Password Reset](#password-reset)
                -   [PHP](#php)
                    -   [Composer](#composer)
                    -   [\*phpMyAdmin](#phpmyadmin)
                -   [\*Prestashop](#prestashop)
                -   [Wordpress](#wordpress)
        -   [WSL Archlinux](#wsl-archlinux)
            -   [Archlinux Installation](#archlinux-installation)
            -   [Initialize package manager](#initialize-package-manager)
            -   [Refresh Pacman GPG keys](#refresh-pacman-gpg-keys)
            -   [Add User](#add-user)
            -   [Install YAY](#install-yay)
            -   [Accessing WSL from Windows](#accessing-wsl-from-windows)
        -   [WSL CentOS 8](#wsl-centos-8)
            -   [Installation](#centos-8-installation)
            -   [\*dnf](#dnf)
    -   [WSL Tips](#wsl-tips)
        -   [Swappiness](#swappiness)
        -   [Bell](#bell)
        -   [Time](#time)
    -   [Interesting tweaks and locations](#interesting-tweaks-and-locations)
        -   [Local Shared Folders](#local-shared-folders)
        -   [Hibernation](#hibernation)
        -   [HYPER-V](#hyper-v)
        -   [Scoop Package Manager](#scoop-package-manager)
        -   [Winget Package Manager](#winget-package-manager)
        -   [HYPER-V](#hyper-v)
        -   [God Mode](#god-mode)
        -   [Disable content indexing on SSDs](#disable-content-indexing-on-ssds)
        -   [Remap default folder locations to a separate HDD](#remap-default-folder-locations-to-a-separate-HDD)
        -   [Image Backup & Recovery](#image-backup-&-recovery)

-   [References](#references)

<br>

# Getting Started

The guide's state is **work in progress**, you will might find inside old deprecated stuff and **lots of mistakes**, ill try to have it clean and up to date, any recommendations welcomed.

<br>

# Automatic Updates & Telemetry

**1.** Disable Windows 10 automatic updates by let it getting you notified before downloading or installing any updates.

run `gpedit.msc`

    Computer Configuration > Administrative Templates > Windows Components > Windows Update

Click on `Configure Automatic Updates` and put in on **enable** with a value of **2**

<br>

**2.** Disable Windows Telemetry, again if you aren't still there run `gpedit.msc`

    Computer Configuration > Administrative Templates > Windows Components > Data Collection and Preview Builds

Click on `Allow Telemetry` and change it to **disable**

<br>

From here we can also **disable** Cortana and Cloud Searching

    Computer Configuration > Administrative Templates > Windows Components > Search

<br>

# Chocolatey Package Manager

[Chocolatey](https://chocolatey.org/) is a package manager for windows it can make our life a **lot easier** by automating updates and package installations.

<br>

### Chocolatey Installation

You can refer first to the official [documentation](https://chocolatey.org/install) for installation

**1.** Open `PowerShell` as **administrator** and run `Get-ExecutionPolicy.`
If it returns `Restricted` , then run `Set-ExecutionPolicy Bypass -Scope Process`

<br>

**2.** Run this command to start the installation:

     Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

now run `choco` to confirm that the installation was **successful** it must return Chocolatey's **version**.

<br>

## Choco Packages

You can find packages by running `search` command, from Chocolatey's [page](https://chocolatey.org/packages) via the searchbox, or from the [GUI](https://chocolatey.org/packages/ChocolateyGUI) which is kinda show and glitchy, best and fastest way is from command line.

<br>

**Essential Packages**:

    choco install -y 7zip microsoft-windows-terminal powertoys docker-desktop virtualbox wsl-ubuntu-2004 coretemp sharex quicklook qbittorrent adobereader etcher thunderbird libreoffice-fresh laragon linux-reader tomcat javaruntime processhacker

<br>

**Text Editors**:

    choco install -y vscode vscode-insiders sublimetext3 atom kate

<br>

**Browsers**:

    choco install -y firefox tor-browser firefox-dev googlechrome chromium googlechrome.dev brave vivaldi

<br>

**Communication**:

    choco install -y viber hexchat zoom discord skype teamviewer

<br>

**Utilities**:

    choco install -y winrar rufus curl wget git grep intel-xtu bleachbit vcxsrv autohotkey gitkraken putty filezilla directx winscreenfetch gimp github-desktop kdenlive obs-studio

<br>

**Chocolatey's packages**:

    choco install -y chocolateygui choco-cleaner chocolatey-core.extension

<br>

**Media**:

    choco install -y vlc soulseek mp3tag stremio kodi spotify youtube-dl retroarch

<br>

Also these are not included in Chocolatey's repositories but are **worth mentioning**.

[Tutanota Email Client](https://tutanota.com/blog/posts/desktop-clients/)

[Protonmail Unofficial Email Client](https://github.com/vladimiry/ElectronMail)

<br>

### Choco Commands

See also the [Chocolatey Command Reference](https://chocolatey.org/docs/commands-reference) for a complete list.

| Command                                                      | Description                                     |
| ------------------------------------------------------------ | ----------------------------------------------- |
| **Search**                                                   |                                                 |
| `choco list`                                                 | List all chocolatey packages                    |
| `choco search` `<package>`                                   | Search packages mentioning `<package>`          |
| `choco search --approved-only` `<package>`                   | Only return approved packages                   |
| `choco info` `<package>`                                     | Get information about `<package>`               |
|                                                              |                                                 |
| **Install**                                                  |                                                 |
| `choco install <package>`                                    | Install `<package>`                             |
| `choco install <package> --install-directory=P:\<directory>` | Install to a specific directory                 |
|                                                              |                                                 |
| **Maintenance**                                              |                                                 |
| `choco list --localonly`                                     | List installed packages                         |
| `choco outdated`                                             | List upgradable packages                        |
| `choco upgrade all -y`                                       | Upgrade all packages                            |
| `cup all -y`                                                 | Upgrade all packages                            |
|                                                              |                                                 |
| **Pin**                                                      |                                                 |
| `choco pin list`                                             | List pinned packages                            |
| `choco pin add --name <package>`                             | Suppress upgrades for `<package>`               |
| `choco pin remove --name <package>`                          | Removes Suppression of upgrades for `<package>` |

<br>

**Preferred search method**: `choco search --by-id-only --order-by-popularity --approved-only` `<package>`

If a package fails to install:
try `choco install -f <packagename>`
if that fails, check under `%TEMP%\Chocolatey` for the installer and, if found, **run it from there**.

<br>

**Options that affect checksum verification**:

`--ignore-checksums` - skips checksumming

`--allow-empty-checksums` - skips checksumming when the package is missing a checksum

`--allow-empty-checksums-secure` - skips checksumming when the package is missing a checksum for secure

<br>

# PowerShell

[Official Documentation](https://docs.microsoft.com/en-us/powershell/)

[Updating](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-windows?view=powershell-7.1) to PowerShell 7.2 (**Preview**):

    iex "& { $(irm https://aka.ms/install-powershell.ps1) } -UseMSI -Preview"

To get `PowerShell` **version** run:

    (Get-Host).Version

or

    $PSVersionTable

<br>

To **refresh** environmental variables from the registry run: `refreshenv`

<br>

We can **enable** and **disable** Windows Features from `PowerShell` with:

    Enable-WindowsOptionalFeature

    Disable-WindowsOptionalFeature

To list all these features with run:

    Get-WindowsOptionalFeature -Online | Sort-Object state | ft

To **disable** Internet Explorer 11 and Windows Media Player run:

    Disable-WindowsOptionalFeature -online -FeatureName internet-explorer-optional-amd64

    Disable-WindowsOptionalFeature -online -FeatureName WindowsMediaPlayer

To get the **status** of Internet Explorer 11 run:

    Get-WindowsOptionalFeature -Online -FeatureName Internet*

<br>

### Basic Commands

| Command                       | Short               | Description            |
| ----------------------------- | ------------------- | ---------------------- |
| Set-Location -Path C:\Windows | sl -path C:\Windows | Move to location path  |
| Get-ChildItem                 | gci                 | View directory content |
| Get-Location                  | gl                  | Reveals path           |
| Remove-Item                   | rm                  | Delete                 |
| New-Item                      | ni                  | Create file            |
| Copy-Item                     | cp                  | Copy file              |
| Move-Item                     | mv                  | Move file              |
| Get-Content                   | gc                  | Cat file               |

<br>

### $PROFILE

To get the location run:

    $PROFILE

If you don't have a profile yet, **create** one by running:

    notepad $PROFILE

<br>

### Autocompletion

To enable autocompletion on PowerShell you need to **set** the `ExecutionPolicy` of PowerShell to `RemoteSigned` this will **allow** unsigned local scripts and signed remote PowerShell scripts to run.

From a `PowerShell` with **administrator privileges** run:

    Set-ExecutionPolicy RemoteSigned

Include this at the beginning of your **$PROFILE**

    #autocompletion
    Register-ArgumentCompleter -Native -CommandName winget -ScriptBlock {
        param($wordToComplete, $commandAst, $cursorPosition)
            [Console]::InputEncoding = [Console]::OutputEncoding = $OutputEncoding = [System.Text.Utf8Encoding]::new()
            $Local:word = $wordToComplete.Replace('"', '""')
            $Local:ast = $commandAst.ToString().Replace('"', '""')
            winget complete --word="$Local:word" --commandline "$Local:ast" --position $cursorPosition | ForEach-Object {
                [System.Management.Automation.CompletionResult]::new($_, $_, 'ParameterValue', $_)
            }
    }

<br>

### Chocolatey Autocompletion

Open your `$PROFILE` and **include** this under the previous `autocompletion`

    #chocolatey autocompletion
    $ChocolateyProfile = "$env:ChocolateyInstall\helpers\chocolateyProfile.psm1"
    if (Test-Path($ChocolateyProfile)) {
    	Import-Module "$ChocolateyProfile"
    }

<!-- <br>

## Create Alias

To check if the alias exists: `$Alias:Edit`

Create a new alias: `Set-Alias Edit notepad.exe`

Just check if this `Edit` alias created successfully or not by running: `$Alias:Edit` if is made will output `Notepad.exe` will open.

That way you cant have permanent aliases when you close PowerShell will get removed so we can add that to our `$PROFILE`, so when the profile starts, the alias will be created automatically. -->

<br>

## Oh-My-Posh

Is a theme engine similar to oh-my-zsh, you can customize it extensively with colors and themes, more info on their [Github page](https://github.com/JanDeDobbeleer/oh-my-posh#prerequisites)

<br>

### [Installation](#ohmyposh-installation)

To install oh-my-posh and posh-git open `PowerShell` with **administrator privileges** and run

    Install-Module posh-git -Scope CurrentUser
    Install-Module oh-my-posh -Scope CurrentUser

Then add this to your `$PROFILE`

    #oh-my-posh
    Import-Module posh-git
    Import-Module oh-my-posh
    Set-Theme agnoster

You can list the current configuration with `$ThemeSettings`

and the Posh-Git settings with `$GitPromptSettings`

To set the initial location add: `set-location c:\User\<username>`

<br>

### Terminal Icons

run `Install-Module -Name Terminal-Icons`

Include this in your `$profile` under `#oh-my-post`

    #terminal-icons
    Set-TerminalIconsIconTheme devblackops
    Set-TerminalIconsColorTheme devblackops

<br>

### Fonts & Glyphs

[Nerd Fonts](https://www.nerdfonts.com/font-downloads) definitely are the most famous, you can go there to choice, not all fonts supports all glyphs but [MesloLGM Nerd Font](https://github.com/ryanoasis/nerd-fonts/tree/master/patched-fonts/Meslo) will cover almost everything.

Install some fonts and change them via PowerShell's `Properties` by right clicking the window title. Other things you can do there are to give some transparency for the background, **enable** `CTRL+SHIFT+V` for copy/paste and **disable** the annoying scroll forwarding.

<br>

#### $PROFILE Example

        #chocolatey autocompletion
        $ChocolateyProfile = "$env:ChocolateyInstall\helpers\chocolateyProfile.psm1"
        if (Test-Path($ChocolateyProfile)) {
        	Import-Module "$ChocolateyProfile"
        }

        #autocompletion
        Register-ArgumentCompleter -Native -CommandName winget -ScriptBlock {
            param($wordToComplete, $commandAst, $cursorPosition)
                [Console]::InputEncoding = [Console]::OutputEncoding = $OutputEncoding = [System.Text.Utf8Encoding]::new()
                $Local:word = $wordToComplete.Replace('"', '""')
                $Local:ast = $commandAst.ToString().Replace('"', '""')
                winget complete --word="$Local:word" --commandline "$Local:ast" --position $cursorPosition | ForEach-Object {
                    [System.Management.Automation.CompletionResult]::new($_, $_, 'ParameterValue', $_)
                }
        }

        #start directory
        set-location c:\Users\Username10

        #oh-my-posh
        Set-Theme Operator
        Import-Module posh-git
        Import-Module oh-my-posh

        #icons
        Set-TerminalIconsIconTheme devblackops
        Set-TerminalIconsColorTheme devblackops

<br>

# Windows Terminal

You can get the latest version from their [Github](https://github.com/microsoft/terminal).

<br>

### Customization

For fonts you can also get this patched [Cascadia Code](https://github.com/adam7/delugia-code) font or [Fira Code](https://github.com/tonsky/FiraCode) with glyphs from [Nerd Fonts](https://www.nerdfonts.com/font-downloads).

For color schemes and other visual pleasing stuff better **check** [this](https://github.com/mbadolato/iTerm2-Color-Schemes) and [this](https://github.com/willmcgugan/rich).

<br>

**_Global settings_**

    "defaults": {
        // Put settings here that you want to apply to all profiles.
        "startingDirectory": "%USERPROFILE%/Desktop/",
        "fontFace": "MesloLGS NF",
        "fontSize": 9,
        },

**_arch_**

    {
        "guid": "{a5a97cb8-8961-5535-816d-772efe0c6a3f}",
        "hidden": false,
        "name": "Arch Linux",
        "commandline": "wsl.exe -d Arch",
        "fontFace": "FiraCode Nerd",
        "fontSize": 9,
        "icon": "C:\\folder\\icons\\archlinux.ico",
        "backgroundImage": "E:\\folder\\Images\\sahara.jpg",
        "backgroundImageOpacity": 0.1,
        "historySize": 9000,
        "padding": "8, 4, 1, 4"
        },

**_cmd_**

    {
        // cmd.exe profile. EXAMPLE
        "guid": "{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",
        "name": "Command Prompt",
        "commandline": "cmd.exe",
        "fontSize": 10,
        "experimental.retroTerminalEffect": true,
        "hidden": false
        },

**_keybinds_**

    "actions": [
            // CTRL+F open search box
            { "command": "find", "keys": "ctrl+f" },

            // ALT+SHIFT+D open new panel
            { "command": { "action": "splitPane", "split": "auto", "splitMode": "duplicate" }, "keys": "ctrl+shift+d" },

            // CTRL+T open new tab
            {"command":"newTab", "keys": "ctrl+t"},

            // CTRL+C CTRL+V copy and paste
            { "command": {"action": "copy", "singleLine": false }, "keys": "ctrl+c" },
            { "command": "paste", "keys": "ctrl+v" }
    ]

<br>

# Windows Subsystem for Linux

You **should** refer first to the official documentation [WSL Installation Guide](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

## WSL Installation

**1.** You must **enable** Virtualization in BIOS first, and ensure that you updated Windows 10 to the latest version.

<br>

**2.** Enable the two Windows Features that needed by running `OptionalFeatures.exe` or via `PowerShell` with **administrator privileges**.

**Enable** Windows Subsystem for Linux:

        dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

**Enable** Virtual Machine:

        dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

After that **reboot**.

<br>

**3.** Download the **latest** Linux kernel package update:

WSL2 Linux kernel [update package for x64 machines](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

or you can run:

    Invoke-WebRequest -Uri https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi -OutFile WSLUpdate.msi -UseBasicParsing

    msiexec.exe /package WSLUpdate.msi /quiet

<br>

**4.** Switch from WSL 1 to WSL 2

>        wsl --set-default-version 2

From now on all new distributions will be WSL 2 if you need a WSL 1 distro you need to switch back with the same command, now you can get a distribution from [Microsoft Store](https://www.microsoft.com/en-us/search?q=linux) or from `PowerShell` as described below:

<br>

Ubuntu 20.04

    Invoke-WebRequest -Uri https://aka.ms/wslubuntu2004 -OutFile Ubuntu.appx -UseBasicParsing

Ubuntu 18.04

    Invoke-WebRequest -Uri https://aka.ms/wsl-ubuntu-1804 -OutFile Ubuntu1804.appx -UseBasicParsing

Ubuntu 16.04

    Invoke-WebRequest -Uri https://aka.ms/wsl-ubuntu-1604 -OutFile Ubuntu1604.appx -UseBasicParsing

Debian 10.5

    Invoke-WebRequest -Uri https://aka.ms/wsl-debian-gnulinux -OutFile debian.appx -UseBasicParsing

Open SUSE Leap 42

    Invoke-WebRequest -Uri https://aka.ms/wsl-opensuse-42 -OutFile opensuse.appx -UseBasicParsing

Kali Linux

    Invoke-WebRequest -Uri https://aka.ms/wsl-kali-linux-new -OutFile kali.appx -UseBasicParsing

SUSE Linux Enterprise Server 12

    Invoke-WebRequest -Uri https://aka.ms/wsl-sles-12 -OutFile suse-server.appx -UseBasicParsing

To install, navigate to folder containing the distribution downloaded, and in that directory run the following command where `app_name` is the name of your distro `.appx` file.

    Add-AppxPackage .\app_name.appx

WSL Ubuntu [Wiki](https://wiki.ubuntu.com/WSL)

<br>

## Accessibility

If you run from shell **`explorer.exe . `** Then windows filemanager will start on this location, same apples with VSCode if you give **`code . `** you will start code in that folder.

WSL provides a mount that allows you to safely access and edit files inside your Linux environment. It is available under **`\\wsl$\`** you can access it via Explorer file manager drop in the URL **`\\wsl$\`** and pin the location in the sidebar for quick access.

You can also access Windows drivers from WSL which are mounted at `/mnt/c/` `/mnt/d/` etc.. you can navigate at `/mnt/` and dir the folder to see the mount points, you can also list all drives on your computer with

    wmic diskdrive list brief

<br>

## Global Configuration

Configure global options are in `.wslconfig`

You can configure global WSL options by placing a .wslconfig file into the root directory of your users folder: `%USERPROFILE%\.wslconfig`. Please keep in mind you may need to run `wsl --shutdown` to shut down the WSL 2 virtual machine and then restart your WSL instance for these changes to take affect.

Here is a sample of a `.wslconfig` file:

    [wsl2]
    kernel=C:\\temp\\myCustomKernel
    memory=4GB
    processors=2
    swap=6GB
    swapFile=D:\\Temp\\WslSwap.vhdx
    # localhostForwarding=true

All WSL files are stored: `%LocalAppData%\Packages`

Ensure volume mounts work

`sudo nano /etc/wsl.conf` edit your `wsl.conf`:

    [automount]
    root = /
    options = "metadata"

<br>

### Import / Export

I totally advise after creating and setting up a distribution to export a backup for safe keeping.

    example:

    wsl --import Ubuntu C:\Ubuntu d:\WSL2Distros\ubuntu.tar
    wsl --export Ubuntu Ubuntu.tar --version 2     /// the location of the export is in $HOME

After importing a distribution you can login only as root to gain access again to the user run this from a `PowerShell`

    <distribution> config --default-user <username> --<wsl version>

<br>

Some useful utilities for WSL:

<br>

[Ansible-WSL](https://github.com/Wintus/Ansible-WSL) Provisioning your Windows by Ansible from WSL

<br>

[LxRunOffline](https://github.com/DDoSolitary/LxRunOffline) A full-featured utility for managing WSL

-   To install it: `choco install lxrunoffline`

[wlsu](https://github.com/wslutilities/wslu) A collection of utilities for WSL

-   To install the latest version before wslu reaches main reporsitory:

          sudo add-apt-repository ppa:wslutilities/wslu

-   Update and install:

          sudo apt update && sudo apt install -y ubuntu-wsl

<br>

## WSL Basic Commands

| Command                                                    | Description                                            |
| ---------------------------------------------------------- | ------------------------------------------------------ |
| `wsl --help`                                               | Help                                                   |
| `wsl --set-version <distribution> <version number>`        | Change WSL version on a distribution                   |
| `wsl --selectdefault <distribution>`                       | Change default distribution that will start on wsl.exe |
| `wsl --list --verbose`                                     | List of installed distributions with WSL version       |
| `wsl --user <username>`                                    | Run as a specific User                                 |
| `wsl --export <distribution> <filename>.tar`               | To backup a distribution                               |
| `wsl --import <distribution> <installLocation> <filename>` | To import a distribution from a known location         |
| `wsl --terminate <distribution>`                           | To stop a distribution                                 |
| `wsl --unregister <distribution>`                          | To remove a distribution                               |
| `wsl --default-user <username>`                            | To change the default user of a distribution           |
| `wsl --distribution <distribution>`                        | To start a specific distribution                       |
| `wslconfig /setdefault <distribution`                      | To set the default distrubution of wsl.exe             |

<br>

[Command Line Reference](https://docs.microsoft.com/en-us/windows/wsl/wsl-config)

<br>

## WSL Ubuntu

### First Steps

<br>

To see the repositiries `sudo nano /etc/apt/sources.list`

Create a User and a Password and then update your mirrorlist and upgrade your packages.

    sudo apt update && sudo apt -y upgrade

You can change the LTS status of your distribution to normal so you can upgrade version

    sudo nano /etc/update-manager/release-upgrades

`lsb_release -a` for checking distribution version

`sudo do-release-upgrade` for updating distribution version

You can Update Ubuntu to `20.10` if you want.

<br>

You might want to add Canonical partners repository if you planning to add a [desktop](https://gist.github.com/tdcosta100/385636cbae39fc8cd0937139e87b1c74) like [XFCE](https://www.xfce.org/) later with `tigervnc` server

    sudo add-apt-repository -y "deb http://archive.canonical.com/ $(lsb_release -sc) partner" && sudo apt update

<br>

### Apt Packages

Needed packages:

    sudo apt install -y mc zsh fzf ripgrep catbat pydf ssh ca-certificates gnupg-agent apt-transport-https
    software-properties-common exa rar unrar unzip build-essential rsync gpg cifs-utils youtube-dl
    tldr httpie memcached libsecret-1-0 libsecret-1-dev  aria2 cmake net-tools neofetch libssl-dev irssi

<br>

If some packages are broken then use any one of these three commands to fix those:

    sudo apt --fix-broken install

    sudo apt --fix-missing upgrade

    sudo dpkg --configure -a

<br>

_Worth Mentioning **cli tools**_:

<br>

[fzf](https://github.com/junegunn/fzf/) is a powerful cli filter `sudo apt install -y fzf`

[mc](https://github.com/MidnightCommander/mc/) is a cli file manager `sudo apt install -y mc`

[exa](https://github.com/ogham/exa) is a colorful replacement for `ls` command `cargo install exa`

[bat](https://github.com/sharkdp/bat/) is a colorful replacement for `cat` its called `batcat` on Ubuntu `sudo apt install -y bat`

[ripgrep](https://github.com/BurntSushi/ripgrep/) is a colorful replacement for `grep` command `sudo apt install -y ripgrep`

[aria2](https://aria2.github.io/) is a replacement of `wget` command `sudo apt install -y aria2`

[tldr](https://github.com/tldr-pages/tldr) is a companion for the `man` command

[broot](https://github.com/Canop/broot) is a treeview directory navigation tool `cargo install broot`

[fd](https://github.com/sharkdp/fd) is a replacement for `find` command

[forgit](https://github.com/wfxr/forgit) is a tool is to help you use git more efficiently

[tig](https://github.com/jonas/tig) is a text-based interface for git repositories

[hub](https://github.com/github/hub) is a cli tool that wraps git in order to extend it, for working with GitHub easier

[httpie](https://github.com/httpie/httpie) is a command-line HTTP client

<br>

### Aliases

To create a alias in bash the syntax is `alias Name='command -v --d'` and you add them in your `~/.zshrc`

| Alias      | Command                                                                         | Description                       |
| ---------- | ------------------------------------------------------------------------------- | --------------------------------- |
| ..         | `cd ..`                                                                         | Folder navigation                 |
| ...        | `cd ../..`                                                                      |                                   |
| ....       | `cd ../../..`                                                                   |                                   |
| upgrade    | `sudo apt update && sudo apt upgrade -y`                                        | Update & Upgrade                  |
| cat        | `batcat`                                                                        | `batcat` replaces `cat` command   |
| grep       | `rg`                                                                            | `ripgrep` replaces `grep` command |
| zshrc      | `code ~/.zshrc`                                                                 | Open `.zshrc` with VSCode         |
| sourcezsh  | `source ~/.zshrc`                                                               | Source `.zshrc` changes           |
| wget       | `aria2`                                                                         | `aria2` replaces `wget`command    |
| lrt        | `broot`                                                                         | To backup a distribution          |
| l          | `exa -l --icons --color=always --group-directories-first`                       | `exa` listing options             |
| ll         | `exa -a --icons --color=always --group-directories-first`                       |                                   |
| la         | `exa -alF --icons --color=always --group-directories-first`                     |                                   |
| le         | `exa -lGFh --icons --color=always --group-directories-first`                    |                                   |
| nvmupgrade | `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.0/install.sh bash` | `nvm` update                      |
| repos      | `sudo nano /etc/apt/sources.list`                                               | Ubuntu repositories               |
| d4         | `pydf`                                                                          | Colorful list of disks            |
| free       | `free -m`                                                                       | Shows available memory            |

<br>

## git

Check the [Official Documentation](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-git) first.

<br>

Setup your **Username**:

    git config --global user.name "Your Name"

Setup your **Password**:

    git config --global user.email "youremail@domain.com"

Git Credential Manager to enable you authenticate remote Git servers

    git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/libexec/git-core/git-credential-manager.exe"

<br>

Or you can:

**Inclune** this in credenatial manager: `sudo nano /usr/bin/git-credential-manager`

    #!/bin/sh
    exec "/mnt/c/Program\ Files/Git/mingw64/libexec/git-core/git-credential-manager.exe" $@

Give the script **permissions**: `sudo chmod +x /usr/bin/git-credential-manager`

**Add** this to your `.gitconfig`: `sudo nano ~/.gitconfig`

    [credential]
    helper = manager

<Br>

The location of git is at `$HOME` folder.

    \\wsl$\Ubuntu-20.04\home\Username

    C:\Users\Username

Sometime you might need to [generate a new gpg key](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/generating-a-new-gpg-key) to use with GitHub.

To check or edit your git **configuration** access it with:

    nano ~/.gitconfig

If you are using a GPG key for code signing security, you may need to associate your GPG key with your GitHub email

To generate **private** and **public** key for git for:

    ssh-keygen -t rsa -b 4096 -C "email@email.com"

Your identification and key are saved at `$HOME/.ssh/`

<br>

**Permanently authenticating with Git repositories**

Run following command to **enable** credential caching:

    git config credential.helper store

    git push https://github.com/repo.git

**Username** for `'https://github.com'`: `<USERNAME>`

**Password** for `'https://USERNAME@github.com'`: `<PASSWORD>`

Use should also specify caching expire

    git config --global credential.helper "cache --timeout 7200"

After enabling **credential caching**, it will be cached for 7200 seconds (2 hour).

Read credentials **docs** also: `git help credentials`

<br>

**VS Code as Git editor**

From the command line, run: `git config --global core.editor "code --wait"`

Now you can run: `git config --global -e` and use VS Code as **editor** for configuring git.

<br>

### GitHub CLI

[gh](https://github.com/cli/cli#debianubuntu-linux) is GitHub on the command line. It brings pull requests, issues, and other GitHub concepts to the terminal next to where you are already working with git and your code.

https://github.com/cli/cli#debianubuntu-linux

https://github.com/cli/cli/blob/trunk/docs/install_linux.md

To [install](https://github.com/cli/cli/blob/trunk/docs/install_linux.md) it
you have to add key and repository

    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-key C99B11DEB97541F0

    sudo apt-add-repository https://cli.github.com/packages

Reload the local package database and install **gh**:

    sudo apt update && sudo apt install gh -y

To confirm run:

    gh --version

<br>

Now you have to login for the first time so run:

    gh auth login

<br>

**gh commands**:

| Command           | Description |
| ----------------- | ----------- |
| gh issue list     |             |
| gh pr status      |             |
| gh pr checkout    |             |
| gh pr create      |             |
| gh pr check       |             |
| gh release create |             |
| gh repo view      |             |
| gh alias set      |             |

<br>

#### SSH with git

Go to Github Settings and link SSH key `/$HOME/.ssh/id_rsa`

    eval `ssh-agent`

    ssh-add /$HOME/id_rsa

To deal with cross platform problems, where sometimes git recognize changes in files, when there are none. This is due to windows using `CRLF` and Linux `LF` to signify an end of line. To fix that, the following line can be run:

    git config --global core.autocrlf input

**SSH repository**

    git remote add origin git@github.com:<username>/<repo>.git

<br>

#### [First Commit](https://gist.github.com/mindplace/b4b094157d7a3be6afd2c96370d39fad)

Using your terminal/command line, get inside the folder where your project is kept and run:

1.  `git init`
1.  `git add .`
1.  `git remote add https://www.github.com/<username>/<repo>.gr`
1.  `git commit`
1.  `git push origin master`

<br>

**Remove repository link**

    git remote -v
    git remove rm origin

<br>

#### .gitignore

[.gitignore](https://docs.github.com/en/free-pro-team@latest/github/using-git/ignoring-files) ..... ... . ..

<!-- Profiles can be defined to launch %windir%\system32\bash.exe ~. -->

<br>

## SSH

    sudo apt install -y ssh

Edit the configuration file

    sudo nano /etc/ssh/sshd_config

To disable clear text passwords, change to no here change `PasswordAuthentication` to **yes**

Add your login user to the bottom of the file to check which is your username you can run `whoami`

    AllowUsers yourusername

Generate **new** Host Keys

    cd /etc/ssh && sudo ssh-keygen -A

Know that we have the **keys** we can start our service.

    sudo service ssh start

    sudo service ssh status

Check the **status** of the service to confirm that is running

    service ssh status

Fix Host keys

    sudo apt-get remove --purge openssh-server

    sudo apt-get install openssh-server

    sudo service ssh --full-restart

<br>

## Shell

To see in which **shell** you are on run `echo "$SHELL"`

To install zsh run:

    sudo apt install -y zsh

You can run `zsh` to switch from bash.

To change the default shell from bash to zsh
run `type -a zsh` to find the path and then:
`chsh -s /bin/bash/`

The configuration file is **`~/.zshrc`** you will **often** do changes on this file, better make an alias to open it quickly

<br>

add this to the **bottom** of your `~/.zshrc` with the other aliases.

    alias zshrc='code ~/.zshrc'

    alias sourcezsh='source ~/.zshrc'

<br>

If you want to set default browser for the current terminal session you can set a `BROWSER` variable.

    export BROWSER='/mnt/c/Program Files/Google/Chrome/Application/Chrome.exe'

Assuming you want to use **Chrome** and have it installed in that **location**, if you want this to be persistent, you can include it in your `~/.zshrc` file.

<br>

We can even play [tetris](https://github.com/zsh-users/zsh/blob/master/Functions/Misc/tetriscurses) in zsh 😆 🤲

    #zsh tetris
    autoload -Uz tetriscurses
    tetriscurses

<br>

## Oh My Zsh

[Oh My Zsh](https://github.com/ohmyzsh/ohmyzsh) is a framework for managing your zsh configuration and includes a plugin manager that can add a ton of **functionality** to our shell, to install it run:

    sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

It will ask you to change the default shell to zsh **accept** it if you want to change back to bash as default run `chsh -s /bin/bash/`

to update ohmyzsh run **`omz update`**

<br>

## Plugins

You can find a [full list](https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins) in ohmyzsh Wiki page
You manage plugins in the `~/.zshrc` under

`plugins=(...)` you can add more plungins inside `( )`

<br>

[zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)

    cd ~/.oh-my-zsh/plugins/ && git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting && cd ~

then in your `~/.zshrc` file **include** the plugin name `zsh-syntax-highlighting` in the plugins section.

[zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

    cd ~/.oh-my-zsh/plugins/ && git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions && cd ~

Add the plugin name `zsh-autosuggestions` to the **list** of plugins in `~/.zshrc`

[zsh-history-substring-search](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/history-substring-search)

    cd ~/.oh-my-zsh/plugins/ && git clone https://github.com/zsh-users/zsh-history-substring-search ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-history-substring-search && cd ~/

Add the plugin to the **list** of plugins in `~/.zshrc`

[Copy-Pasta](https://github.com/ChrisPenner/copy-pasta)

    cd ~/.oh-my-zsh/plugins/ && git clone https://github.com/ChrisPenner/copy-pasta.git && cd ~/

Add the plugin name `copy-pasta` to the list of plugins in `~/.zshrc`

Most plugins don't need installation because are just aliases, you can just extract them and add them directly in your `~/.zshrc`, you **should check each one of these** in their respective **git page** before you `enable` them because ohmyzsh becomes heavy pretty easily.

some **recommendations**:

    plugins=(
        ubuntu
        zsh-syntax-highlighting
        zsh-autosuggestions
        zsh-history-substring-search
        zsh-interactive-cd
        zsh_reload
        docker-compose
        docker-machine
        copy-pasta
        sublime
        node
        npm
        nvm
        colored-man-pages
        colorize
        copydir
        copyfile
        emoji-clock
        emotty
        history
        fzf
        ripgrep
        web-search
        git
        fd
        z
        )

<br>

### Antigen Plugin Manager

To install Antigen clone the stable or the nightly version:

    git clone https://github.com/zsh-users/antigen.git ~/antigen

    nightly -- curl -L git.io/antigen-nightly > antigen.zsh

And then source it into your `~/.zshrc`

    source ~/antigen.zsh

<br>

To use it:

    #load the oh-my-zsh library
    antigen use oh-my-zsh

    #load plugins
    antigen bundle pyenv
    antigen bundle pip
    antigen bundle npm
    antigen bundle nvm
    antigen bundle yarn
    antigen bundle sudo
    antigen bundle colored-man-pages
    antigen bundle colorize
    antigen bundle history
    antigen bundle fzf
    antigen bundle ripgrep
    antigen bundle web-search
    antigen bundle zsh_reload
    antigen bundle zsh-interactive-cd
    antigen bundle zsh-users/zsh-completions
    antigen bundle zsh-users/zsh-autosuggestions
    antigen bundle zsh-users/zsh-syntax-highlighting

    #load theme
    antigen theme romkatv/powerlevel10k

    #always remember to apply at the end
    antigen apply

To get a list of what you are loading run: `antigen list --long`

<br>

## Powerlevel10k Theme

[Powerlevel10k](https://github.com/romkatv/powerlevel10k) is a theme for zsh to **install** it run:

First clone the repo and configure the theme you need to have **already** set in your terminal a font that supports **glyphs**.

    git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k && echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc

Now `source ~/.zshrc` so the change take effect and get in the p10k configuration.You can restart configuration sequence with `p10k configure` also the theme settings are located at:`~/.p10k.zsh`

<br>

to **update** Powerlevel10k run:

    git -C ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k pull

<br>

# Development

<br>

## Javascript

### nodejs

To install [node.js](https://github.com/nodejs/node) run:

    curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -

you may **need** also `sudo apt-get install gcc g++ make` then install nodejs

    sudo apt-get install -y nodejs

Now `node -v` and `npm -v` to confirm that it is installed

For Current and LTS, the GPG detached signature of SHASUMS256.txt is in SHASUMS256.txt.sig. You can use it with gpg to verify the integrity of SHASUM256.txt. You will first **need** to import the GPG keys of individuals authorized to create releases.

To import the **keys**:

    gpg --keyserver pool.sks-keyservers.net --recv-keys DD8F2338BAE7501E3DD5AC78C273792F7D83545D

Download the `SHASUMS256.txt.sig` for the release:

    curl -O https://nodejs.org/dist/vx.y.z/SHASUMS256.txt.sig

Then use: `gpg --verify SHASUMS256.txt.sig SHASUMS256.txt` to verify the file's signature.

<br>

### nvm

[nvm](https://github.com/nvm-sh/nvm#install--update-script) is a version manager for node.js, designed to be installed per-user, so to install it run:

    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.0/install.sh | bash

<br>

**Include** this in your **`~/.zshrc`**

    export NVM_DIR="$HOME/.nvm"
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
    [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

Then source your `.zshrc` and it should tell you that the compile was successful
to update NVM we have to repeat the same so to be faster we make an alias called nvmupgrade
and include it in the bottom of `~/.zshrc`

    alias nvmupgrade='curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.0/install.sh | bash'

You can install also [node.js](https://github.com/nodejs/node) via nvm by running: `nvm install --lts`

`nvm ls`

`nvm ls-remote`

`nvm install v14.15.0`

`nvm install --lts`

`node -v`

<br>

### npm

To install a global package run:

    npm install -g typescript
    npm install -g eslint

To list the outdated global packages run:

    npm outdated -g

To update global packages:

    npm update -g                   # for all the packages
    npm update -g <PackageName>     # for individual packages

For local run it in your project folder and just remove the `-g` flag.

<br>

### yarn

To install the [yarn](https://github.com/yarnpkg/yarn) package manager, run:

     curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -

     echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

     sudo apt-get update && sudo apt-get install yarn

Now `yarn -v` to confirm that it is installed

You have to also include this in your `~/.zshrc`

<!-- export PATH="$PATH:$(yarn global bin)" -->

    export PATH="$PATH:$(yarn global bin)"

or

    export PATH="$HOME/.yarn/bin:$HOME/.config/yarn/global/node_modules/.bin:$PATH"

Also you can simple install it through [npm](https://github.com/npm/npm) `sudo npm i -g yarn`

<!-- ### create-react-app

yarn global add create-react-app
create-react-app --version -->

<br>

### Deno

[Installation](https://deno.land/manual/getting_started/installation), for official documentation click [here](https://deno.land/)

    curl -fsSL https://deno.land/x/install/install.sh | sh

**include** these in your `~/.zshrc`

    export DENO_INSTALL="/home/korum/.deno"
    export PATH="$DENO_INSTALL/bin:$PATH"

To refresh the changes

    source ~/.zshrc

To confirm that the installation completed

    deno --version

To upgrade previous installation of Deno run:

    deno upgrade

You can also use this utility to install a **specific** version of Deno:

    deno upgrade --version 1.0.1

<br>

## Python

To Install Python3:

    sudo apt-get install -y python3 python3-pip build-essential libssl-dev libffi-dev python3-dev

    pip3 install virtualenv pylint

<br>

[pyenv](https://github.com/pyenv/pyenv) python version manager

     git clone https://github.com/pyenv/pyenv.git ~/.pyenv

source to `~/.zshrc`

        echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc

        echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc

Then we can choose Python version and make it **global**:

    pyenv install 3.8.6

    pyenv global 3.8.6

<br>

**miniconda**

    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

If you'd prefer that conda's base environment not be activated on startup,
set the `auto_activate_base` parameter to `false`:

    conda config --set auto_activate_base false

<br>

## Docker

<br>

### Docker [Installation](https://docs.docker.com/engine/install/ubuntu/)

Install docker's **dependencies** first.

    sudo apt install apt-transport-https ca-certificates curl software-properties-common -y

**Download** and add Docker's official public PGP key.

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

**Verify** the fingerprint.

    sudo apt-key fingerprint 0EBFCD88

Add the `stable` channel's Docker upstream repository.

    sudo add-apt-repository \
       "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
       $(lsb_release -cs) \
       stable"

Update the apt package list (for the new apt repo).

    sudo apt update

Install the latest version of Docker CE.

    sudo apt install -y docker-ce docker-ce-cli containerd.io

Allow your user to access the Docker CLI without needing root access.

    sudo usermod -aG docker $USER

 <br>

### Docker Compose

    pip3 install --user docker-compose

    docker-compose --version

Make sure `$HOME/.local/bin` is set on your WSL `$PATH` , If it’s not there, you’ll want to add it to your `$PATH`

    echo $PATH

Connect to a remote Docker daemon to port 2375:

    echo "export DOCKER_HOST=tcp://localhost:2375" >> ~/.zshrc && source ~/.zshrc

Verify it works, if you get error just restart your terminal session and try again

    docker info

<br>

### Docker Commands

For more details see [Official Documentation](https://docs.docker.com/engine/reference/commandline/docker/)

| Command                                              | Description                                                       |
| ---------------------------------------------------- | ----------------------------------------------------------------- |
| `docker –version`                                    | To get the currently installed version of docker                  |
| `docker pull <image name>`                           | To pull images from the docker repository                         |
| `docker run -it -d <image name>`                     | To create a container from an image                               |
| `docker ps`                                          | To list the running containers                                    |
| `docker ps -a`                                       | To show all the running and exited containers                     |
| `docker exec -it <container id> bash`                | To access the running container                                   |
| `docker stop <container id>`                         | To stop a running container                                       |
| `docker kill <container id>`                         | To Kills the container by stopping its execution immediately      |
| `docker commit <conatainer id> <username/imagename>` | To creates a new image of an edited container on the local system |
| `docker login`                                       | To login to the docker hub repository                             |
| `docker push <username/image name>`                  | To push an image to the docker hub repository                     |
| `docker images`                                      | To lists all the locally stored docker images.                    |
| `docker rm <container id>`                           | To delete a stopped container.                                    |
| `docker rmi <image-id>`                              | To delete an image from local storage.                            |
| `docker build <path to docker file>`                 | To build an image from a specified docker file.                   |

<br>

## Rust

[Documentation](https://doc.rust-lang.org/stable/)

[Installation](https://github.com/rust-lang/rust)

    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

Import the environment config for Rust to your `~/.zshrc`

    source $HOME/.cargo/env

You can **verify** the installation by running:

    rustc --version

    cargo --version

The Rust build system uses a file named `config.toml` in the root of the source tree to determine various configuration settings for the build.

<br>

## Nginx

[Documentation](https://nginx.org/en/docs/)

You can access it via 127.0.0.1 localhost the configuration is at `/etc/nginx/nginx.conf`

<br>

_Installation:_

    sudo apt install -y nginx

_Start:_

    sudo nginx

_Stop:_

    sudo nginx -s quit

Adjust the Firewall to **Allow** Web Traffic

    sudo ufw app list

    sudo ufw app info "Apache Full"

    sudo ufw allow 'Nginx Full'

<br>

## Apache2

[Documentation](https://httpd.apache.org/docs/)

The **main** configuration file is located at `/etc/apache2/apache2.conf`

`ports.conf` is used to specify the ports that virtual hosts should listen on.

Installation:

    sudo apt update

    sudo apt install -y apache2

Adjust the Firewall to **Allow** Web Traffic

    sudo ufw app list

    sudo ufw app info "Apache Full"

    sudo ufw allow in "Apache Full"

How To Find your Server’s Public **IP** Address

    ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'

or you can `curl` the `URL`.

<br>

_Installation:_

    sudo apt install apache2

_Start:_

    sudo service apache2 start

<br>

## MongoDB

##### **This doesnt apply on wsl-ubuntu** `2010` **and** `2004` , **Mongodb works only in wsl-ubuntu** `1804` **under the package name** `mongodb`, [installation guide](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-database)

<br>

<br>

[Installation](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/):

Once the packages have updated, install MongoDB with:

    wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -

Install **gnupg** and its required libraries using the following command:

    sudo apt install -y gnupg

Once installed, retry importing the **key**:

    wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -

Confirm installation and get the version number: `mongod --version`

Create a **list file** for MongoDB.

Create the `/etc/apt/sources.list.d/mongodb-org-4.4.list` file for `Ubuntu` `20.04 (Focal)`:

    echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list

Reload local package database.

    sudo apt update

Install the lastest version:

    sudo apt install -y mongodb-org

Optional. Although you can specify any available version of MongoDB, apt will upgrade the packages when a newer version becomes available. To **prevent** unintended upgrades, you can pin the package at the currently installed version:

    echo "mongodb-org hold" | sudo dpkg --set-selections

    echo "mongodb-org-server hold" | sudo dpkg --set-selections

    echo "mongodb-org-shell hold" | sudo dpkg --set-selections

    echo "mongodb-org-mongos hold" | sudo dpkg --set-selections

    echo "mongodb-org-tools hold" | sudo dpkg --set-selections

<br>

Configuration file: `/etc/mongod.conf`

<br>

Checking the status of your database.

    sudo service mongodb status for

To start running your database.

    sudo service mongodb start

To stop running your database.

    sudo service mongodb stop

<br>

## MYSQL

[Installation](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-database):

<!-- https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-ubuntu-18-04 -->

    sudo apt install -y mysql-server default-mysql-server default-mysql-client

When the installation is complete, run a simple security script that comes pre-installed with MySQL which will remove some dangerous defaults and lock down access to your database system. Start the interactive script by running:

    sudo mysql_secure_installation

To open the MySQL prompt, enter: `sudo mysql`

To see what databases you have available, in the MySQL prompt, enter: `SHOW DATABASES;`

To create a new database, enter: `CREATE DATABASE database_name;`

To delete a database, enter: `DROP DATABASE database_name;`

`mysql --version`

<br>

**Configuration files**:

    /etc/my.cnf
    /etc/mysql/my.cnf
    /var/lib/mysql/my.cnf

To see if database is running:

    mysql -h 127.0.0.1 -P 3306 -u root -p <database>

    telnet 127.0.0.1 3306

<br>

Status: `sudo service mysql status`

Start: `sudo /etc/init.d/mysql start`

Stop: `sudo /etc/init.d/mysql stop`

<br>

### Password Reset

To reset database password follow these steps:

1.  Stop `mysql` - `sudo /etc/init.d/mysql stop`
2.  Start MySQL in safe mode: `sudo mysqld_safe --skip-grant-tables &`
3.  Log into MySQL using root: `mysql -u root`
4.  Select the MySQL database to use: `use mysql;`
5.  Reset the password

        -- MySQL version < 5.7

        update user set password=PASSWORD("mynewpassword") where User='root';

        -- MySQL 5.7, mysql.user table "password" field -> "authentication_string"

        update user set authentication_string=password('mynewpassword') where user='root';

6.  Flush the privileges: `flush privileges;`
7.  Restart the server: `quit`
8.  Stop and start the server again:
    `sudo /etc/init.d/mysql stop`
    `sudo /etc/init.d/mysql start`

9.  Login with a new password: `mysql -u root -p`

Type the new password.

<br>

...

...

<br>

<!-- NA BALW KAI AUTA MESA -->
<!-- CREATE USER 'meu-nome-de-usuario'@'localhost' IDENTIFIED BY 'MinhaSenhaSecreta'; FLUSH PRIVILEGES;
GRANT ALL PRIVILEGES ON _._ TO 'meu-nome-de-usuario'@'localhost'; FLUSH PRIVILEGES;
GRANT GRANT OPTION ON _._ TO 'meu-nome-de-usuario'@'localhost'; FLUSH PRIVILEGES; -->

<br>

## PHP

Add the repository:

    sudo add-apt-repository ppa:ondrej/php

Install PHP:

    sudo apt install -y apt-transport-https php7.4-fpm php7.4-mbstring php7.4-curl php7.4-json php7.4-bz2 php7.4-zip php7.4-xml php7.4-gd php7.4-mysql php7.4-intl php7.4-sqlite3 php7.4-soap php7.4-bcmath php7.4-memcached php7.4-redis

    sudo apt install libapache2-mod-php7.3 php7.3 php7.3-common php7.3-curl php7.3-gd php7.3-imagick php7.3-mbstring php7.3-mysql php7.3-json php7.3-xsl php7.3-intl php7.3-zip

Optional dependencies:

    sudo apt install -y dos2unix memcached default-jre  dh-autoreconf beanstalkd redis-server pv ack unoconv

<!-- na ton dw ksana autwn -->
<!-- https://github.com/enflow/wsl2-php-development -->

Confirm version:

    php -version

<!-- sudo apt install php libapache2-mod-php php-mysql
sudo service apache2 restart -->

<br>

### Composer

Install [Composer](https://getcomposer.org/download/)

    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"

    php -r "if (hash_file('sha384', 'composer-setup.php') === '756890a4488ce9024fc62c56153228907f1545c228516cbf63f885e036d37e9a59d27d63f46af1d4d07ee0f76181c7d3') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"

    php composer-setup.php

    php -r "unlink('composer-setup.php');"

Then you 'll have `php composer.phar` in the folder you did the installation

Configuration files are, for **local** `composer.json`, for **global** `config.json`

<br>

### phpMyAdmin

[phpmyadmin](https://www.phpmyadmin.net/)

    sudo apt install -y phpmyadmin

<!-- sudo apt update
sudo apt install phpmyadmin php-mbstring
sudo phpenmod mbstring

sudo vim /etc/apache2/apache2.conf

Include /etc/phpmyadmin/apache.conf -->

<br>

## Prestashop

[Prestashop DevDocs](https://devdocs.prestashop.com/1.7/basics/introduction/) [php-ps-info](https://github.com/PrestaShop/php-ps-info/releases)

[Development Installation](https://devdocs.prestashop.com/1.7/basics/installation/localhost/)

<br>

To install **LAMP** on your computer follow these steps:

<br>

-   **Update your system**

          sudo apt update

-   **Install MySQL**

          sudo apt install -y default-mysql-server default-mysql-client

-   **Install Apache2 server**

          sudo apt install -y apache2

-   **Install PHP 7.3**

          sudo apt install -y libapache2-mod-php7.3 php7.3 php7.3-common php7.3-curl php7.3-gd php7.3-imagick php7.3-mbstring php7.3-mysql php7.3-json php7.3-xsl php7.3-intl php7.3-zip

-   **Creating a database for your shop**

    If you are installing PrestaShop on a web server, then you must create the database and give access to a privileged user. You will need this user’s credentials to configure PrestaShop during the installation process.

-   **Using phpMyAdmin**

    We assume you have root access to phpMyAdmin, and you’re using version 4.x.

    Sign in to phpMyAdmin as the root user

    Click User accounts, and then click on Add user account
    Fill the User name and the Password
    In the Database for user account, select Create database and Grant all privileges
    Create user and database and make sure the COLLATION of your database is utf8mb4_general_ci

<br>

## Wordpress

-   **Installation**

    To install [WordPress](https://ubuntu.com/tutorials/install-and-configure-wordpress#1-overview) run:

        sudo apt install -y wordpress php libapache2-mod-php mysql-server php-mysql

    <br>

-   **Configure Apache for WordPress**

    Create Apache site for WordPress. Create `/etc/apache2/sites-available/wordpress.conf` with following lines:

          Alias /blog /usr/share/wordpress
          <Directory /usr/share/wordpress>
              Options FollowSymLinks
              AllowOverride Limit Options FileInfo
              DirectoryIndex index.php
              Order allow,deny
              Allow from all
          </Directory>
          <Directory /usr/share/wordpress/wp-content>
              Options FollowSymLinks
              Order allow,deny
              Allow from all
          </Directory>

    Then, enable this site with `sudo a2ensite wordpress`, enable URL rewriting with `sudo a2enmod rewrite` and reload apache2 with `sudo service apache2 reload`

    <br>

-   **Configure database**

            sudo mysql -u root          # and choose 1

    Now, let’s configure WordPress to use this database. Open /etc/wordpress/config-localhost.php and write:

            <?php
            define('DB_NAME', 'wordpress');
            define('DB_USER', 'wordpress');
            define('DB_PASSWORD', '<your-password>');
            define('DB_HOST', 'localhost');
            define('DB_COLLATE', 'utf8_general_ci');
            define('WP_CONTENT_DIR', '/usr/share/wordpress/wp-content');
            ?>

    Enable `MySQL` with: `sudo service mysql start`

    <br>

-   **Configure WordPress**

    Open `localhost/blog` in your browser. You will be asked for title of your new site, username, password and address e-mail.

    You can now login under `localhost/blog/wp-login.php` to dashboard.

<!-- wget [https://wordpress.org/latest.tar.gz](https://wordpress.org/latest.tar.gz); tar -vzxf latest.tar.gz; mv wordpress **nova-pasta**;

http://localhost/**nova-pasta** -->
<br>

# WSL Archlinux

<br>

[yuk7 /
ArchWSL](https://github.com/yuk7/ArchWSL)

Run on `command prompt` with **privileges**.

To install or start Archlinux: `arch.exe`

To create a backup: `arch.exe backup`

To recover from a backup: `arch.exe install backup.tar.gz`

To uninstall Archlinux: `arch.exe clean`

To set ArchLinux as default WSL just run: `wsl --set-default Arch`

<br>

## Archlinux Installation

Download [ArchWSL](https://github.com/yuk7/ArchWSL) installer zip, pick a folder (for example, `C:\Archlinux`) and run installer .exe in that folder as **admin**.

<br>

## Initialize package manager

To get into pacman's configuration run: `nano /etc/pacman.conf` You can enable the `multilib` repository from there also you can include ILoveCandy.

To get the correct mirrolists run: `/etc/pacman.d/mirrorlist`

<br>

## Refresh Pacman GPG keys:

    pacman-key --init

    pacman-key --populate archlinux

Run `pacman -Syyu` to update all packages to the latest versions.

<br>

## Add User

-   Create a User and a Home folder

    `useradd --create-home USERNAME`

-   Set password for that User

    `passwd USERNAME`

-   Add User to the groups

    `usermod -aG wheel,users,storage,power,lp,adm,optical USERNAME`

    `id USERNAME`

-   Wheel Group

    Edit `nano /etc/suders` and you have to uncomment the wheel=all under root

-   Set default user for Archlinux

    `Arch.exe config --default-user USERNAME`

    `Arch.exe or su -l USERNAME`

<br>

### Install YAY

Yay is an AUR Helper

    sudo pacman -S base-devel

When asked question on fakeroot and fakeroot-tcp choose to leave `fakeroot-tcp` and not install `fakeroot`, just say `N`

<br>

To install yay first clone it into `/opt`

    cd /opt && sudo git clone https://aur.archlinux.org/yay-git.git

Give it permissions so we can make it

    sudo chown -R USERNAME ./yay-git

Enter yay's folder and run makepkg

    cd yay-git

    makepkg -si

Run `yay -Syu` to update all AUR packages and reinstall `fakeroot-tcp` (will install the latest version) and also remove the leftovers: `rm -rf ~/yay-git`

<br>

## Accessing WSL from Windows

All WSL machines are available in Windows Explorer on `\\wsl$\` URL, Arch linux is: `\\wsl$\Arch`.
For additional convenience you could map `\\wsl$\Arch` to a 1-Letter Windows disk share, for example having `Z:\` pointing to `\\wsl$\Arch`

<br>

<br>

# WSL CentOS 8

You need to have [docker](https://docs.docker.com/docker-for-windows/install/) installed and run all the commands on `cmd`.

## CentOS 8 Installation

We need to pull the later CentOS image from [Docker hub](https://hub.docker.com/) and create it in your folder of choice, so navigate to the folder you want to do the installation and run:

    docker image pull centos:latest

    docker create -i centos bash

Then we need the 4 first digits from the above `create` command output.

    docker export [4 digits]  > centos.tar

    ** for example
    docker export 74c3 > centos.tar

<br>

Now open `PowerShell`on the same folder and import the image:

    wsl --import Centos8 .\CentosImage\ centos.tar

To get in the distribution run: `wsl -d Centos8` then `cat /etc/centos-release` to see the version

To list available distributions: `wslconfig /l`

To set CentOS 8 as the default distribution wsl.exe will use: `wslconfig /s Centos8`

<br>

## dnf

To list all installed packages

    dnf list installed |less

To list all repositories

    dnf repolist

Package managment

    dnf info <package>

    dnf provides <package>

    dnf search <package>


    dnf install -y <package>

    dnf remove <package>


    dnf update <package>

    dnf --exclude=<package>* update


    dnf update -y

    dnf install -y git wget curl

To install Development Tools on RHEL 8 / CentOS 8 run:

    dnf groupinstall "Development Tools"

    dnf install -y epel-release

    dnf repolist -v

    dnf repository-packages epel list

<br>

<br>

<br>

# WSL Tips

You can call any Linux command directly from `cmd` or `PowerShell` by just putting it after `wsl.exe`

From `PowerShell` execute the following command to determine information of all the installed Linux distributions:

    (Get-ChildItem "HKCU:\Software\Microsoft\Windows\CurrentVersion\Lxss" -Recurse)

To obtain the location of the filesystems, you can run:

    (Get-ChildItem HKCU:\Software\Microsoft\Windows\CurrentVersion\Lxss | ForEach-Object {Get-ItemProperty $_.PSPath}) | select DistributionName,BasePath

<br>

### Swappiness

`cat /proc/sys/vm/swappiness` if its over `10`

`sudo nano /proc/sys/vm/swappiness` change it to `10`

`sudo sysctl -p` to refresh the changes

After that `cat` again to confirm that swappiness is changed.

<br>

### Bell

You can also **disable** the annoying bell sound that is triggering with tab when there're no other autosuggest options by running:

    echo 'set bell-style none' >> ~/.inputrc

<br>

### Time

Fix WSL time sync

    sudo ntpdate ntp.ubuntu.com &>/dev/null &

For fixing also UTC dual boot problem with Linux or macOSX run in a `cmd` with **administrator previleges**:

    reg add "HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation" /v RealTimeIsUniversal /d 1 /t REG_QWORD /f

<br>

# Interesting tweaks and locations

in `Run` , `CTRL+R` give:

for temporary file folders

`TEMP` and `%TEMP%`

`recent`

`mblctr.exe` Available only for laptops

`msconfig.msc` System Configuration

`services.msc` Services

`gpedit.msc` Local group policy editor

`perfmon /rel` Reliability monitor

`math input panel` Draws out mathematical equations

The location of StartUp folder

    shell:startup
    shell:common startup

To view your applications similar to Application's folder of macOSX

    shell:AppsFolder

`regedit.exe` Registry Editor

`recdisc.exe` Repair Disk Tool

`sigverif.exe` File Signature Verification

`dialer.exe` Phone Dialer

`dxdiag.exe` DirectX diagnostics tool

`MdSched.exe` Memory Diagnostics Tool

`mmc.exe` Microsoft management console, to create consoles

`MRT.exe` Malicious software removal tool

`msra.exe` Windows remote assistant tool

`wsreset.exe` Cleaning Microsoft Store

`recent` Recent items

`shrpubw.exe` Share creation wizard

`prefetch` For deleting prefetch files

`Reliability Monitor` Best monitoring tool to log Windows performance

`shell:::{80F3F1D5-FECA-45F3-BC32-752C152E456E}` Tablet PC Settings

<br>

**Folder Options**

    Open file explorer to [This PC]

    Display full path in the title bar

    Show hidden files

    Hide Extentions for known file types

    Launch folder windows in a seperate process

    Reveal undo and redo buttons

<br>

Taskmanager opens with `CTRL+SHIFT+ESC` if you have it from the options `always on top` it will open everytime you hit the shortcut on top other crashed windows, to manually refresh its values with `F5`.

To remove items from the right click **context menu** open `regedit.exe` and navigate to:

    HKEY_CLASSES_ROOT\*\shell
    HKEY_CLASSES_ROOT\*\shellex
    HKEY_CLASSES_ROOT\Directory\shell\
    HKEY_CLASSES_ROOT\*\shellex\ContextMenuHandlers\
    HKEY_CLASSES_ROOT\Directory\Background\shell
    HKEY_CLASSES_ROOT\Directory\Background\shellex\ContextMenuHandlers

<br>

To **disable** explorer suggestions on search go to `regedit.exe` and create a new `DWORD (32bit)` called `DisableSearchBoxSuggestions` with the value of `1`, if the folders does not exist create a new `Key` called `Explorer`.

    HKEY_CURRENT_USER\SOFTWARE\Policies\Microsoft\Windows\Explorer

Then create two more `DWORD (32bit)` called `BingSearchEnabled` and `CortanaConsent` with the value of `0`in that location:

    HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Search

<br>

in **cmd** run: `netsh wlan show wlanreport` for a wifi details and log report

in **cmd** run: `tasklist`to get a list and then to kill a task use kill command with the PID or name from the list

    taskkill /f /t /im wsl.exe

    taskkill /f /t /pid 12836

in **cmd** run: `powercfg /energy` Power effeciency diagnostic report.

also you can run `powercfg /batteryreport` if you are on a laptop.

in **cmd** run: `cipher /e C:\Folder\File.txt` to encrypt or `cipher /e C:\Folder\File.txt` to decrypt.
To encrypt a folder with its content `cipher /e /s:C:\Folder\`

<br>

**Disable Cortana**

     Get-AppxPackage -allusers Microsoft.549981C3F5F10 | Remove-AppxPackage

<br>

**Start Menu** items are in two locations:

    %APPDATA%\Microsoft\Windows\Start Menu\Programs\

    %USERPROFILE%\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\

or you can give that in `run`: `shell:Start Menu`

<br>

You can delete the content of previous updates folder.

    C:\Windows\SoftwareDistribution\Download\

<br>

Other locations might be:

    %appdata%\microsoft\windows\recent\automaticdestinations

    %appdata%\microsoft\windows\recent\customdestinations

<br>

[Sysinternals](https://download.sysinternals.com/files/SysinternalsSuite.zip)
`procexp64.exe` , Windows troubleshooting utilities

<br>

To choose a different Windows installation to be the default for the bootloader on `cmd` run:

    bcdboot E:\Windows          // as an example for drive E:

    bcdedit /default {current}

To check for problems on a disk

    chkdsk /f e:

<br>

**Hidden administrator account**

_Enable_: `net user administrator /active:yes`

_Disable_: `net user administrator /active:no`

<br>

You can bypass login prompt by finding in search `netplwiz` and open it as **administrator**

<br>

System File Checker tool to repair missing or corrupted system files
on `cmd.exe` with **administrator privileges** run:

    sfc/scannow

To repair the image of Windows, on `PowerShell` with **administrator privileges**run:

    DISM.exe /Online /Cleanup-image /Restorehealth

<br>

You can get your MAC address and other info with: `ipcofig /all`, you can clear the DNS cache by running: `ipconfig/flushdns`, also better change your DNS provider, [here is a list](https://www.privacytools.io/providers/dns/) of
private focused DNS services, i use mostly **Quad9**
IPv4: `9.9.9.9, 149.112.112.112`

With: `tracert <url of the domain>` its kinda like ping but you can trace every hop till the reaching the final destination of tha domain.
<br>

To **debloat** at a certain level Windows 10, the easiest way i found without running weird scripts, is by installing **CCleaner** and remove any unwanted system app i tend to keep only Groove music player from the native apps.

<br>

Install Microsoft Edge Enterprise

     Invoke-WebRequest -Uri http://dl.delivery.mp.microsoft.com/filestreamingservice/files/1fc0c5fe-c1f5-4879-a43c-515d9f731444/MicrosoftEdgeEnterpriseX64.msi  -OutFile MicrosoftEdge.msi -UseBasicParsing

Reduce size of `WinSxS` folder

    dism.exe /online /cleanup-image /spsuperseded /hidesp

<br>

### Scoop Package Manager

In order to install [Scoop](https://scoop.sh/) you need first to install its dependencies, PowerShell 5 or later and .NET Framework 4.5 or later.

Also you need to allow the execution of scripts if you havent already done it:

    Set-ExecutionPolicy RemoteSigned -scope CurrentUser

To install run:

    iwr -useb get.scoop.sh | iex

To add the extras bucket

    scoop bucket add extras

To list bockets run:

    scoop bucket known

<br>

**Basic Commands**

    scoop help

    scoop search <pakage>

    scoop install <pakage>

    scoop uninstall <pakage>

    scoop update *

<br>

### Winget Package Manager

Install Windows Packet Manager [Winget](https://github.com/microsoft/winget-cli/blob/master/README.md)

    Invoke-WebRequest -Uri https://github.com/microsoft/winget-cli/releases/download/v0.1.4331-preview/Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.appxbundle -OutFile Winget.appx -UseBasicParsing

    Add-AppxPackage .\Winget.appx

<br>

**Basic Commands**

    winget --version

    winget help

    winget search <package>

    winget install <package>

    winget show <package>

<br>

### God Mode

It’s simply a special folder you can `enable` that exposes most of Windows admin, management, settings, and Control Panel tools in a single, easy-to-scroll-through **one interface**.

You can **enter** God Mode by creating a folder and **renaming** it with this line:

    GodMode.{ED7BA470-8E54-465E-825C-99712043E01C}

<br>

### Local Shared Folders

We have first to `enable` Developer Mode so the computer is discoverable and can accessed to share files in your local network.

    Windows Settings > Updates & Security > For Developers

**enable** `Developer Mode` and `Device Discovery` to reveal your local networks devices, you will **not want** to have always `Device Discovery` enabled, especially if you are on a laptop and you changing local networks all the time.

    Windows Settings > Network & Internet > Ethernet Properties > Change advnanced sharing options

On `Public Profile` tab turn **off** `Network Discovery`
On `Private` tab turn **on** `File and printer sharing`
On `Guest` you turn **on** both options
On `All Networks` tab turn **off** `Password Protected Sharing`
Turn **on** `Media streaming`

To share a folder on your local network just right click `Give access to > Specific people` then you need to add everyone to the list over your Username, with the **right privileges**, to right click on folder `Properties` then `Sharing` and click `Advanced Sharing` or run share creation wizard `shrpubw.exe`.

<br>

### Hibernation

Sometime `hiberfil.sys` file can become too large **several GB**, so you will want to turn off hibernation in some point.

How to make it unavailable:

    powercfg.exe /hibernate off

How to make it back to available again:

    powercfg.exe /hibernate on

**Reboot** so changes take effect.

<br>

### HYPER-V

You can **enable** [HYPER-V](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v) from `Turn Windows Features On or Off` , or by running:

    DISM /Online /Enable-Feature /All /FeatureName:Microsoft-Hyper-V

<br>

To **disable** it run:

    Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor

<br>

### System Restore Points

System restore points are off by default so you have to _enable_ it for your main drive.

You can use _Task Scheduler_ to create a task that runs whether user is logged in or not, with **highest priviliges** every 3 or 4 days

On `General tab` click on `Change User or Group` on the object write `system` and press `Check Names`

On `Actions Tab` create a new action, put `wmic.exe` on Program/Script and paste this on `Add argument`:

    /Namespace:\\root\default Path SystemRestore Call CreateRestorePoint "Daily Restore Point", 100, 12

On `Conditions tab` uncheck the `Power` option about AC power.

On `Settings tab` check the `Run task as soon as possible`.

<br>

### Disable content indexing on SSDs

If you installed Windows 10 on an SSD, you might want to disable content indexing:

1. Go to `File Explorer > This PC`.
2. Right click on your disk then select `Properties`.
3. Under `General`, uncheck `Allow files on this drive to have contents indexed in addition to file properties`.
4. Click Apply.

<br>

### Remap default folder locations to a separate HDD

As much as possible, your SSD should only contain Windows 10 and your apps. The target locations of the default folders (`Documents`, `Downloads`, `Desktop`, etc.) can be remapped to a separate HDD.

1. Right click a default folder.
2. Go to `Properties > Location`.
3. Change the target location to the location of your HDD. For example, to move `Documents` to drive `D:`, set the target location to `D:\Documents`.
4. Click Apply.

<br>

### Image Backup & Recovery

Now that almost everything is setup the most wise practice we can follow is to do a **image backup** of the entire disk, to be prepared when things go totally wrong. 🔥🔥

<br>

run `reagentc /info` to check if Windows Recovery Environment is `enabled`

if not run `reagentc /enable`

Go to `Windows Settings > Update & Security > Backup > Backup and Restore (Windows7)`

`Create a system image` and choose a hard drive to make the image

You can recover via `Windows Settings > Update & Security > Recovery` and click `restart now`
Then go to `Troubleshoot` click `more recovery options` and then `System Image Recovery` will appear.

From `Troubleshoot` also you can **reset** your OS.

<br>

<br>

<br>

<br>

<br>

<br>

## References

<br>

https://wiki.ubuntu.com/WSL/

https://docs.microsoft.com/en-us/windows/wsl/

https://docs.microsoft.com/en-us/windows/wsl/install-win10/

https://docs.microsoft.com/en-us/windows/wsl/wsl-config/

https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi/

https://www.microsoft.com/en-us/p/ubuntu/9nblggh4msv6?activetab=pivot:overviewtab/

https://docs.microsoft.com/en-us/powershell/

https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-windows?view=powershell-7.1

https://github.com/zsh-users/antigen/

https://github.com/JanDeDobbeleer/oh-my-posh/

https://github.com/dahlbyk/posh-git/

https://www.nerdfonts.com/

https://github.com/ryanoasis/nerd-fonts/

https://github.com/ryanoasis/nerd-fonts/tree/master/patched-fonts/Meslo/

https://github.com/adam7/delugia-code/

https://github.com/tonsky/FiraCode/

https://github.com/Microsoft/Git-Credential-Manager-for-Windows/releases/tag/1.20.0/

https://scoop.sh/

https://docs.microsoft.com/en-us/windows/package-manager/winget/

https://github.com/willmcgugan/rich/

https://github.com/mbadolato/iTerm2-Color-Schemes/

https://github.com/ohmyzsh/ohmyzsh/

https://github.com/romkatv/powerlevel10k/

https://github.com/zsh-users/zsh-syntax-highlighting/

https://github.com/marlonrichert/zsh-autocomplete/

https://github.com/zsh-users/zsh-autosuggestions/

https://github.com/ChrisPenner/copy-pasta/

https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/history-substring-search/

https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v/

https://github.com/microsoft/terminal/

https://github.com/microsoft/PowerToys/

https://chocolatey.org/

https://chocolatey.org/install/

https://docs.chocolatey.org/en-us/

https://chocolatey.org/packages/

https://docs.chocolatey.org/en-us/choco/commands/

https://tutanota.com/blog/posts/desktop-clients/

https://github.com/vladimiry/ElectronMail/

https://docs.microsoft.com/en-us/sysinternals/downloads/

https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-git#git-can-be-installed-on-windows-and-on-wsl/

https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-database/

https://docs.github.com/en/free-pro-team@latest/github/using-git/ignoring-files/

https://github.com/zsh-users/zsh/blob/master/Functions/Misc/tetriscurses/

https://www.php.net/docs.php/

https://www.phpmyadmin.net/

https://getcomposer.org/

https://github.com/pyenv/pyenv/

https://github.com/nodejs/

https://github.com/npm/npm/

https://github.com/nvm-sh/nvm#install--update-script/

https://github.com/yarnpkg/yarn/

https://ubuntu.com/tutorials/install-and-configure-wordpress#1-overview/

https://deno.land/

https://deno.land/manual/getting_started/installation/

https://devdocs.prestashop.com/1.7/basics/introduction/

https://devdocs.prestashop.com/1.7/basics/installation/localhost/

https://github.com/PrestaShop/php-ps-info/releases/

https://gist.github.com/tdcosta100/385636cbae39fc8cd0937139e87b1c74/

https://www.xfce.org/

https://docs.docker.com/engine/reference/commandline/docker/

https://gist.github.com/mindplace/b4b094157d7a3be6afd2c96370d39fad/

https://github.com/ogham/exa/

https://github.com/BurntSushi/ripgrep/

https://github.com/junegunn/fzf/

https://github.com/sharkdp/bat/

https://github.com/MidnightCommander/mc/

https://github.com/sharkdp/fd/

https://github.com/wfxr/forgit/

https://github.com/jonas/tig/

https://github.com/github/hub/

https://github.com/httpie/httpie/

https://docs.docker.com/engine/reference/commandline/docker/

https://docs.docker.com/docker-for-windows/install/

https://doc.rust-lang.org/stable/

https://github.com/rust-lang/rust/

<br>

<br>

<br>

<br>

<!-- ARXI  -->

       <                                                                                  yyj ^ 2020

<!-- TELOS -->
