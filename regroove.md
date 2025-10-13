# Regroove 

### vars
```sh
OWNER="gbraad-modplug"
REPO="regroove"
REGROOVE_HOME=${APPSHOME}/regroove
```

### config
```ini
[regroove]
app=regroove-tui
folder=${HOME}/Music/Modules/
midi=1
```


### install
```sh
API_URL="https://api.github.com/repos/$OWNER/$REPO/releases/latest"
ASSET_URL=$(curl -s $API_URL | jq -r '.assets[0].browser_download_url')

TMPFILE=$(mktemp)

curl -fsSL "$ASSET_URL" -o "$TMPFILE"
mkdir -p "${REGROOVE_HOME}"
tar -xzf "$TMPFILE" --strip-components=1 -C "${REGROOVE_HOME}"

rm -f ${TMPFILE}
```

### check
```sh
[ -x "${REGROOVE_HOME}/${REGOOVE_APP}" ]
```

### default alias run
```sh evaluate
if app ${APPNAME} check; then
  LD_LIBRARY_PATH=${REGROOVE_HOME} ${REGROOVE_HOME}/${REGROOVE_APP} ${REGROOVE_FOLDER} -m ${REGROOVE_MIDI}
else
  echo "${APPNAME} not installed"
fi
```

### desktop-run
```sh
LD_LIBRARY_PATH=${REGROOVE_HOME} ${REGROOVE_HOME}/regroove-gui ${REGROOVE_FOLDER} -m ${REGROOVE_MIDI}
```

