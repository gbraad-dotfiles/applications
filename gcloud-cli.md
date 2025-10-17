# Google Cloud

### install
```sh evaluate
cd ${HOME}/Downloads
curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-cli-linux-x86_64.tar.gz
tar -xf google-cloud-cli-linux-x86_64.tar.gz -C ${APPSHOME}
${APPSHOME}/google-cloud-sdk/install.sh
```

### env
```sh evaluate
if [ -d "${APPSHOME}/google-cloud-sdk/" ]; then
 . ${APPSHOME}/google-cloud-sdk/path.zsh.inc
 . ${APPSHOME}/google-cloud-sdk/completion.zsh.inc
fi
```

### login
```sh evaluate
gcloud auth application-default login
```
