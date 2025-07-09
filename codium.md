# Codium

## info
Installs the latest VSCodium via AppImage, RPM (dnf), DEB (apt), or user-local tarball.

## shared
```sh
APPNAME=codium
#APPSHOME="${APPSHOME:-$HOME/Applications}"
API_URL="https://api.github.com/repos/VSCodium/vscodium/releases/latest"
response=$(curl -s "$API_URL")
```

## appimage-install
```sh
download_url=$(echo "$response" | grep -oP '(?<="browser_download_url": ")[^"]*AppImage(?!.*sha)' | head -n1)
if [[ -z "$download_url" ]]; then
    echo "AppImage download URL not found"
    return 1
fi
curl -L -o ${APPSHOME}/${APPNAME}.AppImage "$download_url"
chmod +x ${APPSHOME}/${APPNAME}.AppImage
```

## appimage-remove
```sh
rm -f ${APPSHOME}/${APPNAME}.AppImage
```

## appimage-check
```sh
[ -x ${APPSHOME}/${APPNAME}.AppImage ]
```

## appimage-run
```sh
${APPSHOME}/${APPNAME}.AppImage
```

## dnf-install
```sh
arch=$(uname -m)
download_url=$(echo "$response" | grep -oP '(?<="browser_download_url": ")[^"]*\.rpm' | grep "${arch}" | head -n1)
if [[ -z "$download_url" ]]; then
    echo "RPM download URL not found"
    return 1
fi
curl -L -o /tmp/vscodium-latest.rpm "$download_url"
sudo dnf install -y /tmp/vscodium-latest.rpm
```

## apt-install
```sh
arch=$(uname -m)
case "$arch" in
    x86_64)  deb_arch="amd64" ;;
    aarch64) deb_arch="arm64" ;;
    armv7l)  deb_arch="armhf" ;;
    *)       echo "Unsupported architecture: $arch"; exit 1 ;;
esac

download_url=$(echo "$response" | grep -oP '(?<="browser_download_url": ")[^"]*\.deb' | grep "${deb_arch}" | head -n1)
if [[ -z "$download_url" ]]; then
    echo "DEB download URL for architecture $deb_arch not found"
    return 1
fi

curl -L -o /tmp/vscodium-latest.deb "$download_url"
sudo apt install -y /tmp/vscodium-latest.deb
```

## user-install
```sh
download_url=$(echo "$response" | grep -oP '(?<="browser_download_url": ")[^"]*VSCodium-linux-x64[^"]*\.tar\.gz' | head -n1)
if [[ -z "$download_url" ]]; then
    echo "Tarball download URL not found"
    return 1
fi
mkdir -p ${APPSHOME}/${APPNAME}
curl -L -o /tmp/vscodium-latest.tar.gz "$download_url"
tar -xzf /tmp/vscodium-latest.tar.gz -C "${APPSHOME}/${APPNAME}" --strip-components=1
ln -sfn ${APPSHOME}/${APPNAME}/bin/codium ${HOME}/.local/bin/codium
rm -f /tmp/vscodium-latest.tar.gz
```

## user-remove
```sh
rm -rf ${APPSHOME}/${APPNAME}
rm -f ${HOME}/.local/bin/codium
```

## user-check
```sh
[ -x ${HOME}/.local/bin/codium ]
```

## user-run
```sh
${HOME}/.local/bin/codium
```

## run
```sh
if apps codium check user; then
   apps codium run user
elif apps codium check appimage; then
   apps codium run appimage
else
   codium
fi
```

