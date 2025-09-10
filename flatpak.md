# flatpak

### dnf-install
```sh
sudo dnf -y install flatpak
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

### check
```sh
[ -x /usr/bin/flatpak ]
```

