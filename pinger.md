# Pinger

## info


## vars
```sh
APPNAME=pinger
APPTITLE="Pinger"
SVCNAME=dotfiles-apps-${APPNAME}
PINGERHOST=$(dotini pinger pinger.host)
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

## journal-service
```sh interactive
journalctl --user -u ${SVCNAME} -f
```

## default alias run run-service
```sh interactive
ping ${PINGERHOST}
```

## screen
```sh interactive
screen apps pinger
```
