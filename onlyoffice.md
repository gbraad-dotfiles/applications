# OnlyOffice desktop editors

## info

## vars
```sh
APPNAME="OnlyOffice"
```

## rpm-install
```sh
#https://github.com/ONLYOFFICE/DesktopEditors/releases/latest/download/onlyoffice-desktopeditors.x86_64.rpm
sudo dnf install https://download.onlyoffice.com/repo/centos/main/noarch/onlyoffice-repo.noarch.rpm
sudo dnf install onlyoffice-desktopeditors -y
```

## rpm-run
```sh
desktopeditors
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
