# Itch

### info

### vars
```
APPNAME="itch"
ITCHVERSION=1.27.0
ITCHURL="https://broth.itch.zone/itch-setup/linux-amd64/${ITCHVERSION}/archive/default"
ITCHHOME=${HOME}/.itch
ITCHBIN=${ITCHHOME}/itch
```

### user-install
```sh
curl -L -o /tmp/itch-setup.zip ${ITCHURL}
unzip /tmp/itch-setup.zip -d /tmp
/tmp/itch-setup
```

### default alias run-desktop
```sh
if app itch check user; then
    app itch run user
elif app itch check flatpak; then
    app itch run flatpak
else
    echo "${APPNAME} is not installed."
fi
```

### user-run
```sh
${ITCHBIN}
```

### user-check
```sh
[ -x ${ITCHBIN} ]
```

### flatpak-install
```sh
flatpak install -y flathub io.itch.itch
```

### flatpak-run
```sh
flatpak run io.itch.itch
```

### flatpak-check
```sh
flatpak info io.itch.itch > /dev/null
```

