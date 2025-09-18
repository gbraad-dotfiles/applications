# LibreOffice

### info


### vars
```sh
cmd=/usr/bin/libreoffice
```

### dnf-install
```
sudo dnf install -y libreoffice
```

### flatpak-install
```sh
flatpak install --user --assumeyes flathub org.libreoffice.LibreOffice
```

### flatpak-run
```sh
flatpak run org.libreoffice.LibreOffice
```

### flatpak-check
```sh
flatpak info org.libreoffice.LibreOffice > /dev/null
```

### appimage-install
```sh
curl -sL https://appimages.libreitalia.org/LibreOffice-still.standard-x86_64.AppImage -o ${APPSHOME}/${APPNAME}.AppImage
chmod +x ${APPSHOME}/${APPNAME}.AppImage
```

### appimage-remove
```sh
rm -f ${APPSHOME}/${APPNAME}.AppImage
```

### appimage-run
```sh
${APPSHOME}/${APPNAME}.AppImage
```

### appimage-check
```sh
[ -x ${APPSHOME}/${APPNAME}.AppImage ]
```

### user-install
```sh
app libreoffice install appimage
${APPSHOME}/${APPNAME}.AppImage --appimage-extract
mv squashfs-root ${APPSHOME}/${APPNAME}
app libreoffice remove appimage
```

### user-remove
```sh
rm -rf ${APPSHOME}/${APPNAME}
```

### user-check
```sh
[ -x ${APPSHOME}/${APPNAME}/AppRun ]
```

### user-run
```sh
${APPSHOME}/${APPNAME}/AppRun
```

### check
```sh
[ -x $cmd ]
```

### alias default run run-desktop
```sh
if app libreoffice check; then
    $cmd
elif app libreoffice check user; then
    app libreoffice run user
elif app libreoffice check appimage; then
    app libreoffice run appimage
elif app libreoffice check flatpak; then
    app libreoffice run flatpak
else
    echo "LibreOffice not installed"
fi
```

