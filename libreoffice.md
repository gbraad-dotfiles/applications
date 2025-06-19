# LibreOffice

## info


## dnf-install
```
sudo dnf install -y libreoffice
```

## flatpak-install
```sh
flatpak install flathub org.libreoffice.LibreOffice
```

## flatpak-run
```sh
flatpak run org.libreoffice.LibreOffice
```

## run
```sh
if [ -x /usr/bin/libreoffice ]; then
    /usr/bin/libreoffice
else
    apps libreoffice run flatpak
fi
```

