# Cloudflare (container)

### info

  - https://github.com/spotsnel/cloudflared-systemd/


### podman-install
```sh
podman run -d --name=flaresys \
    --hostname $HOSTNAME-flaresys --network=host --systemd=always \
    ghcr.io/spotsnel/cloudflared-systemd/fedora:latest
```
