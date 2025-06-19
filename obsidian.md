# Obsidian

## info
Obsidian is a powerful knowledge base that works on top of a local folder of plain text Markdown files.

- Homepage: [https://obsidian.md](https://obsidian.md)
- GitHub: [obsidianmd/obsidian-releases](https://github.com/obsidianmd/obsidian-releases)
- AppImage: Downloads from latest GitHub release

## appimage-install
```sh
latest=$(curl -s https://api.github.com/repos/obsidianmd/obsidian-releases/releases/latest | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/')
url="https://github.com/obsidianmd/obsidian-releases/releases/download/${latest}/Obsidian-${latest#v}.AppImage"
curl -L "$url" -o ${APPSHOME}/Obsidian.AppImage
chmod +x ${APPSHOME}/Obsidian.AppImage
```

## appimage-remove
```sh
rm -rf ${APPSHOME}/Obsidian.AppImage
```

## appimage-run
```sh
${APPSHOME}/Obsidian.AppImage
```

## user-install
```sh
apps obsidian install appimage
${APPSHOME}/Obsidian.AppImage --appimage-extract
mv squashfs-root ${APPSHOME}/Obsidian
apps obsidian remove appimage
```

## user-remove
```sh
rm -rf ${APPSHOME}/Obsidian
```

## flatpak-install
```sh
flatpak install --assumeyes flathub md.obsidian.Obsidian
```

## flatpak-run
```sh
flatpak run md.obsidian.Obsidian
```

## flatpak-check
```sh
flatpak info md.obsidian.Obsidian
```

## run
```sh
if [ -x "${APPSHOME}/Obsidian/obsidian" ]; then
    "${APPSHOME}/Obsidian/obsidian" --no-sandbox
elif [ -x "${APPSHOME}/Obsidian.AppImage" ]; then
    "${APPSHOME}/Obsidian.AppImage"
elif apps obsidian check flatpak; then
    apps obsidian run flatpak
else
    echo "Obsidian is not installed in ${APPSHOME}."
fi

