# Codium server

## info
Installs Codium server


## install
```sh
if is_root; then
  apps codium-server install system
else
  apps codium-server install user
fi
```

## user-install
```sh
_installcodiumserveruser
mkdir -p ~/.config/systemd/user/
curl -fsSL https://raw.githubusercontent.com/gbraad-vscode/code-systemd/refs/heads/main/codium-user/codium-server.service \
  -o ~/.config/systemd/user/codium-server.service
systemctl --user daemon-reload
systemctl --user enable --now codium-server
```

## system-install
```sh
_installcodiumserversystem
curl -fsSL https://raw.githubusercontent.com/gbraad-vscode/code-systemd/refs/heads/main/codium-system/codium-server%40.service \
  -o /etc/systemd/system/codium-server@.service
sudo systemctl daemon-reload
sudo systemctl enable --now codium-server@${USER}
```
