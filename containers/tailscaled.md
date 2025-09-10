# Tailscale (container)

### info

  - https://github.com/spotsnel/tailscale-systemd


### podman-install
```sh 
podman run -d --name=scalesys \
    --hostname $HOSTNAME-scalesys --network=host --systemd=always \
    ghcr.io/spotsnel/tailscale-systemd/fedora:latest
```

