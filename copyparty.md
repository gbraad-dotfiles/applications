# copyparty

### info

  - https://www.youtube.com/watch?v=15_-hgsX2V0
  - https://github.com//9001/copyparty


### vars
```sh
APPNAME=copyparty
APPTITLE=copyparty
SVCNAME=dotfiles-apps-${APPNAME}
COPYPARTYHOST=localhost
COPYPARTYPORT=3923
COPYPARTYVOLUME=/media/${USER}
```

### install-service
```sh
apps-export-service ${APPNAME} ${APPTITLE}
```

### enable-service
```sh
systemctl --user enable --now ${SVCNAME}
```

### disable-service
```sh
systemctl --user disable --now ${SVCNAME}
```

### restart-service
```sh
apps ${APPNAME} service stop
apps ${APPNAME} service start
```

### start-service
```sh
systemctl --user start ${SVCNAME}
```

### stop-service
```sh
systemctl --user stop ${SVCNAME}
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

---

### install pip-install
```sh
python3 -m pip install --user -U copyparty --break-system-packages
```

### check
```sh
python -c "import copyparty" 2>/dev/null
```

### expose
```sh
tailscale serve --bg --tcp ${COPYPARTYPORT} ${COPYPARTYPORT}
```

---

### default alias run run-service
```sh interactive
if ! apps copyparty check; then
  apps copyparty install
fi

if [ ! -d "${COPYPARTYVOLUME}" ]; then
  sudo mkdir -p "${COPYPARTYVOLUME}"
  sudo chown "${USER}:${USER}" "$COPYPARTYVOLUME"
fi

python -m copyparty -i ${COPYPARTYHOST} -p ${COPYPARTYPORT} \
  -v ${COPYPARTYVOLUME}::rw \
  -v ${HOME}/Documents/:/Documents:rw -v ${HOME}/Downloads:/Downloads:rw -v ${HOME}/Projects:/Projects:rw
```
