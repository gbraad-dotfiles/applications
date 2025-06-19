# Obsidian

## install
```sh
latest=$(curl -s https://api.github.com/repos/obsidianmd/obsidian-releases/releases/latest | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/')
url="https://github.com/obsidianmd/obsidian-releases/releases/download/${latest}/Obsidian-${latest#v}.AppImage"
curl -L "$url" -o ${APPSHOME}/Obsidian.AppImage
chmod +x ${APPSHOME}/Obsidian.AppImage
```

## remove
```sh
rm -rf ${APPSHOME}/Obsidian.AppImage
```

## run
```sh
${APPSHOME}/Obsidian.AppImage
```
