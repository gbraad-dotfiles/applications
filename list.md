# Application list

## shared
```sh
apps_dir="${_appsdefpath:-$HOME/.dotapps}"

show_apps_tree() {
  local dir="$1"
  local prefix="$2"

  # List only non-hidden entries (skip dotfiles/dirs)
  local entries=()
  while IFS= read -r entry; do
    local base=$(basename "$entry")
    [[ "$base" == .* ]] && continue         # skip hidden
    [[ "$base" == "README.md" ]] && continue # skip README.md
    entries+=("$entry")
  done < <(find "$dir" -maxdepth 1 -mindepth 1 | sort)

  local total=${#entries[@]}
  local idx=0

  for entry in "${entries[@]}"; do
    idx=$((idx+1))
    local base=$(basename "$entry")
    local connector="├──"
    [ "$idx" -eq "$total" ] && connector="└──"

    if [ -d "$entry" ]; then
      echo "${prefix}${connector} $base"
      show_apps_tree "$entry" "$prefix│   "
    elif [[ "$base" == *.md ]]; then
      if [ -L "$entry" ]; then
        target=$(readlink "$entry")
        base="${base%.md} -> ${target%.md}"
        descfile="$dir/$(basename "$target")"
      else
        base="${base%.md}"
        descfile="$entry"
      fi
      if [ -f "$descfile" ]; then
        desc=$(grep -m1 '^# ' "$descfile" | sed 's/^# //')
      else
        desc=""
      fi
      printf "%s%s %-20s -  %s\n" "$prefix" "$connector" "$base" "$desc"
    fi
  done
}
```

## update
```sh
cd ${_appsdefpath}
git pull
cd -
```

## switch
```sh
cd ${apps_dir}
git remote remove origin
git remote add origin git@github.com:gbraad-dotfiles/applications
git fetch
git branch --set-upstream-to=origin/main main
cd - > /dev/null
```

## show
```sh
echo "App defintions in ${apps_dir}"
show_apps_tree ${apps_dir} "  "
```

## reset
```sh
cd ${_appsdefpath}
git stash
git fetch origin
git reset --hard origin/main
cd - > /dev/null
```
