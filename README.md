This is my my WSL configuration provisionned by ansible.

![Ansible Lint](https://github.com/M0NsTeRRR/wsl-ansible/workflows/Ansible%20Lint/badge.svg)

# Requirements

- WSL2 with Ubuntu (version >= 22.04)
- Python3 and Pip
- Windows Terminal

# Configure

##  SSH
Install SSH client from winget ` winget install "openssh beta"` (it can causes error using old version)  
Enable ssh-agent on windows and share UserProfile and ProgramFiles env var to WSL2, open powershell prompt with admin right
```powershell
Get-Service ssh-agent | Set-Service -StartupType Automatic
Start-Service ssh-agent
setx WSLENV 'ProgramFiles/up:USERPROFILE/up'
```
Install niperelay on windows in `%USERPROFILE%\.wsl`  
Enable ssh agent support for OpenSSH on keepassXC  

##  GPG  
Install GnuPG on Windows in `%PROGRAMFILES%`  
Import GPG keys  
Share %PROGRAMFILES% env var (in SSH step)  

## Windows terminal
[Download MesloLGS fonts](https://github.com/romkatv/dotfiles-public/tree/master/.local/share/fonts/NerdFonts) and install them

Set `terminal.integrated.fontFamily` to `MesloLGS NF` in VSCode to display font icon in terminal  

Change `defaultProfile` with Ubuntu GUID  

Edit ubuntu profile and replace with this  
Don't forget to replace `<GUID>`and `<background_image>`  
```
{
    "guid": "{<GUID>}",
    "hidden": false,
    "name": "Ubuntu",
    "source": "Windows.Terminal.Wsl",
    "opacity" : 75,
    "backgroundImage": "<background_image>",
    "backgroundImageOpacity": 0.2,
    "closeOnExit" : true,
    "colorScheme" : "One Half Dark",
    "cursorColor" : "#FFFFFF",
    "cursorShape" : "bar",
    "font": {
        "face": "MesloLGS NF",
        "size": 13
    },
    "historySize" : 9001,
    "icon" : "ms-appx:///ProfileIcons/{9acb9455-ca41-5af7-950f-6bca1bc9722f}.png",
    "padding" : "0, 0, 0, 0",
    "snapOnInput" : true,
    "useAcrylic" : false
}
```

Replace the scheme with this  
```
{
    "background" : "#0C0C0C",
    "black" : "#0C0C0C",
    "blue" : "#1170b5",
    "brightBlack" : "#5A6374",
    "brightBlue" : "#61AFEF",
    "brightCyan" : "#56B6C2",
    "brightGreen" : "#98C379",
    "brightPurple" : "#C678DD",
    "brightRed" : "#E06C75",
    "brightWhite" : "#DCDFE4",
    "brightYellow" : "#E5C07B",
    "cyan" : "#56B6C2",
    "foreground" : "#DCDFE4",
    "green" : "#98C379",
    "name" : "One Half Dark",
    "purple" : "#C678DD",
    "red" : "#E06C75",
    "white" : "#DCDFE4",
    "yellow" : "#E5C07B"
}
```

Add keybindings
```
{ "command": { "action": "splitPane", "split": "down" }, "keys": "alt+shift+numpad_plus" },
{ "command": { "action": "splitPane", "split": "right" }, "keys": "alt+shift+numpad_minus" },
{ "command": { "action": "splitPane", "split": "auto" }, "keys": "alt+shift+|" },
{ "command": { "action": "moveFocus", "direction": "down" }, "keys": "alt+down" },
{ "command": { "action": "moveFocus", "direction": "left" }, "keys": "alt+left" },
{ "command": { "action": "moveFocus", "direction": "right" }, "keys": "alt+right" },
{ "command": { "action": "moveFocus", "direction": "up" }, "keys": "alt+up" },
{ "command": "closePane", "keys": "alt+numpad_minus" },
{ "command": "find", "keys": "ctrl+f" },
{ "command": {"action": "copy", "singleLine": false }, "keys": "ctrl+c" },
{ "command": "paste", "keys": "ctrl+v" }
```

Disable wsl bell sound in profiles -> defaults
```
{
    // Put settings here that you want to apply to all profiles.
    "bellStyle": "none"
}
```

## WSL
Create `/etc/wsl.conf`  
```
[boot]
systemd=true

[automount]
enabled = true
mountFsTab = false
root = /mnt/
options = "metadata,umask=22,fmask=11"

[network]
generateHosts = true
generateResolvConf = true
```

#  Run
Create a python environment `python3 -m venv venv`  
Source it `source venv/bin/activate`  
Install python dependencies `pip3 install -r requirements.txt`

Update `vars.yml` if needed

Put CA certificates in `files/ca`

Install ansible galaxy dependencies `ansible-galaxy install -r requirements.yml`

The playbook must be runned as `lortega` user (not root)
`ansible-playbook -i localhost, deploy.yml --ask-become-pass`  

Import GPG keys
```
gpg --import < priv.asc
gpg --import < pub.asc
gpg --import-ownertrust < trust.asc
```

# Contributing

We welcome and encourage contributions to this project! Please read the [Contributing guide](CONTRIBUTING.md). Also make sure to check the [Code of Conduct](CODE_OF_CONDUCT.md) and adhere to its guidelines

# Security

See [SECURITY.md](SECURITY.md) file for details.

# Licence

The code is under CeCILL license.

You can find all details here: https://cecill.info/licences/Licence_CeCILL_V2.1-en.html

# Credits

Copyright © Ludovic Ortega, 2020

Contributor(s):

-Ortega Ludovic - ludovic.ortega@adminafk.fr
