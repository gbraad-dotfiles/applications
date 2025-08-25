# Golang


  - https://github.com/gbraad-devenv/fedora-golang
  - https://github.com/gbraad-devenv/debian-golang
  - https://github.com/gbraad-devenv/ubuntu-golang
  - https://github.com/gbraad-devenv/centos-golang

## debian-install ubuntu-install
```sh
sudo apt-get update
sudo apt-get install -y \
    golang \
    delve
```

## fedora-install centos-install
```sh
sudo dnf install -y --setopt=tsflags=nodocs \
    delve \
    golang \
    golang-bin
```

