# Tailscale user-space

## info

## vars
```sh
STATEDIR=$(dotini tailscale --get "tailproxy.statedir")

STATEDIR="${STATEDIR/#\~/$HOME}"
SESSION="tailproxy"
```

## run
```sh
tmux has-session -t $SESSION 2>/dev/null

if [ $? != 0 ]; then
    mkdir -p $STATEDIR
    startcmd="
        tailscaled --tun=userspace-networking \
            --socks5-server=$(dotini tailscale --get tailproxy.socks5-server) \
            --outbound-http-proxy-listen=$(dotini tailscale --get tailproxy.outbound-http-proxy-listen) \
            --state=$STATEDIR/userspace.state \
            --socket=$STATEDIR/userspace.sock \
            --port $(dotini tailscale --get tailproxy.port)"
    tmux new-session -s $SESSION -d
    tmux send -t $SESSION "$startcmd" ENTER
fi
```

## kill
```sh
tmux kill-session -t $SESSION
```

## status
```sh
tailscale -socket $STATEDIR/userspace.sock \
  status
```

## up
```sh
tailscale -socket $STATEDIR/userspace.sock \
  status
RESULT=$?

if [ $RESULT -eq 1 ]; then
    upcmd="
        tailscale -socket $STATEDIR/userspace.sock \
          up \
            --hostname=$(cat /proc/sys/kernel/hostname)-proxy \
            --ssh"
    if [ -n "$TAILSCALE_AUTHKEY" ]; then
      upcmd+=" --auth-key $TAILSCALE_AUTHKEY"
    fi
    eval $upcmd
fi
```

## exitmull
```sh
tailscale -socket $STATEDIR/userspace.sock \
  set --exit-node $(tp exit-node list | grep mull | fzf | awk '{print $2}')
```

## exitnode
```sh
tailscale -socket $STATEDIR/userspace.sock \
  set --exit-node $(tpfexit | awk '{print $2}' | fzf)
```

## ping
```sh
  NODE=$1
  if [ -z "${NODE}" ]; then
    NODE=$(tpfon | awk '{print $1, $2}' | fzf | awk '{print  $1}')
  fi
  tailscale -socket $STATEDIR/userspace.sock \
    ping ${NODE}
```

