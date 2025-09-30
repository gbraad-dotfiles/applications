# Actionfiles

### config
```ini
[actionfiles]
    repo="https://github.com/gbraad-dotfiles/actionfiles"
    path="${HOME}/Projects/gbraad-dotfiles/actionfiles"
```

### exists
```sh evaluate
[ -d ${ACTIONFILES_PATH} ]
```

### cd
```sh evaluate
cd ${ACTIONFILES_PATH}
```

### checkout
```sh
git clone ${ACTIONFILES_REPO} ${ACTIONFILES_PATH}
```

### update
```sh
cd ${ACTIONFILES_PATH}
git pull
```

### default run alias
```sh
if ! app ${APPNAME} exists; then
  app ${APPNAME} checkout
fi
app ${APPNAME} aliases
```

### aliases
```sh evaluate
alias dotfedora="run ${ACTIONFILES_PATH}/machine/dotfedora.md"
```

### aliases
Generate aliases for Actionfiles that use an `alias` section

> [!NOTE]
> Needs to run in `interactive`-mode to allow the aliases to be exported

```sh evaluate
actions_aliases() {
  local actname actfile folder alias_name alias_cmd
  if [[ $(dotini apps --get "apps.aliases") == true ]]; then
    # Find all .md files in ACTIONFILES_PATH and subfolders
    find "${ACTIONFILES_PATH}" -type f -name '*.md' | while read -r mdfile; do

      actfile="${mdfile##*/}"
      actname="${actfile%.md}"

      folder="$(dirname "${mdfile}")"
      folder="${folder##*/}"

      if [[ "${folder}" == "$(basename "${ACTIONFILES_PATH}")" ]]; then
        alias_name="${actname}"
        alias_cmd="action ${ACTIONFILES_PATH}/${actfile}"
      else
        alias_name="${folder}-${actname}"
        alias_cmd="action ${ACTIONFILES_PATH}/${folder}/${actfile}"
      fi

      if grep -E -q '^###.*\balias\b' "$mdfile"; then
        alias "${alias_name}"="${alias_cmd}"
      fi
    done
  fi
}

actions_aliases
unset FILENAME
unset APPNAME
unset TITLE
```
