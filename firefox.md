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

## alias default run run-desktop
```sh background
if ! apps firefox check; then
  echo "Not installed. Try to install with: apps firefox install"
  exit 1
fi
firefox
```

## check
```sh
command -v firefox > /dev/null 2>&1
```
