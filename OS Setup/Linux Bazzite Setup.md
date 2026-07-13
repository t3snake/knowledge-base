# Linux Bazzite Setup

*For linux specific knowledge see* [[Linux Commons]]

[Bazzite.gg](https://bazzite.gg)

Bazzite and uses KDE Plasma (GNOME option also available) like Steam Deck, except it is based on fedora instead of arch linux.
With my hardware, everything ran perfectly without manually adding any drivers.
In fact there are additional benefits, I did not even know were possible through software such as adjusting brightness of both my monitors similar to laptops.

Bazzite is an immutable system, lot of system files are read-only.
It means it is hard to screw up the system.
But it also means we need to use toolbx (podman b/h the scene) to install system stuff like CUDA.
It does in the container also use the host system's drivers, I/O etc.

Bazzite updates daily in background and applies updates on the next reboot.
That seems to be the cause of 2 minute boot time.
Disable the timer for updates by running the following command.

```
systemctl disable uupd.timer
```

## Install CUDA

Since Bazzite is immutable, installing CUDA requires extra steps.

Here is a guide from llama.cpp to install CUDA: 
[CUDA Install in Fedora Immutable systems](https://github.com/ggml-org/llama.cpp/blob/master/docs/backend/CUDA-FEDORA.md)