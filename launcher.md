# Launcher

## shared
```sh
launcher_commands=(
   apps devenvs machines userctl
)
```

## default alias run
```sh interactive
select_launch_type() {
  local selected
  selected=$(printf "%s\n" "${launcher_commands[@]}" | fzf --prompt="Type> ")
  case "$selected" in
    apps)     apps ;;
    devenvs)  apps devenvs ;;
    machines) apps machines ;;
    userctl)  apps userctl ;; 
   *) echo "Nothing selected"; return 1 ;;
  esac
}

select_launch_type
```
