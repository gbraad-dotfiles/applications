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

## pkg-install termux-install
```sh
pkg install ranger
```

## default run alias screen 
```sh interactive
if ! apps ranger check; then
  #apps ranger install
  echo "Not installed."
fi
screen ranger
```

## check
```sh
command -v ranger > /dev/null 2>&1
```
