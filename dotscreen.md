# dotscreen Remote tailscale

## info

## run
```sh
selected=$(tailscale status --json | jq -r '
  .Peer[] 
  | select(.sshHostKeys != null and (.sshHostKeys | length > 0))
  | "\(.HostName)\t\(.TailscaleIPs[0])"
' | fzf | awk '{print $2}')

if [ -n "$selected" ]; then
  dotscreen "$selected"
fi
```
