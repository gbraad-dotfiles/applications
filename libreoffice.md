# LibreOffice

## info


## vars
```sh
cmd="/usr/bin/office"
```


## dnf-install
```
sudo dnf install -y libreoffice
```

## flatpak-install
```sh
flatpak install --assumeyes flathub org.libreoffice.LibreOffice
```

## flatpak-run
```sh
flatpak run org.libreoffice.LibreOffice
```

## flatpak-check
```sh
flatpak info org.libreoffice.LibreOffice > /dev/null
```

## check
```sh
[ -x $cmd ]
```

## run
```sh
if apps libreoffice check; then
    $cmd
elif apps libreoffice check flatpak; then
    apps libreoffice run flatpak
else
    echo "LibreOffice not installed"
fi
```

