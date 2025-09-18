# Compile gvproxy


### info

Definition to compile the [gvproxy](https://github.com/containers/gvisor-tap-vsock) binary.


### vars
This action defines variables that will be used in all the actions

```sh
APPNAME="projects/gvproxy"
PROJHOME="~/Projects"
GVPREPO="https://github.com/containers/gvisor-tap-vsock"
GVPSOURCE="${PROJHOME}/containers/gvisor-tap-vsock"
GVPLOCAL=$(eval echo "${GVPSOURCE}")
GVPDEVENV="gofedora"
```

### shared

---

Local source interaction.

### exists-source
```sh
[ -d ${GVPLOCAL} ]
```

### remove-source
```sh
rm -rf ${GVPLOCAL}
```

### reset-source
```sh
cd ${GVPLOCAL}
git reset --hard
cd -
```

### checkout-source
```sh
mkdir -p ${GVPLOCAL}
git clone ${GVPREPO} ${GVPLOCAL}
```

### cp
This action installs the gvproxy binary into the podman machine expected location

```sh
cp ${GVPLOCAL}/bin/gvproxy ${LOCALBIN}/gvproxy
```

### rm
```sh
rm -f ${LOCALBIN}/gvproxy
```

### cd
This action changes to the local source directory.

```sh
if ! app ${APPNAME} source exists; then
  echo "Run: 'app ${APPNAME} source checkout' first."
  return
fi
cd ${GVPLOCAL}
```

### code
```sh
if ! app ${APPNAME} source exists; then
  echo "Run: 'app ${APPNAME} source checkout' first."
  return
fi
code ${GVPLOCAL}
```

---

These are actions to manage the `devenv` container that is used.

### remove-devenv
```sh
devenv ${GVPDEVENV} remove
```

### start-devenv
```sh
devenv ${GVPDEVENV} noinit
```

### stop-devenv
```sh
devenv ${GVPDEVENV} stop
```

### exists-devenv
```sh
devenv ${GVPDEVENV} exists
```

---

The compilation actions will be performed inside a `devenv`-container.

### make
```sh
devenv ${GVPDEVENV} usercmd "cd ${GVPSOURCE} && make"
```

### cross-make
```sh
devenv ${GVPDEVENV} usercmd "cd ${GVPSOURCE} && make cross"
```

### clean-make
```sh
devenv ${GVPDEVENV} usercmd "cd ${GVPSOURCE} && make clean"
```

### local-make
Temporary solution to run make locally on the host

```sh
cd ${GVPLOCAL} && make
```

---

Default action is to compile. Performs the following:

  - checkout source if not exists
  - start `devenv` if not exists
  - `make clean`
  - `make cross`

### default alias compile
```sh
if ! app ${APPNAME} source exists; then
  app ${APPNAME} source checkout
fi
if ! app ${APPNAME} devenv exists; then
  app ${APPNAME} devenv start
fi
app ${APPNAME} make clean
app ${APPNAME} make
```

