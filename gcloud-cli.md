# Google Cloud

### vars
```sh
GCTGZ=google-cloud-cli-linux-x86_64.tar.gz
GCURL=https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/${GCTGZ}
GCSDK=${APPSHOME}/google-cloud-sdk/
```

### install
```sh evaluate
curl -fsSL ${GCURL} -o ${HOME}/Downloads/${GCTGZ}
tar -xf ${HOME}/Downloads/${GCTGZ} -C ${APPSHOME}
${GCSDK}/install.sh
rm -f ${HOME}/Downloads/${GCTGZ}

app ${APPNAME} env install
```

### install-env
```sh
echo "command -v app >/dev/null && app ${APPNAME} env" > ~/.zshrc.d/app-${APPNAME}.zsh
```

### env
```sh evaluate
if [ -d "${GCSDK}" ]; then
 . ${GCSDK}/path.zsh.inc
 . ${GCSDK}/completion.zsh.inc
fi
```

### login
```sh evaluate
gcloud auth application-default login
```
