# Launcher

### shared
```sh
launcher_commands=(
   apps devenvs devboxes machines actions playbooks notebooks userctl
)
```

### resident
"Terminate, Stay Resident"
i
```sh evaluate
while true; do
  app ${APPNAME}
done
```

### default alias run
```sh evaluate
select_launch_type() {
  local selected
  selected=$(printf "%s\n" "${launcher_commands[@]}" | fzf --prompt="Type> ")

  [[ -n $selected ]] && app $selected  
}

select_launch_type
```
