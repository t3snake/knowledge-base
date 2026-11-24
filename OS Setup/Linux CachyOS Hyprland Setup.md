# Linux CachyOS Hyprland Setup

*For linux specific knowledge see* [[Linux Commons]]

[CachyOS main site](https://cachyos.org/)

[CachyOS wiki](https://wiki.cachyos.org/)

[Hyprland wiki](https://wiki.hypr.land/)

I installed CachyOS with Hyprland in dualboot mode on a single partition.
Had to manually create partitions: 4mb Fat32 boot and then the root btrfs.

The setup has Hyprland preinstalled which provides the window management layer.
The rest such as title bar and header is provided by Noctalia.
This can do title bar, bottom dock with tons of customization.

`ddcutil` is preinstalled for monitors but needs to be enabled in Noctalia Settings `Super + Z`.

Use CachyOS package manager for arch connected packages.
Includes cachyos optimized packages.
Gear Level not needed since CachyOs  Package installer and Shelly takes care of flathub and appimages.
CachyOS Package Installer supporting flathub and appimages is very new (Nov 26), so guide still mentions Shelly but I can exclusively use CachyOS package manager for everything.
