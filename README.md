This is my my WSL configuration provisionned by ansible.

![Ansible Lint](https://github.com/M0NsTeRRR/wsl-ansible/workflows/Ansible%20Lint/badge.svg)

# Requirements

- WSL2 with Ubuntu (version >= 22.04)
- Ansible (version >= 2.14)
- Windows Terminal

# Configure

# Windows
##  SSH
Enable ssh-agent on windows and share %USERPROFILE% env var to WSL2, open powershell prompt with admin right
```powershell
Get-Service ssh-agent | Set-Service -StartupType Automatic
Start-Service ssh-agent
setx WSLENV USERPROFILE/up
```
Install niperelay in `%USERPROFILE%\.wsl`   
Enable ssh agent support for OpenSSH on keepassXC  

##  GPG
Install GnuPG on Windows  
Import GPG keys  
Get GPG exe path `wsl wslpath $(where.exe gpg)`  
Edit `vars.yml` if needed

## Windows terminal
[Download MesloLGS fonts](https://github.com/romkatv/dotfiles-public/tree/master/.local/share/fonts/NerdFonts) and install them

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
[automount]
enabled = true
mountFsTab = false
root = /mnt/
options = "metadata,umask=22,fmask=11"

[network]
generateHosts = true
generateResolvConf = true
```

Create `~/.ssh` folder for `lortega` user  
Add all private and public keys to the folder  
Add `lortega` to sudoers  

#  Run
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

# Credits

Copyright © Ludovic Ortega, 2020

Contributor(s):

-Ortega Ludovic - ludovic.ortega@adminafk.fr
