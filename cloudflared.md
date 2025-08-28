# Cloudflared

## vars
```sh
APPNAME=cloudflared
APPTITLE="Cloudflare Tunnels"
CF_PORT=8000
SVCNAME=dotfiles-apps-${APPNAME}
```

## install-service
```sh
if ! apps ${APPNAME} check; then
  apps ${APPNAME} install
fi

apps-export-service ${APPNAME} ${APPTITLE}
```

## enable-service
```sh
systemctl --user enable --now ${SVCNAME}
```

## disable-service
```sh
systemctl --user disable --now ${SVCNAME}
```

## restart-service
```sh
apps ${APPNAME} service stop
apps ${APPNAME} service start
```

## start-service
```sh
systemctl --user start ${SVCNAME}
```

## stop-service
```sh
systemctl --user stop ${SVCNAME}
```

## status-service
```sh
systemctl --user status ${SVCNAME}
```

## active-service
```sh
systemctl --user is-active ${SVCNAME}
```

## journal-service
```sh interactive
journalctl --user -u ${SVCNAME} -f
```

---

## check
```sh
[ -x `which cloudflared` ]
```


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

## run run-service
```sh
cloudflared tunnel --url localhost:${CF_PORT}
```
