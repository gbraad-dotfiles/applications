# Actionfiles

### vars
```sh
ACTREPO="https://github.com/gbraad-dotfiles/actionfiles"
ACTPATH="${HOME}/Projects/gbraad-dotfiles/actionfiles"
```

### exists
```sh
[ -d ${ACTPATH} ]
```

### cd
```sh evaluate
cd ${ACTPATH}
```

### checkout
```sh
git clone ${ACTREPO} ${ACTPATH}
```

### update
```sh
cd ${ACTPATH}
git pull
```

### `reset runner`
```sh
run ${ACTPATH}/runner.md reset permissions
```

### `compile crc`
```sh
run ${ACTPATH}/projects/crc.md compile
```

### `compile macadam`
```sh
run ${ACTPATH}/projects/macadam.md compile
```

### `compile gvproxy`
```sh
run ${ACTPATH}/projects/gvproxy.md compile
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
alias dotfedora="run ${ACTPATH}/machine/dotfedora.md"
```

### aliases
Generate aliases for Actionfiles that use an `alias` section

> [!NOTE]
> Needs to run in `interactive`-mode to allow the aliases to be exported

```sh evaluate
actions_aliases() {
  local actname actfile folder alias_name alias_cmd
  if [[ $(dotini apps --get "apps.aliases") == true ]]; then
    # Find all .md files in APPSDIR and subfolders
    find "${ACTPATH}" -type f -name '*.md' | while read -r mdfile; do

      actfile="${mdfile##*/}"
      actname="${actfile%.md}"

      folder="$(dirname "${mdfile}")"
      folder="${folder##*/}"

      if [[ "${folder}" == "$(basename "${ACTPATH}")" ]]; then
        alias_name="${actname}"
        alias_cmd="action ${ACTPATH}/${actfile}"
      else
        alias_name="${folder}-${actname}"
        alias_cmd="action ${ACTPATH}/${folder}/${actfile}"
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
