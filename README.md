# qmenu
Utilizes [dmenu](https://tools.suckless.org/dmenu/) to provide the user with a
drop down menu for [QubesOS](https://qubes-os.org/), 
from which they can quickly administer their qube
preferences, firewall rules, applications, devices, etc. with only the keyboard.

## qmenu-vm
List, manage and configure your qubes.

Selecting the top row, instead of any qube, will refresh the list.
    
    Usage: qmenu-vm [OPTION] (--light-theme)
    
    --all
    --tags=TAG  
    --running
    --paused
    --halted

## qmenu-dv
List and manage your connected devices.

    Usage: qmenu-dv [OPTION] (--light-theme)

    --all
    --audio-input
    --block
    --usb
