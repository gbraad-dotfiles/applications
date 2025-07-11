# LibreOffice

## info


## vars
```sh
cmd=/usr/bin/libreoffice
APPNAME=libreoffice
```

## dnf-install
```
sudo dnf install -y libreoffice
```

## flatpak-install
```sh
flatpak install --user --assumeyes flathub org.libreoffice.LibreOffice
```

## flatpak-run
```sh
flatpak run org.libreoffice.LibreOffice
```

## flatpak-check
```sh
flatpak info org.libreoffice.LibreOffice > /dev/null
```

## appimage-install
```sh
curl -sL https://appimages.libreitalia.org/LibreOffice-still.standard-x86_64.AppImage -o ${APPSHOME}/${APPNAME}.AppImage
chmod +x ${APPSHOME}/${APPNAME}.AppImage
```

## appimage-remove
```sh
rm -f ${APPSHOME}/${APPNAME}.AppImage
```

## appimage-run
```sh
${APPSHOME}/${APPNAME}.AppImage
```

## appimage-check
```sh
[ -x ${APPSHOME}/${APPNAME}.AppImage ]
```

## user-install
```sh
apps libreoffice install appimage
${APPSHOME}/${APPNAME}.AppImage --appimage-extract
mv squashfs-root ${APPSHOME}/${APPNAME}
apps libreoffice remove appimage
```

## user-remove
```sh
rm -rf ${APPSHOME}/${APPNAME}
```

## user-check
```sh
[ -x ${APPSHOME}/${APPNAME}/AppRun ]
```

## user-run
```sh
${APPSHOME}/${APPNAME}/AppRun
```

## check
```sh
[ -x $cmd ]
```

## default run
```sh
if apps libreoffice check; then
    $cmd
elif apps libreoffice check user; then
    apps libreoffice run user
elif apps libreoffice check appimage; then
    apps libreoffice run appimage
elif apps libreoffice check flatpak; then
    apps libreoffice run flatpak
else
    echo "LibreOffice not installed"
fi
```

