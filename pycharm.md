# PyCharm

### info

### vars
```sh
# https://download-cdn.jetbrains.com/python/pycharm-2025.1.2-aarch64.tar.gz
URL="https://download.jetbrains.com/python/pycharm-2025.1.2.tar.gz"
INSTALL_DIR="${APPSHOME}/pycharm"
```

### install
```sh
mkdir -p "$INSTALL_DIR"
curl -L "$URL" -o /tmp/pycharm.tar.gz
tar --strip-components=1 -xzf /tmp/pycharm.tar.gz -C "$INSTALL_DIR"
rm /tmp/pycharm.tar.gz
```

### default run run-desktop
```sh
${APPSHOME}/pycharm/bin/pycharm.sh"
```

