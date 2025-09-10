# Systemctl manager for user


### shared
```sh
userctl_commands=(
  status restart start stop enable disable remove journal
)

userctl_units() {
  find ~/.config/systemd/user/ -maxdepth 1 -type f -name "*.service" \
    | xargs -n1 basename \
    | sed 's/\.service$//'
}

userctl_units_status() {
  for name in $(userctl_units); do
    state=$(systemctl --user is-active "${name}.service" 2>/dev/null)
    substate=$(systemctl --user show -p SubState --value "${name}.service" 2>/dev/null)
    echo -e "${name}\t[${state} (${substate})]"
  done
}

userctl_units_status_desc() {
  for name in $(userctl_units); do
    state=$(systemctl --user is-active "${name}.service" 2>/dev/null)
    substate=$(systemctl --user show -p SubState --value "${name}.service" 2>/dev/null)
    description=$(systemctl --user show -p Description --value "${name}.service" 2>/dev/null)
    echo -e "${name}\t${description}\t[${state} (${substate})]"
  done
}

create_user_session() {
  sudo macinectl shell ${USER}@ /usr/bin/echo "User session created"
}
```

### default alias run
```sh interactive
run_userctl() {
  local chosen_command=$(printf "%s\n" "${userctl_commands[@]}" | fzf --prompt="userctl command> ")
  [[ -z "$chosen_command" ]] && return 1

  # List of relevant units
  local target_list=""
  #if [[ "$chosen_command" == "remove" ]]; then
  #  # Only show installed units
  #  target_list=$(userctl_units | awk '{print $1 "\t[Installed]"}')
  #else
    # Show all units with status columns
    target_list=$(userctl_units_status_desc)
  #fi

  target_list=$(echo -e "$target_list" | column -t -s $'\t' | fzf --prompt="Choose unit> ")
  [[ -z "$target_list" ]] && return
  local unit=$(echo "$target_list" | awk '{print $1}')

  if [[ "$chosen_command" == "status" ]]; then
    chosen_command=$(printf "%s\n" "${userctl_commands[@]}" | fzf --prompt="userctl command> ")
    [[ -z "$chosen_command" ]] && return                                                       
  fi

  if [[ "$chosen_command" == "remove" ]]; then
    rm -i ~/.config/systemd/user/"$unit.service"
    systemctl --user daemon-reload
    echo "$unit.service removed."
  elif [[ "$chosen_command" == "journal" ]]; then
    journalctl --user -u "$unit.service"
  else
    systemctl --user "$chosen_command" "$unit.service"
  fi
}

run_userctl
```
