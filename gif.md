# Fuzzy Git

### shared
```sh
gif_commands=(
   status diff add commit
)
```

### default alias run
```sh evaluate
select_gif_action() {
  local selected
  selected=$(printf "%s\n" "${gif_commands[@]}" | fzf --prompt="Do> ")
  case "$selected" in
    status) git status;;
    diff)   git diff ;;
    add)    git status --short | fzf -m | awk '{print $2}' | xargs git add ;;
    commit) git commit ;;
   *) echo "Nothing selected"; return 1 ;;
  esac
}

select_gif_action
```
