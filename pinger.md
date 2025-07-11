# Pinger

## info


## install-service
```sh
apps-export-service pinger "Pinger"
```

## enable-service
```sh
systemctl --user enable --now dotfiles-apps-pinger
```

## disable-service
```sh
systemctl --user disable --now dotfiles-apps-pinger
```

## start-service
```sh
systemctl --user start dotfiles-apps-pinger
```

## stop-service
```sh
systemctl --user stop dotfiles-apps-pinger
```

## status-service
```sh
systemctl --user status dotfiles-apps-pinger
```

## active-service
```sh
systemctl --user is-active dotfiles-apps-pinger
```

## default run run-service run-desktop
```sh interactive
ping $(dotini pinger pinger.host)
```
