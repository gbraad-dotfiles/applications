# Fuzzy Git

### status
```sh
git status
```

### diff
```sh
git diff
```

### add
```sh
git status --short | fzf -m | awk '{print $2}' | xargs git add
```

### commit
```sh
git commit
```

### push
```sh
git push
```

### default alias run
```sh evaluate
select_gif_action() {
  local gif_commands=(
    $(app gif --list-actions | grep -vE '^(run|alias|var|default|shared)$')
  )

  local selected
  selected=$(printf "%s\n" "${gif_commands[@]}" | fzf --prompt="Do> ")
  if [ -z "$selected" ]; then
    echo "Nothing selected"
    return 1
  fi

  app ${APPNAME} "$selected"
}

select_gif_action
```
