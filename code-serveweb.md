# Code - serve web

### info

  - https://github.com/gbraad-vscode/code-systemd


> [!NOTE]
> Make sure to install the cli first, with:
> ```
> apps code install cli
> ```


### vars
```sh
APPNAME=code-serveweb
APPTITLE="Code serveweb"
SVCNAME=dotfiles-apps-${APPNAME}

CODEHOST=0.0.0.0
CODEPORT=8001
```

---

### install-service
```sh
if ! apps code check cli; then
  apps code install cli
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

---

### user-install
```sh
mkdir -p ~/.config/systemd/user/
curl -fsSL  https://raw.githubusercontent.com/gbraad-vscode/code-systemd/refs/heads/main/user/code-serveweb.service \
  -o ~/.config/systemd/user/code-serveweb.service
systemctl --user daemon-reload
#systemctl --user enable --now code-serveweb
```

### system-install
```sh
sudo curl -fsSL https://raw.githubusercontent.com/gbraad-vscode/code-systemd/refs/heads/main/system/code-serveweb%40.service \
  -o /etc/systemd/system/code-serveweb@.service
sudo systemctl daemon-reload
#sudo systemctl enable --now code-serveweb@${USER}
```

### default run run-service
```sh
${LOCALBIN}/code serve-web --without-connection-token --host ${CODEHOST} --port ${CODEPORT}
```

### alias screen
```sh
screen code serveweb
```

