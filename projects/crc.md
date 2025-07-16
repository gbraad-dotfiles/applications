# Compile CRC

## info

Alias to compile [CRC](https://github.com/crc-org/crc) for `make cross` from `~/Projects/crc-org/crc`.


## vars
```sh
APPNAME="projects/crc"
PROJHOME="~/Projects"
CRCSOURCE="${PROJHOME}/crc-org/crc"
CRCLOCAL=$(eval echo "${CRCSOURCE}")
CRCDEVENV="gofedora"
```

---

Local source interaction

## exists-source
```sh
[ -d ${CRCLOCAL} ]
```

## remove-source
```sh
rm -rf ${CRCLOCAL}
```

## reset-source
```sh
cd ${CRCLOCAL}
git reset --hard
cd -
```

## checkout-source
```sh
mkdir -p ${CRCLOCAL}
git clone https://github.com/crc-org/crc ${CRCLOCAL}
```

---

## remove-devenv
```sh
devenv ${CRCDEVENV} remove
```

## start-devenv
```sh
devenv ${CRCDEVENV} noinit
```

## stop-devenv
```sh
devenv ${CRCDEVENV} stop
```

## exists-devenv
```sh
devenv ${CRCDEVENV} exists
```

---

## make
```sh interactive
devenv ${CRCDEVENV} usercmd "cd ${CRCSOURCE} && make"
```

## cross-make
```sh interactive
devenv ${CRCDEVENV} usercmd "cd ${CRCSOURCE} && make cross"
```

## clean-make
```sh
devenv ${CRCDEVENV} usercmd "cd ${CRCSOURCE} && make clean"
```

---

## default alias run
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

