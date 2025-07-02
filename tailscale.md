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
This will enable the `tailscaled` daemon using systemd

```sh
systemctl enable --now tailscaled
```

## up
Starts the onboarding process

```sh
tailscale up
```

## status
this returns the status and known nodes on the current tailnet
```sh
tailscale status
```

## online-status
This will return the online nodes on the current tailnet

```sh
tailscale status | offline_filter
```

## direct-status
This will return the direct connections with the current host

```sh
tailscale status | direct_filter
```

## exitnode-status
T hsi wqill return the exit nodes on the current tailnet

```sh
tailscale status | direct_filter
```

## select-exitnode
```sh
exitnodes=$(tailscale status --json | jq -r '
  .Peer[] 
  | select(.ExitNodeOption == true) 
  | "\(.DNSName)\t\(.HostName)\t\(.Online)\t\(.OS)"')

if [[ -z "$exitnodes" ]]; then
  echo "No exit nodes found."
  return 1
fi

selected=$(echo "$exitnodes" | fzf --header="Select an exit node (tab to preview details)" --with-nth=1,2 --delimiter=$'\t' | cut -f1)

if [[ -z "$selected" ]]; then
  #echo "No exit node selected."
  apps tailscale exitnode clear
  return 2
fi

tailscale set --exit-node="$selected"
echo "Exit node set to $selected"
```

## clear-exitnode
```sh
tailscale set --exit-node ""
echo "Exit node cleared"
```

