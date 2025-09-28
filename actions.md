# Actions

### vars
```sh
ACTPATH="${HOME}/Projects/gbraad-dotfiles/actionfiles"
```

### shared
```sh
actions_list_names_and_descs() {
  local actionpath="$1"
  find -L "$actionpath" -type f -name '*.md' | sort | while IFS= read -r file; do
    relpath="${file#$actionpath/}"
    desc=$(grep -m1 '^# ' "$file" | sed 's/^# //')
    printf "%s\t%s\n" "$relpath" "$desc"
  done
}

actions_fuzzy_pick() {
  local dir=$1
  local list=$(actions_list_names_and_descs $dir)
  local actselect
  actselect=$(printf "%s\n" "${list[@]}" | column -t -s $'\t' | \
    fzf --prompt="Select actionfile: " \
        --header=$'Enter: select\tCtrl+R: run\tCtrl+B: run bg' \
        --bind "ctrl-r:accept" \
        --expect=enter,ctrl-r,ctrl-i,ctrl-b)

  local -a act_line actname acttitle
  act_line=("${(@f)actselect}")
  key="${act_line[1]}"
  selected="${act_line[2]}"

  [[ -z "$selected" ]] && return 1
  fields=(${(z)selected})
  actname="${fields[1]}"
  acttitle="${fields[2, -1]}"

  case "$key" in
    ctrl-r) echo "$actfile run"; return ;;
    ctrl-b) echo "$actfile run --background"; return ;;
    *)      ;;
  esac

  [[ -z $actname ]] && return 1

  local action
  action=$(action $dir/$actname --list-actions | grep -vE '^(info|alias|vars|default|shared)$' | tac | fzf --prompt="Select action: ")
  
  [[ -z $action ]] && return 2

  echo $dir/$actname $action
}

### here
```sh evaluate
app ${APPNAME} run --arg WHERE=$(pwd)
```

### resident
"Terminate, Stay Resident"

```sh evaluate
while true; do
  app ${APPNAME}
done
```

### resident-here
```sh evaluate
while true; do
  app ${APPNAME} here
done
```

### default alias run
```sh evaluate
run_actions() {
  if [ -n "$1" ] && [ -d "$1" ]; then
    dir=$1
  else
    dir=${ACTPATH}
    if ! app actionfiles exists ; then
      app actionfiles checkout
    fi
  fi

  local result picked_actfile picked_action
  result=($(actions_fuzzy_pick "$dir"))
  local exitcode=$?
  picked_actfile=${result[1]} 
  picked_action=("${result[@]:1}") 

  [[ "$exitcode" -gt 0 ]] && return $exitcode
  [[ -z $picked_action ]] && return 3

  # inject ${APPNAME} to be compatible with app
  local appname
  appname="${picked_actfile%.md}"
  appname="${appname##*/}"
  local common_args=(--arg APPNAME="${appname}")
  # --arg CONFIGPATH="${APPSCONFIG}")

  action "$picked_actfile" "${picked_action[@]}" ${common_args[@]}
}

run_actions $WHERE
```
