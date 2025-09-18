# Fedora dotfiles (virual machine)

## 

### info

### vars
```sh
APPNAME=dotfedora
SVCNAME=dotfiles-machine-${APPNAME}
```

---

### install-service
```sh
machine-export-service ${APPNAME} 
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

### run-machine (not used)
```sh
machine gofedora service
```

---

### download
```sh
machine gofedora download
```

### create
```sh
machine gofedora create
```

### exists
```sh
machine gofedora exists
```

### start
```sh
machine gofedora start
```

### stop
```sh
machine gofedora stop
```

### rm remove
```sh
machine gofedora rm
```

### alias
```sh
app machine/dotfedora
```

### console ssh
```sh
machine dotfedora ssh
```
