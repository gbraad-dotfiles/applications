# Fuzzy Git

### status
```sh
git status
```

### patch
```sh evaluate
if git diff --quiet; then
  echo "No changes"
  return 0
fi
git diff > /tmp/gif.patch
vim /tmp/gif.patch
git checkout -b cleaned-changes HEAD
git apply /tmp/gif.patch
git add .
git commit -m "Apply cleaned changes"
```

### reset
```sh evaluate
if git diff --quiet; then
  echo "No changes"
  return 0
fi
git reset -p 
```

### stage
```sh evaluate
if git diff --quiet; then
  echo "No changes"
  return 0
fi
git status --short | fzf -m | awk '{print $2}' | xargs -o -I{} git add -e {}
```

### rebase
```sh evaluate
commit=$(git log --oneline --decorate | fzf --reverse --prompt="Rebase from> " | awk '{print $1}')
if [ -n "$commit" ]; then
  git rebase -i "$commit"
else
  echo "No commit selected."
fi
```

### diff
```sh evaluate
git diff | vim -R -c 'set syntax=diff' -
```

### add
```sh
if git diff --quiet; then
  echo "No changes"
  return 0
fi
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
