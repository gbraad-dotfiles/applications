# Obsidian

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

## install
```sh
apps obsidian install appimage
${APPSHOME}/Obsidian.AppImage --appimage-extract
mv squashfs-root ${APPSHOME}/Obsidian
apps obsidian remove appimage
```

## run
```sh
if [ -x "${APPSHOME}/Obsidian/obsidian" ]; then
    "${APPSHOME}/Obsidian/obsidian" --no-sandbox
elif [ -x "${APPSHOME}/Obsidian.AppImage" ]; then
    "${APPSHOME}/Obsidian.AppImage"
else
    echo "Obsidian is not installed in ${APPSHOME}."
fi

