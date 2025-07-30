# copyparty

## info

  - https://www.youtube.com/watch?v=15_-hgsX2V0
  - https://github.com//9001/copyparty


## vars
```sh
APPNAME=copyparty
APPTITLE=copyparty
SVCNAME=dotfiles-apps-${APPNAME}
COPYPARTYIP=`tailscale ip -4`
COPYPARTYPORT=3923
COPYPARTYVOLUME=/media/${USER}
```

## install-service
```sh
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

## restart-service
```sh
apps ${APPNAME} service stop
apps ${APPNAME} service start
```

## start-service
```sh
systemctl --user start ${SVCNAME}
```

## stop-service
```sh
systemctl --user stop ${SVCNAME}
```

## status-service
```sh
systemctl --user status ${SVCNAME}
```

## active-service
```sh
systemctl --user is-active ${SVCNAME}
```

## journal-service
```sh interactive
journalctl --user -u ${SVCNAME} -f
```

---

## pip-install
```sh
python3 -m pip install --user -U copyparty --break-system-packages
```

---

## default alias run run-service
```sh interactive
python -m copyparty -i ${COPYPARTYIP} -p ${COPYPARTYPORT} -v ${COPYPARTYVOLUME}::rw
```
