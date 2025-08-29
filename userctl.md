# Systemctl manager for user


## shared
```sh
userctl_commands=(
  status start stop enable disable remove
)

userctl_units() {
  find ~/.config/systemd/user/ -maxdepth 1 -type f -name "*.service" \
    | xargs -n1 basename \
    | sed 's/\.service$//'
}
```

## default alias run
```sh interactive
select_userctl_command() {
  local chosen_command=$(printf "%s\n" "${userctl_commands[@]}" | fzf --prompt="userctl command> ")
  [[ -z "$chosen_command" ]] && return

  unit=$(userctl_units | fzf --prompt="Select unit> ")
  [[ -z "$unit" ]] && return

  if [[ "$chosen_command" == "remove" ]]; then
    rm -i ~/.config/systemd/user/"$unit.service"
    systemctl --user daemon-reload
    echo "$unit.service removed."
  else
    systemctl --user "$chosen_command" "$unit.service"
  fi
}

select_userctl_command
```
