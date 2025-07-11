# MAME

## info

## flatpak-install
```sh
flatpak install flathub org.mamedev.MAME
```

## flatpak-run
```sh
flatpak run org.mamedev.MAME
```

## flatpak-check
```sh
flatpak info org.mamedev.MAME > /dev/null
```

## dnf-install
```sh
sudo dnf install -y mame
```

## default run
```sh
if apps mame check flatpak; then
  apps mame run flatpak
fi
```
