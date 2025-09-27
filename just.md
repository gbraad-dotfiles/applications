# Just

### install
```sh
curl --proto '=https' --tlsv1.2 -sSf https://just.systems/install.sh | bash -s -- --to ~/.local/bin
```

### default run
```sh evaluate
#!/bin/bash
run_just() {
  local recipe
  recipe=$(just --list \
    | grep -v '^Available recipes:' \
    | awk '/^[ ]*[a-zA-Z0-9_-]/ {sub(/^ +/, ""); print}' \
    | fzf --delimiter='#' --with-nth=1,2 \
    | awk '{print $1}')

  if [[ -n "$recipe" ]]; then
    just "$recipe"
  fi
}

run_just
```
