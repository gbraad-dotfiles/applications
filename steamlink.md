# Steam Link


## flatpak-install
```sh
flatpak install --user --assumeyes flathub com.valvesoftware.SteamLink
```

## flatpak-run
```sh
flatpak run com.valvesoftware.SteamLink
```

## run
```sh
apps steamlink run flatpak
```

## rpi-install
```sh
curl -#Of https://media.steampowered.com/steamlink/rpi/latest/steamlink.deb
sudo dpkg -i steamlink.deb
```

## rpi-run
```sh
steamlink
```
