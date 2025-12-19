# Emscripten SDK

> [!NOTE]
> Might need to install `atomic-static`


### install
```sh
mkdir -p ${APPSHOME}/${APPNAME}
git clone https://github.com/emscripten-core/emsdk ${APPSHOME}/${APPNAME}
cd ${APPSHOME}/${APPNAME} && ./emsdk install latest && ./emsdk activate latest
source ./emsdk_env.sh
```

### activate
```sh evaluate
source ${APPSHOME}/${APPNAME}/emsdk_env.sh
```

### check
```sh
[ -d "${APPSHOME}/${APPNAME}" ]
```

### run default alias
```sh evaluate
if app ${APPNAME} check; then
   app ${APPNAME} activate
else
   app ${APPNAME} install
fi
```
