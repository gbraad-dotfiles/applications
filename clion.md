# CLion

### info

### vars
```sh
# https://download-cdn.jetbrains.com/cpp/CLion-2025.1.3-aarch64.tar.gz
URL="https://download.jetbrains.com/cpp/CLion-2025.1.3.tar.gz"
INSTALL_DIR="${APPSHOME}/clion"
```

### install
```sh
mkdir -p "$INSTALL_DIR"
curl -L "$URL" -o /tmp/clion.tar.gz
tar --strip-components=1 -xzf /tmp/clion.tar.gz -C "$INSTALL_DIR"
rm /tmp/clion.tar.gz
```

### default run run-desktop
```sh
${APPSHOME}/clion/bin/clion.sh
```
