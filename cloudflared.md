# Cloudflared

## shared
```sh
ARCH=`uname -m`
case "$(uname -m)" in
  x86_64) DEB_ARCH="amd64" ;;
  i686|i386) DEB_ARCH="i386" ;;
  armv7l) DEB_ARCH="armhf" ;;
  aarch64) DEB_ARCH="arm64" ;;
  *) DEB_ARCH="$(uname -m)" ;;
esac
```

## dnf-install
```sh
curl -fsSL https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-${ARCH}.rpm -o /tmp/cloudflared.rpm
sudo dnf install -y /tmp/cloudflared.rpm
rm -f /tmp/cloudflared.rpm
```

## apt-install
```sh
case "$(uname -m)" in
  x86_64) DEB_ARCH="amd64" ;;
  i686|i386) DEB_ARCH="i386" ;;
  armv7l) DEB_ARCH="armhf" ;;
  aarch64) DEB_ARCH="arm64" ;;
  *) DEB_ARCH="$(uname -m)" ;;
esac
curl -fsSL https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-${DEB_ARCH}.deb -o /tmp/cloudflared.deb \
sudo apt install -y /tmp/cloudflared.deb
rm -f /tmp/cloudflared.deb
```
