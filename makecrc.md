# Compile CRC

## info

## vars
```sh
PROJHOME="${HOME}/Projects/"
CRCBUILD="${PROJHOME}/crc-org/crc"
CRCRUNNER="gofedora"
```

Alias to compile CRC for `make cross` from `~/Projects/crc-org/crc`.

## user-install
```sh
mkdir -p ${CRCBUILD}
git clone https://github.com/crc-org/crc ${CRCBUILD}
```

## default alias run
```sh interactive
if ! apps makecrc check user; then
  apps makecrc install user
fi
if ! devenv ${CRCRUNNER} exists; then
  echo "Starting ${CRCRUNNER} with 'noinit'"
  devenv ${CRCRUNNER} noinit
fi
devenv ${CRCRUNNER} usercmd "cd ${CRCBUILD} && make clean && make cross"
```

## user-check
```sh
[ -d ${CRCBUILD} ]
```
