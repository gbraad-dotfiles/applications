# Taildrop - Tailscale file transfer

### info

Uses Tailscale and `fzf` to send files to a target machine. Use `<TAB>` to select multiple files and `<ENTER>` to confirm selection.
`<ESC>` cancels the function.


### alias default run
```sh
if [ ! -n "$(find . -maxdepth 1 -type f)" ]; then
  echo "No files in the current folder."
  return 1
fi

files=$(find . -type f ! -path './.*/*' | sed 's|^\./||' | fzf --multi --prompt="Select files to send: " --exit-0)
if [ -z "$files" ]; then
  echo "No files selected."
  return 1
fi

nodes=$(tailscale status --json | jq -r '.Peer[] | select(.Online == true) | [.HostName, .DNSName, .TailscaleIPs[0]] | @tsv')
if [ -z "$nodes" ]; then
  echo "No online Tailscale nodes found."
  return 2
fi

target=$(echo "$nodes" | awk -F'\t' '{print $1 " (" $2 ") [" $3 "]"}' | fzf --prompt="Select Tailscale node: "  --exit-0)
if [ -z "$target" ]; then
  echo "No target selected."
  return 3
fi

# Extract DNSName (inside parentheses)
target_dns=$(echo "$target" | sed -E 's/.*\(([^)]+)\).*/\1/')

echo "Sending files to $target_dns..."
tailscale file cp $files "$target_dns:"
```
