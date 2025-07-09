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

## operator-config
```sh
if ! is_root; then
  sudo tailscale set --operator ${USER}
else
  echo "Can't set root as operator"
fi
```

## ssh-config
```sh
tailscale set --ssh=true
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
if [ -n "${TAILSCALE_AUTHKEY}" ]; then
  tailscale up --auth-key "${TAILSCALE_AUTHKEY}"
else
  tailscale up --qr
fi
```

## status
this returns the status and known nodes on the current tailnet
```sh
tailscale status | comment_filter
```

## online-status
This will return the online nodes on the current tailnet

```sh
tailscale status | online_filter
```

## offline-status
This will return the offline nodes on the current tailnet

```sh
tailscale status | offline_filter
```

## direct-status
This will return the direct connections with the current host

```sh
tailscale status | direct_filter
```

## tagged-status
This will return the tagged nodes

```sh
tailscale status | tagged_filter
```

## tsnet-status
This will return the other ts.net nodes

```sh
tailscale status | tsnet_filter
```


## exitnode-status
This will return the exit nodes on the current tailnet

```sh
tailscale status | exitnode_filter
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

selected=$(echo "$exitnodes" | fzf --header="Select an exit node" --with-nth=1,2 --delimiter=$'\t' | cut -f1)

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

## file
```sh
apps taildrop run
```

## drive
```sh
apps taildrive run
```

## ping
```sh
onlinenodes=$(tailscale status --json | jq -r '
  .Peer[] | select(.Online == true) | "\(.DNSName)\t\(.HostName)\t\(.TailscaleIPs[0])"
')

if [[ -z "$onlinenodes" ]]; then
  echo "No online nodes found."
  exit 1
fi

# User selects a node
selected=$(echo "$onlinenodes" | fzf --header="Select an online node to ping" --with-nth=1,2 --delimiter=$'\t')
if [[ -z "$selected" ]]; then
  echo "No node selected."
  exit 2
fi

# Use the DNS name for ping; fallback to IP if DNS is empty
node_dns=$(echo "$selected" | awk -F'\t' '{print $1}')
node_ip=$(echo "$selected" | awk -F'\t' '{print $3}')

target=$node_dns
if [[ -z "$target" ]]; then
  target=$node_ip
fi

tailscale ping $target
```
