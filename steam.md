# Steam


## dnf-install
```sh
sudo dnf install -y https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
dnf install -y steam
```

## run
```sh
if [ -x /usr/bin/steam ]; then
    /usr/bin/steam
else
    flatpak run com.valvesoftware.Steam
fi
```

## flatpak-install
```sh
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
flatpak install --assumeyes flathub com.valvesoftware.Steam
```

## flatpak-run
```sh
flatpak run com.valvesoftware.Steam
```
