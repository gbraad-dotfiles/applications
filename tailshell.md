# Remote tailscale (rshell)


### vars
```sh
DOTUSER=${USER}
```

### pick
```sh
tailshell_pick_online() {
  tailscale status --json | jq -r '
    .Peer[]
    | select(.sshHostKeys != null and (.sshHostKeys | length > 0))
    | "\(.HostName)\t\(.TailscaleIPs[0])"
  ' | column -t -s $'\t' | fzf | awk '{print $2}'
}
tailshell_pick_online
```

### online-pick
```sh
tailshell_pick_online() {
  tailscale status --json | jq -r '
    .Peer[]
    | select(.Online == true and .sshHostKeys != null and (.sshHostKeys | length > 0))
    | "\(.HostName)\t\(.TailscaleIPs[0])"
  ' | column -t -s $'\t' | fzf | awk '{print $2}'
}
tailshell_pick_online
```

### start-runner
```
cd ~/.dotfiles
gh workflow run tailscale-ssh-dotscreen-amd64
```

### connect-runner
```sh evaluate
app ${APPNAME} run --arg DOTUSER=runner
```

### default alias run
```sh evaluate
run_tailshell() {
  local dotuser selected
  dotuser=$1
  
  selected=$(app ${APPNAME} pick online)

  if [ -n "$selected" ]; then
    rshell ${dotuser}@${selected}
  fi
}

run_tailshell ${DOTUSER}
unset FILENAME
unset DOTUSER
```
