# ranger

## info

## dnf-install
```sh
sudo dnf install -y ranger file
```

## apt-install
```sh
sudo apt install -y ranger
```

## brew-install bluefin-install
```sh
brew install ranger
```

## termux-install
```sh
pkg install ranger
```

## default run
```sh
if ! apps ranger check; then
  apps ranger install
fi
screen ranger
```

## check
```sh
command -v ranger > /dev/null 2>&1
```
