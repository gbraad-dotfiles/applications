# Actionfiles

### vars
```sh
ACTREPO="https://github.com/gbraad-dotfiles/actionfiles"
ACTPATH="${HOME}/Projects/actionfiles"
```

### `source checkout`
```sh
git clone ${ACTREPO} ${HOME}/Projects/actionfiles
```

### `source update`
```sh
cd ${ACTPATH}
git pull
```

### `reset permissions`
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

