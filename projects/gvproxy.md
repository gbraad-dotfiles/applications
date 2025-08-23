# Compile gvproxy


## info

Definition to compile the [gvproxy](https://github.com/containers/gvisor-tap-vsock) binary.


## vars
This action defines variables that will be used in all the actions

```sh
APPNAME="projects/gvproxy"
PROJHOME="~/Projects"
GVPREPO="https://github.com/containers/gvisor-tap-vsock"
GVPSOURCE="${PROJHOME}/containers/gvisor-tap-vsock"
GVPLOCAL=$(eval echo "${GVPSOURCE}")
GVPDEVENV="gofedora"
```

## shared

---

Local source interaction.

## exists-source
```sh
[ -d ${GVPLOCAL} ]
```

## remove-source
```sh
rm -rf ${GVPLOCAL}
```

## reset-source
```sh
cd ${GVPLOCAL}
git reset --hard
cd -
```

## checkout-source
```sh
mkdir -p ${GVPLOCAL}
git clone ${GVPREPO} ${GVPLOCAL}
```

## cp
This action installs the gvproxy binary into the podman machine expected location

```sh
cp ${GVPLOCAL}/bin/gvproxy ${LOCALBIN}/gvproxy
```

## rm
```sh
rm -f ${LOCALBIN}/gvproxy
```

## cd
This action changes to the local source directory.

```sh interactive
if ! apps ${APPNAME} source exists; then
  echo "Run: 'apps ${APPNAME} source checkout' first."
  return
fi
cd ${GVPLOCAL}
```

## code
```sh interactive
if ! apps ${APPNAME} source exists; then
  echo "Run: 'apps ${APPNAME} source checkout' first."
  return
fi
code ${GVPLOCAL}
```

---

These are actions to manage the `devenv` container that is used.

## remove-devenv
```sh
devenv ${GVPDEVENV} remove
```

## start-devenv
```sh
devenv ${GVPDEVENV} noinit
```

## stop-devenv
```sh
devenv ${GVPDEVENV} stop
```

## exists-devenv
```sh
devenv ${GVPDEVENV} exists
```

---

The compilation actions will be performed inside a `devenv`-container.

## make
```sh interactive
devenv ${GVPDEVENV} usercmd "cd ${GVPSOURCE} && make"
```

## cross-make
```sh interactive
devenv ${GVPDEVENV} usercmd "cd ${GVPSOURCE} && make cross"
```

## clean-make
```sh interactive
devenv ${GVPDEVENV} usercmd "cd ${GVPSOURCE} && make clean"
```

## local-make
Temporary solution to run make locally on the host

```sh interactive
cd ${GVPLOCAL} && make
```

---

Default action is to compile. Performs the following:

  - checkout source if not exists
  - start `devenv` if not exists
  - `make clean`
  - `make cross`

## default alias compile
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

