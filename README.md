This is my my WSL configuration provisionned by ansible.

# Requirements

- WSL2 with Ubuntu (version >= 20.04)
- Ansible (version >= 2.9.6)
- Windows Terminal

# Configure
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
    "acrylicOpacity" : 0.75,
    "backgroundImage": "<background_image>",
    "backgroundImageOpacity": 0.2,
    "closeOnExit" : true,
    "colorScheme" : "One Half Dark",
    "cursorColor" : "#FFFFFF",
    "cursorShape" : "bar",
    "fontFace" : "MesloLGS NF",
    "fontSize" : 13,
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

#  Run
The playbook must be runned as user (not root)  
`ansible-playbook -i localhost, deploy.yml --ask-become-pass`  

# Credits

Copyright © Ludovic Ortega, 2020

Contributor(s):

-Ortega Ludovic - mastership@hotmail.fr