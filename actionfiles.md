# Actionfiles

### vars
```sh
ACTREPO="https://github.com/gbraad-dotfiles/actionfiles"
ACTPATH="${HOME}/Projects/gbraad-dotfiles/actionfiles"
```

### exists
```sh
[ -d ${ACTPATH} ]
```

### cd
```sh evaluate
cd ${ACTPATH}
```

### checkout
```sh
git clone ${ACTREPO} ${ACTPATH}
```

### update
```sh
cd ${ACTPATH}
git pull
```

### `reset runner`
```sh
run ${ACTPATH}/runner.md reset permissions
```

### `compile crc`
```sh
run ${ACTPATH}/projects/crc.md compile
```

### `compile macadam`
```sh
run ${ACTPATH}/projects/macadam.md compile
```

### `compile gvproxy`
```sh
run ${ACTPATH}/projects/gvproxy.md compile
```

### default run alias
```sh
if ! app ${APPNAME} exists; then
  app ${APPNAME} checkout
fi
app ${APPNAME} aliases
```

### aliases
```sh evaluate
alias dotfedora="run ${ACTPATH}/machine/dotfedora.md"
```
