# Onion

## info


## fedora-install
```sh
echo '[tor]
name=Tor for Fedora $releasever - $basearch
baseurl=https://rpm.torproject.org/fedora/$releasever/$basearch
enabled=1
gpgcheck=1
gpgkey=https://rpm.torproject.org/fedora/public_gpg.key
cost=100' | sudo tee /etc/yum.repos.d/tor.repo
sudo dnf install -y torbrowser-launcher
```

---

## default alias run run-desktop
```sh
torbrowser-launcher
```
