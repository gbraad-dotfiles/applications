# Launcher

### shared
```sh
launcher_commands=(
   app devenvs devboxes machines userctl
)
```

### default alias run
```sh
select_launch_type() {
  local selected
  selected=$(printf "%s\n" "${launcher_commands[@]}" | fzf --prompt="Type> ")
  case "$selected" in
    apps)     app ;;
    devenvs)  app devenvs ;;
    devboxes) app devboxes ;;
    machines) app machines ;;
    userctl)  app userctl ;; 
   *) echo "Nothing selected"; return 1 ;;
  esac
}

select_launch_type
```
