# Compile CRC

## info

Alias to compile [CRC][crc project] for `make cross` from `~/Projects/crc-org/crc`.


## vars
```sh
APPNAME="makecrc"
PROJHOME="${HOME}/Projects/"
CRCSOURCE="${PROJHOME}/crc-org/crc"
CRCDEVENV="gofedora"
```

---

## exists-source
```sh
[ -d ${CRCSOURCE} ]
```

## reset-source
```sh
cd ${CRCSOURCE}
git reset --hard
cd -
```

## checkout-source
```sh
mkdir -p ${CRCSOURCE}
git clone https://github.com/crc-org/crc ${CRCBUILD}
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

## cross-make
```sh interactive
devenv ${CRCDEVENV} usercmd "cd ${CRCSOURCE} && make clean && make cross"
```

## clean-make
```sh
devenv ${CRCRUNNER} usercmd "cd ${CRCSOURCE} && make clean"
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


[crc project]: https://github.com/crc-org/crc "CRC project"
