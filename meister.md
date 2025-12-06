# Regroove Meister

### info
MIDI Controller Suite for Live/DJ Performance

- Homepage: [https://music.gbraad.nl/meister](https://music.gbraad.nl/meister)
- GitHub: [gbraad-music/meister](https://github.com/gbraad-music/meister)


### appimage-install
```sh
latest=$(curl -s https://api.github.com/repos/gbraad-music/meister/releases/latest | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/')
url="https://github.com/gbraad-music/meister/releases/download/${latest}/Regroove-Meister-Portable.AppImage"
curl -L "$url" -o ${APPSHOME}/${APPNAME}.AppImage
chmod +x ${APPSHOME}/${APPNAME}.AppImage
```

### appimage-remove
```sh
rm -rf ${APPSHOME}/${APPNAME}.AppImage
```

### appimage-check
```sh
[ -x "${APPSHOME}/${APPNAME}.AppImage" ]
```

### appimage-run
```sh
${APPSHOME}/${APPNAME}.AppImage
```

### user-install
```sh
app ${APPNAME} install appimage
${APPSHOME}/${APPNAME}.AppImage --appimage-extract
mv squashfs-root ${APPSHOME}/${APPNAME}
app ${APPNAME} remove appimage
```

### user-remove
```sh
rm -rf ${APPSHOME}/${APPNAME}
```

### user-check
```sh
[ -x "${APPSHOME}/${APPNAME}/meister" ]
```

### user-run
```sh
"${APPSHOME}/${APPNAME}/meister" --no-sandbox
```

### alias default run run-desktop
```sh background
if app ${APPNAME} check user; then
    app ${APPNAME} run user
elif app ${APPNAME} check appimage; then
    app ${APPNAME} run appimage
else
    echo "${APPNAME} is not installed in ${APPSHOME}."
fi

