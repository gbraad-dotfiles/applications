# Vivaldi

## info
**Vivaldi** is a free, cross-platform web browser developed by Vivaldi Technologies. It is known for its rich customization options, advanced tab management, built-in productivity tools (such as notes, mail, and calendar), and a strong focus on user privacy. Vivaldi is built on Chromium, allowing compatibility with Chrome extensions, and is available for Linux, Windows, macOS, and Android.

- Homepage: [https://vivaldi.com](https://vivaldi.com)
- Download: [https://vivaldi.com/download/](https://vivaldi.com/download/)
- Documentation: [https://help.vivaldi.com/](https://help.vivaldi.com/)
- Source Code: [https://vivaldi.com/source/](https://vivaldi.com/source/)
- Key Features:
  - Highly customizable UI and shortcuts
  - Tab stacking, tiling, and advanced tab management
  - Integrated email client, calendar, and feed reader
  - Notes manager and screen capture
  - Built-in ad blocker and tracking protection
  - Sync across devices with end-to-end encryption
  - Support for Chrome extensions


## vars
```sh
cmd=/usr/bin/vivaldi
APPNAME=Vivaldi
```

## check
```sh
[ -x $cmd ]
```

## run
```sh
if apps vivaldi check; then
    $cmd
elif apps vivaldi check flatpak; then
    apps vivaldi run flatpak
fi
```

## dnf-install
```sh
sudo dnf config-manager addrepo --from-repofile=https://repo.vivaldi.com/archive/vivaldi-fedora.repo
sudo dnf install -y vivaldi-stable
```

## dnf-remove
```sh
sudo dnf remove -y vivaldi-stable
```

## flatpak-install
```sh
flatpak install --user --assumeyes flathub com.vivaldi.Vivaldi
```

## flatpak-run
```sh
flatpak run com.vivaldi.Vivaldi
```

## flatpak-check
```sh
flatpak info com.vivaldi.Vivaldi > /dev/null
```

## flatpak-remove
```sh
flatpak uninstall com.Vivaldi.vivaldi
rm -rf ~/.var/app/com.vivaldi.Vivaldi/config/vivaldi
```

## appimage-install
Unofficial releases

```sh
BASE_URL="https://github.com/ivan-hc/Vivaldi-appimage/releases/download/continuous"
FILENAME=$(curl -sL "https://github.com/ivan-hc/Vivaldi-appimage/releases/expanded_assets/continuous" \
  | grep -oP 'Vivaldi-stable-[^"]+x86_64\.AppImage' | head -n 1)

if [ -z "$FILENAME" ]; then
  echo "Could not find the latest Vivaldi AppImage."
  exit 1
fi
curl -L "$BASE_URL/$FILENAME" -o ${APPSHOME}/${APPNAME}.AppImage

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
apps vivaldi install appimage
${APPSHOME}/${APPNAME}.AppImage --appimage-extract
mv squashfs-root ${APPSHOME}/${APPNAME}
apps vivaldi remove appimage
```

## user-remove
```sh
rm -rf ${APPSHOME}/${APPNAME}
```

## user-check
```sh
[ -x "${APPSHOME}/${APPNAME}/vivaldi" ]
```

## user-run
```sh
${APPSHOME}/${APPNAME}/vivaldi
```
