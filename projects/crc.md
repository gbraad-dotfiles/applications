# Compile CRC


### info

Definition to compile the [CRC](https://github.com/crc-org/crc) project.


### vars
This action defines variables that will be used in all the actions

```sh
APPNAME="projects/crc"
PROJHOME="~/Projects"
CRCSOURCE="${PROJHOME}/crc-org/crc"
CRCLOCAL=$(eval echo "${CRCSOURCE}")
CRCDEVENV="gofedora"
```

### shared

---

Local source interaction.

### exists-source
```sh
[ -d ${CRCLOCAL} ]
```

### remove-source
```sh
rm -rf ${CRCLOCAL}
```

### reset-source
```sh
cd ${CRCLOCAL}
git reset --hard
cd -
```

### checkout-source
```sh
mkdir -p ${CRCLOCAL}
git clone https://github.com/crc-org/crc ${CRCLOCAL}
```

### cd
This action changes to the local source directory.

```sh interactive
if ! apps ${APPNAME} source exists; then
  echo "Run: 'apps ${APPNAME} source checkout' first."
  return
fi
cd ${CRCLOCAL}
```

### code
```sh interactive
if ! apps ${APPNAME} source exists; then
  echo "Run: 'apps ${APPNAME} source checkout' first."
  return
fi
code ${CRCLOCAL}
```

---

These are actions to manage the `devenv` container that is used.

### remove-devenv
```sh
devenv ${CRCDEVENV} remove
```

### start-devenv
```sh
devenv ${CRCDEVENV} noinit
```

### stop-devenv
```sh
devenv ${CRCDEVENV} stop
```

### exists-devenv
```sh
devenv ${CRCDEVENV} exists
```

---

The compilation actions will be performed inside a `devenv`-container.

### make
```sh interactive
devenv ${CRCDEVENV} usercmd "cd ${CRCSOURCE} && make"
```

### cross-make
```sh interactive
devenv ${CRCDEVENV} usercmd "cd ${CRCSOURCE} && make cross"
```

### clean-make
```sh interactive
devenv ${CRCDEVENV} usercmd "cd ${CRCSOURCE} && make clean"
```

### local-make
Temporary solution to run make locally on the host

```sh interactive
cd ${CRCLOCAL} && make
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
apps ${APPNAME} make cross
```

