# Tailscale user-space

## info

## vars
```sh
STATEDIR=$(dotini tailscale --get "tailproxy.statedir")
STATEDIR="${STATEDIR/#\~/$HOME}"

tailproxy() {
  tailscale -socket "$STATEDIR/userspace.sock" "$@"
}
```

## install
```sh
apps-export-service tailproxy "Tailscale user-space"
```

## start
```sh
systemctl --user start dotfiles-apps-tailproxy
```

## stop
```sh
systemctl --user stop dotfiles-apps-tailproxy
```

## run
```sh
mkdir -p $STATEDIR
tailscaled --tun=userspace-networking \
    --socks5-server=$(dotini tailscale --get tailproxy.socks5-server) \
    --outbound-http-proxy-listen=$(dotini tailscale --get tailproxy.outbound-http-proxy-listen) \
    --state=$STATEDIR/userspace.state \
    --socket=$STATEDIR/userspace.sock \
    --port $(dotini tailscale --get tailproxy.port)
```

## up
```sh
tailproxy status > /dev/null 2>&1
RESULT=$?

if [ $RESULT -eq 1 ]; then
    upcmd="
        tailproxy \
          up \
            --hostname=$(cat /proc/sys/kernel/hostname)-proxy \
            --ssh"
    if [ -n "$TAILSCALE_AUTHKEY" ]; then
      upcmd+=" --auth-key $TAILSCALE_AUTHKEY"
    fi
    eval $upcmd
fi
```

## connect
```sh
secrets var tailscale_authkey
apps tailproxy up
```

## status
```sh
tailproxy status | comment_filter
```

## online-status
This will return the online nodes on the current tailnet

```sh
tailproxy status | offline_filter
```

## direct-status
This will return the direct connections with the current host

```sh
tailproxy status | direct_filter
```

## exitnode-status
This will return the exit nodes on the current tailnet

```sh
tailproxy status | exitnode_filter
```

## select-exitmull
```sh
tailproxy set --exit-node $(tailproxy exit-node list | grep mull | fzf | awk '{print $2}')
```

## select-exitnode
```sh
tailproxy set --exit-node $(apps tailproxy status exitnode | awk '{print $2}' | fzf)
```

## ping
```sh
NODE=$(apps tailproxy status online | awk '{print $1, $2}' | fzf | awk '{print  $1}')
tailproxy ping ${NODE}
```

