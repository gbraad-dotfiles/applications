# Vivaldi

## info
**Vivaldi** is a free, cross-platform web browser developed by Vivaldi Technologies. It is known for its rich customization options, advanced tab management, built-in productivity tools (such as notes, mail, and calendar), and a strong focus on user privacy. Vivaldi is built on Chromium, allowing compatibility with Chrome extensions, and is available for Linux, Windows, macOS, and Android.

- Homepage: [https://vivaldi.com](https://vivaldi.com)
- Download: [https://vivaldi.com/download/](https://vivaldi.com/download/)
- Documentation: [https://help.vivaldi.com/](https://help.vivaldi.com/)
- Source Code: [https://vivaldi.com/source/](https://vivaldi.com/source/)
- Key Features:
  - Highly customizable UI and shortcuts
  - Tab stacking, tiling, and advanced tab management
  - Integrated email client, calendar, and feed reader
  - Notes manager and screen capture
  - Built-in ad blocker and tracking protection
  - Sync across devices with end-to-end encryption
  - Support for Chrome extensions


## run
```sh
vivaldi
```

## dnf-install
```sh
echo -e "[vivaldi]\nname=vivaldi\nbaseurl=https://repo.vivaldi.com/archive/rpm/x86_64\nenabled=1\ngpgcheck=1\ngpgkey=https://repo.vivaldi.com/archive/linux_signing_key.pub" | sudo tee /etc/yum.repos.d/vivaldi.repo > /dev/null
sudo dnf install -y vivaldi-stable
```

## dnf-remove
```sh
sudo dnf remove -y vivaldi-stable
```

## flatpak-install
```sh
flatpak install flathub com.vivaldi.Vivaldi
```

## flatpak-run
```sh
flatpak run com.vivaldi.Vivaldi
```

## flatpak-remove
```sh
flatpak uninstall com.Vivaldi.vivaldi
rm -rf ~/.var/app/com.vivaldi.Vivaldi/config/vivaldi
```

