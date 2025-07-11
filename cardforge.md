# Cardforge

## info
Forge is a dynamic and open-source Rules Engine tailored for Magic: The Gathering enthusiasts.

  - Website: https://card-forge.github.io/forge/
  - Source: https://github.com/Card-Forge/forge


## fedora-install
```sh
sudo dnf install -y java-21-openjdk
apps cardforge install user
```

## user-install
```sh
mkdir -p ${APPSHOME}/cardforge
curl -L https://github.com/Card-Forge/forge/releases/download/forge-2.0.00/forge-installer-2.0.00.tar.bz2 -o /tmp/forge-install.tar.bz2
tar -xjvf /tmp/forge-install.tar.bz2 -C ${APPSHOME}/cardforge/
rm -f /tmp/forge-install.tar.bz2
```

## default run
```sh
${APPSHOME}/cardforge/forge.sh
```

## devenv-install
```sh
devenv cardforge system
```

## podman-install
```sh
mkdir -p ~/.cache/forge
podman run -d --name cardforge \
   --hostname ${HOSTNAME}-cardforge -p 8444:8444 \
   --cap-add=NET_ADMIN --cap-add=NET_RAW --device=/dev/net/tun --device=/dev/fuse \
   -v ~/.cache/forge:/home/gbraad/.cache/forge \
   ghcr.io/gbraad-gaming/cardforge:latest
podman exec cardforge tailscale up -qr
ip=`podman exec cardforge tailscale ip -4`
echo "Open CardForge at https://${ip}:8444"
```

