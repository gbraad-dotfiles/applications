# Pinger

### info


### vars
```sh
APPNAME=pinger
APPTITLE="Pinger"
SVCNAME=dotfiles-apps-${APPNAME}
```

### config
```ini
[pinger]
   HOST="google.com"
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
```sh
journalctl --user -u ${SVCNAME} -f
```

### default alias run run-service
```sh evaluate
ping ${PINGER_HOST}
```

### screen
```sh
screen app pinger
```
