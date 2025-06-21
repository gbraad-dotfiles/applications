# Workstation

## info

## fedora-install
```sh
sudo dnf install -y \
    https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm \
    https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm

sudo dnf group install -y \
    base-graphical \
    container-management \
    core \
    fonts \
    gnome-desktop \
    guest-desktop-agents \
    hardware-support \
    multimedia \
    networkmanager-submodules \
    printing \
    virtualization \
    workstation-product

sudo systemctl set-default graphical.target

# See https://fedoraproject.org/wiki/Changes/UnprivilegedUpdatesAtomicDesktops:
#     Avoid annoying popups when logged in.
sudo dnf install -y fedora-release-ostree-desktop
```

