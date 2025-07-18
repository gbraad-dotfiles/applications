# dotscreen Remote tailscale


## info


## start-runner
```
cd ~/.dotfiles
gh workflow run tailscale-ssh-dotscreen-amd64
```

## run
```sh
selected=$(tailscale status --json | jq -r '
  .Peer[] 
  | select(.sshHostKeys != null and (.sshHostKeys | length > 0))
  | "\(.HostName)\t\(.TailscaleIPs[0])"
' | fzf | awk '{print $2}')

if [ -n "$selected" ]; then
  rscreen "$selected"
fi
```
