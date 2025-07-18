# Onion

## info

## vars


---




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

