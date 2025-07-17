# Code - tunnel

## info

  - https://github.com/gbraad-vscode/code-systemd


## vars
```sh
APPNAME=code-tunnel
APPTITLE="Code tunnel"
SVCNAME=dotfiles-apps-${APPNAME}
```

---

## install-service
```sh
if ! apps code check cli; then
  apps code install cli
fi

apps-export-service ${APPNAME} ${APPTITLE}
```

## enable-service
```sh
systemctl --user enable --now ${SVCNAME}
```

## disable-service
```sh
systemctl --user disable --now ${SVCNAME}
```

## start-service
```sh
systemctl --user start ${SVCNAME}
```

## stop-service
```sh
systemctl --user stop ${SVCNAME}
```

## restart-service
```sh
systemctl --user restart ${SVCNAME}
```

## status-service
```sh
systemctl --user status ${SVCNAME}
```

## active-service
```sh
systemctl --user is-active ${SVCNAME}
```

---

## user-install
```sh
mkdir -p ~/.config/systemd/user/
curl -fsSL  https://raw.githubusercontent.com/gbraad-vscode/code-systemd/refs/heads/main/user/code-tunnel.service   \
  -o ~/.config/systemd/user/code-tunnel.service
systemctl --user daemon-reload
#systemctl --user enable --now code-tunnel
```

## system-install
```sh
sudo curl -fsSL https://raw.githubusercontent.com/gbraad-vscode/code-systemd/refs/heads/main/system/code-tunnel%40.service   \
  -o /etc/systemd/system/code-tunnel@.service
sudo systemctl daemon-reload
#sudo systemctl enable --now code-tunnel@${USER}
```

## default run run-service
```sh interactive
if [ -z "${HOSTNAME}" ]; then
    echo "HOSTNAME not set"
    return 1
fi

code tunnel --accept-server-license-terms --name ${HOSTNAME}
```

## alias screen
```sh
screen code tunnel
```

