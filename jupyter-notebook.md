# Jupyter

## info

## vars
```sh
APPNAME="jupyter-notebook"
APPTITLE="Jupyter notebook"
JUPYTERHOST=0.0.0.0
SVCNAME=dotfiles-apps-${APPNAME}
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

## pip-install install
```sh
pip install --user notebook
```

## default alias run run-service
```sh
jupyter notebook --ip ${JUPYTERHOST} --IdentityProvider.token=''
```

## screen
```sh
screen apps jupyter-notebook run
```

