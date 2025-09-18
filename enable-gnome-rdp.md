# Enable Remote Desktop

### run
```sh
gsettings set org.gnome.desktop.remote-desktop.rdp enable true
gsettings set org.gnome.desktop.remote-desktop.rdp screen-share-mode 'mirror-primary'

systemctl --user start gnome-remote-desktop
```
