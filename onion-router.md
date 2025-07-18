# Onion

## info


## vars
```sh
APPNAME="onion-router"
APPTITLE="Onion router"
SVCNAME="dotfiles-apps-${APPNAME}"
ORPORT=9001
ORSOCKSPORT=9050
ORCONTROLPORT=9051
```

---

## install-service
```sh interactive
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

## start-service
```sh
systemctl --user start ${SVCNAME}
```

## stop-service
```sh
systemctl --user stop ${SVCNAME}
```

## restart-service
```sh
systemctl --user restart ${SVCNAME}
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

## expose
This exposes the SOCKS5 port on the host tailnet

```sh
tailscale serve --bg --tcp ${ORSOCKSPORT} ${ORSOCKSPORT}
```

---

## centos-install almalinux-install
```sh
echo '[tor]
name=Tor for Enterprise Linux $releasever - $basearch
baseurl=https://rpm.torproject.org/centos/$releasever/$basearch
enabled=1
gpgcheck=1
gpgkey=https://rpm.torproject.org/centos/public_gpg.key
cost=100' | sudo tee /etc/yum.repos.d/tor.repo
sudo dnf install -y tor
```

## fedora-install
```sh
echo '[tor]
name=Tor for Fedora $releasever - $basearch
baseurl=https://rpm.torproject.org/fedora/$releasever/$basearch
enabled=1
gpgcheck=1
gpgkey=https://rpm.torproject.org/fedora/public_gpg.key
cost=100' | sudo tee /etc/yum.repos.d/tor.repo
sudo dnf install -y tor
```

---

## run run-service
```sh interactive
sudo mkdir -p /run/tor
sudo chmod 700 /run/tor
sudo chown ${USER} /run/tor
tor ORPort ${ORPORT} SocksPort ${ORSOCKSPORT} ControlPort ${ORCONTROLPORT}
```
