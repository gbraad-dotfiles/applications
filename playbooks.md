# Playbooks

### config
```ini
[playbooks]
    repo="https://github.com/gbraad-dotfiles/playbooks"
    path="${HOME}/Projects/gbraad-dotfiles/playbooks"
```

### pick
```sh
playbooks_pick() {
  local chosen_target
  chosen_target=$(printf "%s\n" "$(playbooks_list_names $PLAYBOOKS_PATH)" | fzf --prompt="Choose playbook> ")
  chosen_target=$(echo "$chosen_target" | awk '{print $1}')
  echo ${PLAYBOOKS_PATH}/${chosen_target}
}
playbooks_pick
```

### shared
```sh
playbooks_list_names() {
  local playbookpath="$1"
  find -L "$playbookpath" -type d -name '.*' -prune -o \
    \( \( -name "*.yml" -o -name "*.yaml" \) \
       ! -name "requirements.yml" ! -name "requirements.yaml" \) -type f -print \
    | sed -E "s|^$playbookpath/||" | sed -E 's/\.(yml|yaml)$//' | sort | uniq \
    | while read relname; do
      for ext in yml yaml; do
        local file="$playbookpath/$relname.$ext"
        if [[ -f "$file" ]]; then
          # Extract the first "- name:" line not indented more than 2 spaces
          # (i.e., play-level, not tasks)
          local title=$(grep -m1 -E '^[ ]{0,2}-[ ]name:' "$file" | sed -E 's/^[ ]*- name:[ ]*//')
          if [[ -n "$title" ]]; then
            echo "$relname.$ext\t$title"
          else
            echo "$relname.$ext"
          fi
        fi
      done
    done
}

playbooks_fuzzy_pick() {
  local dir=$1
  local list=$(playbooks_list_names $dir)
  local pbselect
  pbselect=$(printf "%s\n" "$list"| column -t -s $'\t' | \
    fzf --prompt="Select playbook: " \
        --header=$'Enter: select\tCtrl+R: execute\tCtrl+E: edit' \
        --bind "ctrl-r:accept" \
        --expect=enter,ctrl-r,ctrl-e)

  local -a pb_line
  pb_line=("${(@f)pbselect}")
  local key="${pb_line[1]}"
  local selected="${pb_line[2]}"

  [[ -z "$selected" ]] && return 1
  local -a fields
  fields=(${(z)selected})
  local pbfile="${fields[1]}"
  pbfile="${dir}/${pbfile}"

  case "$key" in
    ctrl-r) echo "${pbfile} execute"; return ;;
    ctrl-s) echo "${pbfile} sync"; return ;;
    ctrl-e) echo "${pbfile} edit"; return ;;
    # remote   tailshell online nodes
    # machine  running machines
    # devenv   running devenvs
    *)      ;;
  esac

  echo ${pbfile} execute
}
```

### exists
```sh
[ -d ${PLAYBOOKS_PATH} ]
```

### cd
```sh evaluate
cd ${PLAYBOOKS_PATH}
```

### checkout
```sh
git clone ${PLAYBOOKS_REPO} ${PLAYBOOKS_PATH}
```

### update
```sh
cd ${PLAYBOOKS_PATH}
git pull
```

### here
```sh evaluate
app ${APPNAME} run --arg PBWHERE=$(pwd)
```

### default alias run
```sh evaluate
run_playbooks() {
  if [ -n "$1" ] && [ -d "$1" ]; then
    dir=$1
  else
    if ! app ${APPNAME} exists ; then
      app ${APPNAME} checkout
    fi
    dir=${PLAYBOOKS_PATH}
  fi

  local result picked_playbook picked_command
  result=($(playbooks_fuzzy_pick "$dir"))

  local exitcode=$?
  picked_playbook=${result[1]}
  picked_command=("${result[@]:1}")

  [[ "$exitcode" -gt 0 ]] && return $exitcode
  [[ -z $picked_command ]] && return 3

  local picked_host
  # if command remote, devenv, machine
  # "remote")
  #   app tailshell pick online
  # "devenv")
  #   app devenvs pick running
  # "machine")
  #   app machines pick running
  picked_host=""

  playbook $picked_playbook $picked_command $picked_host
}

run_playbooks $PBWHERE
unset PBWHERE
```
