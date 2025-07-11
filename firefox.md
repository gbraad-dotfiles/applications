# Firefox

## info

## dnf-install fedora-install
```sh
sudo dnf install -y firefox
```

## apt-install ubuntu-install
```sh
sudo apt install -y firefox
```

## default run
```sh background
if ! apps firefox check; then
  apps firefox install
fi
firefox
```

## check
```sh
command -v firefox > /dev/null 2>&1
```
