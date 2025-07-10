# Obsidian

## info
Obsidian is a powerful knowledge base that works on top of a local folder of plain text Markdown files.

- Homepage: [https://obsidian.md](https://obsidian.md)
- GitHub: [obsidianmd/obsidian-releases](https://github.com/obsidianmd/obsidian-releases)


## shared
```sh
APPNAME="Obsidian"
```

## appimage-install
```sh
latest=$(curl -s https://api.github.com/repos/obsidianmd/obsidian-releases/releases/latest | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/')
url="https://github.com/obsidianmd/obsidian-releases/releases/download/${latest}/Obsidian-${latest#v}.AppImage"
curl -L "$url" -o ${APPSHOME}/${APPNAME}.AppImage
chmod +x ${APPSHOME}/${APPNAME}.AppImage
```

## appimage-remove
```sh
rm -rf ${APPSHOME}/${APPNAME}.AppImage
```

## appimage-check
```sh
[ -x "${APPSHOME}/${APPNAME}.AppImage" ]
```

## appimage-run
```sh
${APPSHOME}/${APPNAME}.AppImage
```

## user-install
```sh
apps obsidian install appimage
${APPSHOME}/${APPNAME}.AppImage --appimage-extract
mv squashfs-root ${APPSHOME}/${APPNAME}
apps obsidian remove appimage
```

## user-remove
```sh
rm -rf ${APPSHOME}/${APPNAME}
```

## user-check
```sh
[ -x "${APPSHOME}/${APPNAME}/obsidian" ]
```

## user-run
```sh
"${APPSHOME}/${APPNAME}/obsidian" --no-sandbox
```

## flatpak-install
```sh
flatpak install --user --assumeyes flathub md.obsidian.Obsidian
```

## flatpak-run
```sh
flatpak run md.obsidian.Obsidian
```

## flatpak-check
```sh
flatpak info md.obsidian.Obsidian > /dev/null
```

## podman-install
```sh
podman run -d --name obsidian --cap-add=NET_ADMIN --cap-add=NET_RAW --device=/dev/net/tun --device=/dev/fuse ghcr.io/gbraad-apps/obsidian:latest
podman exec obsidian tailscale up -qr
ip=`podman exec obsidian tailscale ip -4`
echo "Open Obsidian at https://${ip}:8444"
```

## run
```sh background
if apps obsidian check user; then
    apps obsidian run user
elif apps obsidian check appimage; then
    apps obsidian run appimage
elif apps obsidian check flatpak; then
    apps obsidian run flatpak
else
    echo "${APPNAME} is not installed in ${APPSHOME}."
fi

