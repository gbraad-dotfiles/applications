# Jupyter

### info

### vars
```sh
APPNAME="jupyter-notebook"
APPTITLE="Jupyter notebook"
JUPYTERHOST=0.0.0.0
SVCNAME=dotfiles-apps-${APPNAME}
```

### install-service
```sh
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
```sh interactive
journalctl --user -u ${SVCNAME} -f
```

### pip-install install
```sh
pip install --user notebook
```

### default alias run run-service
```sh
jupyter notebook --ip ${JUPYTERHOST} --IdentityProvider.token=''
```

### screen
```sh
screen apps jupyter-notebook run
```

