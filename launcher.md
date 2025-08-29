# Launcher

## default alias run
```sh interactive
select_launch_type() {
  local options=("apps" "devenvs" "machines")
  local selected
  selected=$(printf "%s\n" "${options[@]}" | fzf --prompt="Type> ")
  case "$selected" in
    apps)     apps ;;
    devenvs)  devenvs ;;
    machines) machines ;;
    *) echo "Nothing selected"; return 1 ;;
  esac
}

select_launch_type
```
