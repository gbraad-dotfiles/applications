# Taildrive - share folder with Tailscale

## info

Select directories you would like to share with other nodes on your tailnet. They can be accessed by WebDAV via http://100.100.100.100:8080/...
You can select multiple directories with `<TAB>`, and confirm the selection with `<ENTER>`. `<ESC>` cancels the function.


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
