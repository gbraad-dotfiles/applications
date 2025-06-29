# OnlyOffice desktop editors

## info

## vars
```sh
APPNAME="OnlyOffice"
```

## centos-install fedora-install
```sh
#https://github.com/ONLYOFFICE/DesktopEditors/releases/latest/download/onlyoffice-desktopeditors.x86_64.rpm
sudo dnf install https://download.onlyoffice.com/repo/centos/main/noarch/onlyoffice-repo.noarch.rpm
sudo dnf install onlyoffice-desktopeditors -y
```

## centos-run fedora-run
```sh
desktopeditors
```

## centos-remove fedora-remove
```sh
sudo dnf remove onlyoffice-desktopeditors
```

## debian-install ubuntu-install
```sh
mkdir -p -m 700 ~/.gnupg
gpg --no-default-keyring --keyring gnupg-ring:/tmp/onlyoffice.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys CB2DE8E5
chmod 644 /tmp/onlyoffice.gpg
sudo chown root:root /tmp/onlyoffice.gpg
sudo mv /tmp/onlyoffice.gpg /usr/share/keyrings/onlyoffice.gpg

echo 'deb [signed-by=/usr/share/keyrings/onlyoffice.gpg] https://download.onlyoffice.com/repo/debian squeeze main' | sudo tee -a /etc/apt/sources.list.d/onlyoffice.list

sudo apt-get update
sudo apt-get install onlyoffice-desktopeditors
```

## debian-run ubuntu-run
```sh
desktopeditors
```

## debian-remove ubuntu-remove
```sh
sudo apt-get remove onlyoffice-desktopeditors
```

## user-install
```sh
apps onlyoffice install appimage
${APPSHOME}/${APPNAME}.AppImage --appimage-extract
mv squashfs-root ${APPSHOME}/${APPNAME}
apps onlyoffice remove appimage
```

## user-check
```sh
[ -x "${APPSHOME}/${APPNAME}/AppRun" ]
```

## user-run
```sh
${APPSHOME}/${APPNAME}/AppRun
```

## flatpak-install
```sh
flatpak install flathub org.onlyoffice.desktopeditors
```

## flatpak-run
```sh
flatpak run org.onlyoffice.desktopeditors
```

## flatpak-check
```sh
flatpak info org.onlyoffice.desktopeditors > /dev/null
```

## appimage-install
```sh
latest=$(curl -s https://api.github.com/repos/ONLYOFFICE/appimage-desktopeditors/releases/latest | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/')
url="https://github.com/ONLYOFFICE/appimage-desktopeditors/releases/download/${latest}/DesktopEditors-x86_64.AppImage"
curl -L "$url" -o ${APPSHOME}/${APPNAME}.AppImage
chmod +x ${APPSHOME}/${APPNAME}.AppImage
```

## appimage-run
```sh
${APPSHOME}/${APPNAME}.AppImage
```

## appimage-remove
```
rm -f ${APPSHOME}/${APPNAME}.AppImage
```

## podman-install
```sh
podman run -d --name onlyoffice --cap-add=NET_ADMIN --cap-add=NET_RAW --device=/dev/net/tun --device=/dev/fuse ghcr.io/gbraad-apps/onlyoffice:latest
podman exec onlyoffice tailscale up -qr
ip=`podman exec onlyoffice tailscale ip -4`
echo "Open OnlyOffice at https://${ip}:8444"
```

## run
```sh
if apps onlyoffice check user; then
    apps onlyoffice run user
elif apps onlyoffice check appimage; then
    apps onlyoffice run appimage
elif apps onlyoffice check flatpak; then
    apps onlyooffice run flatpak
else
    echo "${APPNAME} is not installed in ${APPSHOME}."
fi
```
