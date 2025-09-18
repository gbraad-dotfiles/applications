# copyparty

### info

  - https://www.youtube.com/watch?v=15_-hgsX2V0
  - https://github.com//9001/copyparty


### vars
```sh
APPNAME=copyparty
APPTITLE=copyparty
SVCNAME=dotfiles-apps-${APPNAME}
```

### config

```ini
[copyparty]
    HOST="localhost"
    PORT=3923
    VOLUME="/media/${USER}"
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

### restart-service
```sh
app ${APPNAME} service stop
app ${APPNAME} service start
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
```sh
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
tailscale serve --bg --tcp ${COPYPARTY_PORT} ${COPYPARTY_PORT}
```

---

### default alias run run-service
```sh evaluate
if ! app copyparty check; then
  app copyparty install
fi

COPYPARTYARGS=( "-i" "${COPYPARTY_HOST}" "-p" "${COPYPARTY_PORT}" )
if [ -d "${COPYPARTY_VOLUME}" ]; then
  COPYPARTYARGS+=( "-v" "${COPYPARTY_VOLUME}::rw" )
fi

SHAREFOLDERS=$(dotini folders --get folders.shares)
echo "${SHAREFOLDERS}" | tr ',' '\n' | while IFS= read -r folder; do
  if [ -d "${HOME}/${folder}" ]; then
    COPYPARTYARGS+=( "-v" "${HOME}/${folder}:/${folder}:rw" )
  fi
done

python -m copyparty ${COPYPARTYARGS[@]}
```
