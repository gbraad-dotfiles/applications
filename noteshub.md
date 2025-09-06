# NotesHub


## vars
```sh
APPNAME=NoteHub
NHVERSION=3.8.6
```

## flatpak-install
```sh
curl -sL https://github.com/NotesHubApp/noteshub-releases/releases/download/V${NHVERSION}/io.atom.electron.NotesHub_stable_x86_64.flatpak -o /tmp/noteshub.flatpak
flatpak install --user --assumeyes /tmp/noteshub.flatpak
rm -f /tmp/noteshub.flatpak
```

## flatpak-run
```sh
flatpak run io.atom.electron.NotesHub 
```

## flatpak-check
```sh
flatpak info io.atom.electron.NotesHub > /dev/null
```

## appimage-remove
```sh
rm -rf ${APPSHOME}/${APPNAME}.AppImage
```

## appimage-check
```sh
[ -x "${APPSHOME}/${APPNAME}.AppImage" ]
```

## appimage-install
```sh
curl -L https://github.com/NotesHubApp/noteshub-releases/releases/download/V${NHVERSION}/NotesHub-${NHVERSION}-x64.AppImage -o ${APPSHOME}/${APPNAME}.AppImage
chmod +x ${APPSHOME}/${APPNAME}.AppImage
```

## appimage-run
```sh background
${APPSHOME}/${APPNAME}.AppImage
```

## user-install
```sh
apps noteshub install appimage
${APPSHOME}/${APPNAME}.AppImage --appimage-extract
mv squashfs-root ${APPSHOME}/${APPNAME}
apps noteshub remove appimage
```

## user-remove
```sh
rm -rf ${APPSHOME}/${APPNAME}
```

## user-check
```sh
[ -x "${APPSHOME}/${APPNAME}/AppRun" ]
```

## user-run
```sh
"${APPSHOME}/${APPNAME}/AppRun"
```

## web-run
```sh
xdg-open https://www.noteshub.app/notebooks
```

## alias default run run-desktop
```sh background
if apps noteshub check user; then
    apps noteshub run user
elif apps noteshub check appimage; then
    apps noteshub run appimage
elif apps noteshub check flatpak; then
    apps noteshub run flatpak
else
    echo "${APPNAME} is not installed in ${APPSHOME}."
fi

