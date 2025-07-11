# Codium server

## info
Installs Codium server


## default install
```sh
if is_root; then
  apps codium-server install system
else
  apps codium-server install user
fi
```

## shared
```sh
_installcodiumserverdownload() {
    local install_location="$1"
  
    API_URL="https://api.github.com/repos/VSCodium/vscodium/releases/latest"
    response=$(curl -s "$API_URL")
    download_url=$(echo "$response" | grep -oP '(?<="browser_download_url": ")[^"]*vscodium-reh-web-linux-x64[^"]*.tar.gz(?!.*sha)')
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

## user-install
```sh
install_location=${HOME}/.codium
_installcodiumserverdownload $install_location
ln -sfn "$install_location/latest/bin/codium-server" "${LOCALBIN}/codium-server"

mkdir -p ~/.config/systemd/user/
curl -fsSL https://raw.githubusercontent.com/gbraad-vscode/code-systemd/refs/heads/main/codium-user/codium-server.service \
  -o ~/.config/systemd/user/codium-server.service
systemctl --user daemon-reload
systemctl --user enable --now codium-server
```

## system-install
```sh
install_location=/opt/codium
_installcodiumserverdownload $install_location
sudo ln -sfn "$install_location/latest/bin/codium-server" "/usr/bin/codium-server"

curl -fsSL https://raw.githubusercontent.com/gbraad-vscode/code-systemd/refs/heads/main/codium-system/codium-server%40.service \
  -o /etc/systemd/system/codium-server@.service
sudo systemctl daemon-reload
sudo systemctl enable --now codium-server@${USER}
```
