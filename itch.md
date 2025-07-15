# Itch

## info

## vars
```
ITCHVERSION=1.27.0
ITCHURL="https://broth.itch.zone/itch-setup/linux-amd64/${ITCHVERSION}/archive/default"
ITCHHOME=${HOME}/.itch
ITCHBIN=${ITCHHOME}/itch
```

## install
```sh interactive
curl -L -o /tmp/itch-setup.zip ${ITCHURL}
unzip /tmp/itch-setup.zip -d /tmp
/tmp/itch-setup
```

## default alias run-desktop
```sh
if ! apps itch check; then
  apps itch install
else
  ${ITCHBIN}
fi
```

## check
```sh
[ -x ${ITCHBIN} ]
```

## flatpak-install
```sh
flatpak install -y flathub io.itch.itch
```

