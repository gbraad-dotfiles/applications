# Remote tailscale (rshell)


## info


## vars
```sh
APPNAME=dotshell
DOTUSER=${USER}
```

## start-runner
```
cd ~/.dotfiles
gh workflow run tailscale-ssh-dotscreen-amd64
```

## connect-runner
```sh interactive
apps ${APPNAME} run --arg DOTUSER=runner
```

## default alias run
```sh interactive
selected=$(tailscale status --json | jq -r '
  .Peer[]
  | select(.Online == true and .sshHostKeys != null and (.sshHostKeys | length > 0))
  | "\(.HostName)\t\(.TailscaleIPs[0])"
' | column -t -s $'\t' | fzf | awk '{print $2}')

if [ -n "$selected" ]; then
  rshell ${DOTUSER}@${selected}
fi
```
