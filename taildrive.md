# Taildrive - share folder with Tailscale

## info

## run
```sh
dirs=$(find . -maxdepth 2 -type d ! -path './.*' ! -path './*/.*' | sed 's|^\./||' | grep -v '^$' | fzf --multi --prompt="Select directories to share: ")
if [ -z "$dirs" ]; then
  echo "No directory selected."
  return 1
fi

while IFS= read -r dir; do
  local fullpath name share_name
  if [[ "$dir" == "." ]]; then
    fullpath="$PWD"
    name="$(basename "$PWD")"
  else
    fullpath="$(realpath "$dir")"
    name="$(basename "$dir")"
  fi
  # Remove leading dot from name, if present
  share_name="${name#.}"
  tailscale drive share "$share_name" "$fullpath"
done <<< "$dirs"
```
