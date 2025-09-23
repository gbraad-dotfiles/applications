# Fuzzy Git

### status
```sh
git status
```

### diff
```sh evaluate
git diff | vim -R -c 'set syntax=diff' -
```

### add
```sh
git status --short | fzf -m | awk '{print $2}' | xargs git add
```

### commit
```sh evaluate
git commit
```

### push
```sh
git push
```

### default alias run
```sh evaluate
select_gif_action() {
  local selected
  selected=$(app ${APPNAME} --list-actions | fzf --prompt="Do> ")
  if [ -z "$selected" ]; then
    echo "Nothing selected"
    return 1
  fi

  app ${APPNAME} "$selected"
}

select_gif_action
```
