# Remote tailscale (rscreen)


## info


## vars
```sh
APPNAME=dotscreen
DOTUSER=${USER}
```

## start-runner
```
cd ~/.dotfiles
gh workflow run tailscale-ssh-dotscreen-amd64
```

## runner-user
```sh interactive
apps ${APPNAME} run --arg DOTUSER=runner
```

## gbraad-user
```sh interactive
apps ${APPNAME} run --arg DOTUSER=gbraad
```

## root-user
```sh interactive
apps ${APPNAME} run --arg DOTUSER=root
```

## default alias run
```sh interactive
selected=$(tailscale status --json | jq -r '
  .Peer[] 
  | select(.Online == true and .sshHostKeys != null and (.sshHostKeys | length > 0))
  | "\(.HostName)\t\(.TailscaleIPs[0])"
' | fzf | awk '{print $2}')

if [ -n "$selected" ]; then
  rscreen ${DOTUSER}@${selected}
fi
```

