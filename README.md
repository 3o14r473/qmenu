qmenu
=====

Collection of tools that utilize
[dmenu](https://tools.suckless.org/dmenu/), a dynamic menu for X,
to provide the user with a drop down menu for [QubesOS](https://qubes-os.org) administration.

With these tools, the user is able to list, start and stop their qubes, attach and detach connected devices,
manage qube preferences, firewall rules and tags, switch per-qube keyboard layouts, take notes and screenshots, launch applications and more, very quickly with only the keyboard.

## Usage

By default, dmenu is completely controlled by the keyboard. Items are selected using the arrow keys, Page Up, Page Down, Home, End and multiple shortcuts like Ctrl-p (up) and Ctrl-n (down).

Return selects the highlighted item, Ctrl+Return will let you select multiple items and Shift+Return will select the user input, instead of the highlighted item.

Consult the [manpage](https://manpages.debian.org/unstable/suckless-tools/dmenu.1.en.html#USAGE) of dmenu for more information.

### qmenu-am

Launch domU and dom0 applications.

    Usage: qmenu-am [OPTION]

     --all              List all applications
     --focused          List every application available to the qube of the focused window

### qmenu-dm

List and manage your connected devices.

    Usage: qmenu-dm [OPTION]

     --all              List all devices, select a running qube to attach/detach
     --focused          List all devices, attach/detach to/from the qube of the focused window

### qmenu-vm

List, manage and configure your qubes.

    Usage: qmenu-vm [OPTION]

     --all              List all qubes
     --focused          Manage the qube of the focused window
     --halted           List halted qubes
     --paused           List paused qubes
     --running          List running qubes
     --qube=[QUBE]      Manage the given qube
     --tags=[TAG]       List all qubes marked with the given tag

### Screenshots

Screenshots taken via qmenu-vm are placed in `$HOME/Pictures/`, inside the relevant domU qube.

## Configuration and appearance

### Label color

Whenever relevant, the "selected background" color of dmenu will be matched to a qube label.
The color for each label can be defined by creating a text file called `qmenu.conf` in `$HOME/.config/` inside dom0 with the following contents:

~~~
[LABEL 1]=#[RRGGBB]
[LABEL 2]=#[RRGGBB]
...
[LABEL 8]=#[RRGGBB]
~~~

<details>
 <summary>Try this example:</summary>

~~~
purple=#9a009a
blue=#4363d8
gray=#bebebe
green=#3cb44b
yellow=#ffe119
orange=#f58231
red=#e6194b
black=#000000
~~~

</details>

Empty `qmenu.conf` entries let dmenu display the "selected background" as black.

### dmenu

Everything else, like display options, font size and other colors, is handled by dmenu.
Therefore, extensive customization can be achieved by [modifying](https://tools/suckless.org/dmenu/patches/) or replacing dmenu with something else like [rofi](https://github.com/davatorium/rofi).

### Qube Settings

Executing the Qube Settings application will launch the Qube Manager
qube settings. This can be changed to launch qmenu-vm instead,
which makes it possible to use qmenu-am to quickly launch into the qmenu-vm
settings for a particular qube, without first listing a set of qubes or using
the `--focused` option.

Simply create an executable file called `qubes-vm-settings` in `/usr/local/bin/` inside
dom0 with the following contents: `qmenu-vm --qube="$1"`

## Dependencies

All qmenu tools are written in POSIX-compliant shell script and solely
depend on the POSIX-compliant variants of the (nonqubes-)utilities they use.

dmenu and X11-tools can be replaced and symlinked.

<details>
 <summary>qmenu-am</summary>

* cut
* dmenu
* grep
* _xdotool_ (For '--focused' option)
* _xprop_ (For '--focused' option)
* <details>
   <summary>Qubes tools</summary>

  * _qvm-prefs_ (For '--focused' option)
  * qvm-run
  </details>

</details>

<details>
 <summary>qmenu-dm</summary>

* awk
* cut
* dmenu
* grep
* sed
* sort
* wc
* _xdotool_ (For '--focused' option)
* _xprop_ (For '--focused' option)
* <details>
   <summary>Qubes tools</summary>

  * qvm-device
  * qvm-ls
  * qvm-prefs
  </details>

</details>

<details>
 <summary>qmenu-vm</summary>

- dom0
   * awk
   * cut
   * dmenu
   * grep
   * ls
   * _notify-send_
   * sed
   * sleep
   * sort
   * wc
   * _xdotool_ (For '--focused' option)
   * _xprop_ (For '--focused' option)
   * <details>
      <summary>Qubes tools</summary>

     * qubes-log-viewer
     * qubes-prefs
     * qubes-vm-boot-from-device
     * qvm-appmenus
     * qvm-block
     * qvm-check
     * qvm-clone
     * qvm-create
     * qvm-devices
     * qvm-firewall
     * qvm-kill
     * qvm-ls
     * qvm-pause
     * qvm-pci
     * qvm-pool
     * qvm-prefs
     * qvm-remove
     * qvm-run
     * qvm-service
     * qvm-shutdown
     * qvm-start
     * qvm-tags
     * qvm-unpause
     * qvm-volume
     </details>

- domU
   * _scrot_ (For taking screenshots)
   * _setxkbmap_ (For switching keyboard layouts)
</details>

## Installation

### dmenu

Clone `https://git.suckless.org/dmenu`, compile it, [copy](https://www.qubes-os.org/doc/how-to-copy-from-dom0/#copying-to-dom0) the resulting binary to dom0, place it in `/usr/local/bin/` and make it executable.

Alternatively, use the dom0 package manager to install dmenu from the repositories to quickly try it out:
    
    [@dom0 ~]# qubes-dom0-update dmenu


Keep in mind that dmenu can only be configured before compilation, so if you wish to change default colors or have it [centered](https://tools.suckless.org/dmenu/patches/center/) in the middle
of the screen and add a [border](https://tools.suckless.org/dmenu/patches/border/) (which changes color depending on qube label) to it for example, you will have to compile it yourself.

dmenu can be replaced with [qubes-rofi](https://github.com/QubesOS-contrib/qubes-rofi), among others.

### qmenu

qmenu is part of the official [contributed package repository](https://github.com/QubesOS-contrib), the [latest release](https://github.com/QubesOS-contrib/qubes-qmenu) can therefore be conviniently
[downloaded, verified and installed](https://www.qubes-os.org/doc/how-to-install-software-in-dom0/#contributed-package-repository) by the dom0 package manager:

    [@dom0 ~]# qubes-dom0-update qubes-repo-contrib

    [@dom0 ~]# qubes-dom0-update --clean qmenu


If you prefer to manually install the most recent version from the dev branch, you can follow these steps:

Warning: This repository, and therefore the dev branch, is not reviewed by the Qubes Team.


    [@dispXXXX ~]$ git clone https://github.com/3o14r473/qmenu/

    [@dom0 ~]$ qvm-run --pass-io --filter-escape-chars --no-color-output dispXXXX 'cat /home/user/qmenu/qmenu-XX' > /tmp/qmenu-XX

    [@dom0 ~]# cp /tmp/qmenu-XX /usr/local/bin/

    [@dom0 ~]# chmod 755 /usr/local/bin/qmenu-XX

For qmenu-vm, _additionally_:

    [@dom0 ~]$ mkdir /tmp/qmenu_vm

    [@dom0 ~]$ for file in $(qvm-run --pass-io --filter-escape-chars --no-color-output dispXXXX 'ls /home/user/qmenu/lib/qmenu_vm/'); do qvm-run --pass-io --filter-escape-chars --no-color-output dispXXXX "cat /home/user/qmenu/lib/qmenu_vm/$file" > /tmp/qmenu_vm/$file; done

    [@dom0 ~]# cp -r /tmp/qmenu_vm/ /lib/


# Author

* 3o14r473 - [fingerprint](E4FEE61C3B02F4CAB6D80CA7F105757D34BEFA98) [email](3o14@pm.me) [moneroj](41rMoMLvk8hEJYP2vbv3dNUGzN95CLXoANAtmAVaUxzse5KfPjhkE7d4PUwh8kCkF16FwwqfZTmS4ZKmYCjrsFAcGXTPpwH)
