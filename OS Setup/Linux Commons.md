# Linux Common Stuff

- `fastfetch` for system information.
- `nvidia-smi` to get gpu vram usage and more.
- `journalctl -b -1 -k` to get system logs from crashes.
- `systemd-analyze` to get boot time in short.
- `systemd-analyze blame` to get boot time of all services.

## How to install stuff

- There seems to be simple executable programs such as **Godot** and **Blender** that launches just like an exe in all linux distros. (Why not go this route for all?)
- There is **AppImage** which is a new portal file that can simply be launched. (Use **Gear Level** in KDE to manage appimages). It brings all it's dependencies.
- There is **FlatPak** which all run in a separate container. This allows to dodge bugs in a distro that can just use a container based in different distro to execute fine. Launch seems to be slow. This is popular and there is also **FlatHub**.
- Each distro / or distro it is based on uses their own repositories for installing apps such as `snap`, `ostree`, `apt-get` etc. Containerization can allow a distro to use others.
- `homebrew` seems to be viable option but it has less apps than Mac. (Ghostty available through brew in mac but not in linux). Linux brew seems to advertize "CLI based apps".

## PATH and env var

Path and environment variables can be set by editing `.bashrc`, `.bash_profile`, `/etc/profile/*.sh` 
#todo more details

It seems any sh file added in profile automatically runs (given apt permissions) and available system wide.

For containers through `toolbx`, there is a container specific profile which doesn't affect the host system.