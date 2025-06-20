# Tailscale

## info
**Tailscale** is a zero-config, peer-to-peer VPN based on WireGuard that makes secure networking effortless. It allows you to create a private mesh network between your devices, regardless of their physical location or network. Tailscale is used for remote access, secure connectivity, and simplifying access control for developers and teams.

- Homepage: [https://tailscale.com](https://tailscale.com)
- GitHub: [tailscale/tailscale](https://github.com/tailscale/tailscale)
- Documentation: [https://tailscale.com/kb/](https://tailscale.com/kb/)
- Free for personal use, with paid plans for teams and enterprise features
- Supports Linux, Windows, macOS, iOS, Android, and more
- CLI tool: `tailscale`


## install
```sh
curl -fsSL https://tailscale.com/install.sh | sh
```

## dnf-update
```sh
sudo dnf update -y tailscale
```

## run
```sh
systemctl enable --now tailscaled
```

## up
```sh
tailscale up
```

## status
```sh
tailscale status
```

## online-status
```sh
tailscale status | offline_filter
```

## direct-status
```sh
tailscale status | direct_filter
```

## exitnode-status
```sh
tailscale status | direct_filter
```
