# Cardforge

## info
Forge is a dynamic and open-source Rules Engine tailored for Magic: The Gathering enthusiasts.


## fedora-install
```sh
sudo dnf install -y java-21-openjdk
apps cardforge install user
```

## user-install
```sh
mkdir -p ${APPSHOME}/cardforge
wget https://github.com/Card-Forge/forge/releases/download/forge-2.0.00/forge-installer-2.0.00.tar.bz2 -O /tmp/forge-install.tar.bz2
tar -xjvf /tmp/forge-install.tar.bz2 -C ${APPSHOME}/cardforge/
rm -f /tmp/forge-install.tar.bz2
```

## run
```sh
${APPSHOME}/cardforge/forge.sh
```
