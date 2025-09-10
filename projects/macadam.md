# Compile Macadam


### info

Definition to compile the [Macadam](https://github.com/crc-org/macadam) library.


### vars
This action defines variables that will be used in all the actions

```sh
APPNAME="projects/macadam"
PROJHOME="~/Projects"
MCDREPO="https://github.com/crc-org/macadam"
MCDSOURCE="${PROJHOME}/crc-org/macadam"
MCDLOCAL=$(eval echo "${MCDSOURCE}")
MCDDEVENV="gofedora"
```

### shared

---

Local source interaction.

### exists-source
```sh
[ -d ${MCDLOCAL} ]
```

### remove-source
```sh
rm -rf ${MCDLOCAL}
```

### reset-source
```sh
cd ${MCDLOCAL}
git reset --hard
cd -
```

### checkout-source
```sh
mkdir -p ${MCDLOCAL}
git clone ${MCDREPO} ${MCDLOCAL}
```

### cp
This action copies the macadam binary to the user's local bin folder.
```sh
cp ${MCDLOCAL}/bin/macadam-linux-amd64 ${LOCALBIN}/macadam
```

### rm
```sh
rm -f ${LOCALBIN}/macadam
```

### cd
This action changes to the local source directory.

```sh interactive
if ! apps ${APPNAME} source exists; then
  echo "Run: 'apps ${APPNAME} source checkout' first."
  return
fi
cd ${MCDLOCAL}
```

### code
```sh interactive
if ! apps ${APPNAME} source exists; then
  echo "Run: 'apps ${APPNAME} source checkout' first."
  return
fi
code ${MCDLOCAL}
```

---

These are actions to manage the `devenv` container that is used.

### remove-devenv
```sh
devenv ${MCDDEVENV} remove
```

### start-devenv
```sh
devenv ${MCDDEVENV} noinit
```

### stop-devenv
```sh
devenv ${MCDDEVENV} stop
```

### exists-devenv
```sh
devenv ${MCDDEVENV} exists
```

---

The compilation actions will be performed inside a `devenv`-container.

### make
```sh interactive
devenv ${MCDDEVENV} usercmd "cd ${MCDSOURCE} && make"
```

### cross-make
```sh interactive
devenv ${MCDDEVENV} usercmd "cd ${MCDSOURCE} && make cross"
```

### clean-make
```sh interactive
devenv ${MCDDEVENV} usercmd "cd ${MCDSOURCE} && make clean"
```

### local-make
Temporary solution to run make locally on the host

```sh interactive
cd ${MCDLOCAL} && make
```

---

Default action is to compile. Performs the following:

  - checkout source if not exists
  - start `devenv` if not exists
  - `make clean`
  - `make cross`

### default alias compile
```sh interactive
if ! apps ${APPNAME} source exists; then
  apps ${APPNAME} source checkout
fi
if ! apps ${APPNAME} devenv exists; then
  apps ${APPNAME} devenv start
fi
apps ${APPNAME} make clean
apps ${APPNAME} make
```

