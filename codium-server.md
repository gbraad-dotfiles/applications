# Codium server

### info
Installs Codium server


  - https://github.com/gbraad-actions/codium-server-action
  - https://github.com/gbraad-vscode/code-systemd/

### vars
```sh
SVCNAME=dotfiles-apps-${APPNAME}
```

### config
```ini
[codium]
    path="${HOME}/.codium"
    host="0.0.0.0"
```

---

### install-service
```sh
if ! app ${APPNAME} check; then
  app ${APPNAME} install
fi

apps-service-install ${APPNAME} ${APPTITLE}
```

### enable-service
```sh
systemctl --user enable --now ${SVCNAME}
```

### disable-service
```sh
systemctl --user disable --now ${SVCNAME}
```

### start-service
```sh
systemctl --user start ${SVCNAME}
```

### stop-service
```sh
systemctl --user stop ${SVCNAME}
```

### restart-service
```sh
systemctl --user restart ${SVCNAME}
```

### status-service
```sh
systemctl --user status ${SVCNAME}
```

### active-service
```sh
systemctl --user is-active ${SVCNAME}
```

### journal-service
```sh
journalctl --user -u ${SVCNAME} -f
```

---

### shared
```sh
_installcodiumserverdownload() {
    local install_location="$1" install_dir
    local download_url response version temp_file 
    local ARCH SUFFIX API_URL

    ARCH=$(uname -m)
    if [[ "$ARCH" == "aarch64" ]]; then
        SUFFIX="arm64"
    elif [[ "$ARCH" == "x86_64" ]]; then
        SUFFIX="x64"
    else
        echo "Unsupported architecture: $ARCH"
        exit 1
    fi
 
    API_URL="https://api.github.com/repos/VSCodium/vscodium/releases/latest"
    response=$(curl -s "$API_URL")
    download_url=$(echo "$response" | grep -oP "(?<=\"browser_download_url\": \")[^\"]*vscodium-reh-web-linux-${SUFFIX}[^\"]*\.tar\.gz(?!.*sha)")
    version=$(echo "$download_url" | grep -oP '(?<=/download/)[^/]+')

    if [[ -z "$download_url" ]]; then
        echo "Download URL not found"
        return
    fi

    temp_file=$(mktemp)
    curl -L -o "$temp_file" "$download_url"
  
    install_dir="$install_location/$version"
    mkdir -p "$install_dir"
    tar -xzf "$temp_file" -C "$install_dir"
    ln -sfn "$install_dir" "$install_location/latest"
    rm "$temp_file"
}
```

### check
```sh
[ -x "${CODIUM_PATH}/latest/bin/codium-server" ]
```

### install
```sh
_installcodiumserverdownload ${CODIUM_PATH}
ln -sfn "${CODIUM_PATH}/latest/bin/codium-server" "${LOCALBIN}/codium-server"

# install extensions
${LOCALBIN}/codium-server --install-extension gbraad.extensionpack
```

### remove
```sh
rm -rf ${CODIUM_PATH}
```

### alias run run-service
```sh
${CODIUM_PATH}/latest/bin/codium-server --without-connection-token --host ${CODIUM_HOST}
```

### screen
```
screen app codium-server run
```

---

> [!NOTE]
> These are here for archival.

#### install
```sh
if is_root; then
  app codium-server install system
else
  app codium-server install user
fi
```

#### system-install
```sh
install_location=/opt/codium
_installcodiumserverdownload $install_location
sudo ln -sfn "$install_location/latest/bin/codium-server" "/usr/bin/codium-server"
```

#### user-service-install
```sh
mkdir -p ~/.config/systemd/user/
curl -fsSL https://raw.githubusercontent.com/gbraad-vscode/code-systemd/refs/heads/main/codium-user/codium-server.service \
  -o ~/.config/systemd/user/codium-server.service
systemctl --user daemon-reload
systemctl --user enable --now codium-server
```

#### system-service-install
```sh
curl -fsSL https://raw.githubusercontent.com/gbraad-vscode/code-systemd/refs/heads/main/codium-system/codium-server%40.service \
  -o /etc/systemd/system/codium-server@.service
sudo systemctl daemon-reload
sudo systemctl enable --now codium-server@${USER}
```

