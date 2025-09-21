# Applications launcher

### info

### pick all
```sh evaluate
app --list-apps | apps_fuzzy_pick
```

### desktop
```sh
app list desktop | apps_fuzzy_pick
```

### services
```sh
app list services | apps_fuzzy_pick
```

### shared 
```sh evaluate
apps_fuzzy_pick() {
  local apps_list=()
  while IFS= read -r line; do
    apps_list+=("$line")
  done

  app=$(printf "%s\n" "${apps_list[@]}" | column -t -s $'\t' | \
      fzf --prompt="Select app: " \
          --header=$'Enter: select\tCtrl+R: run\tCtrl+B: run bg\tCtrl+I: install\tCtrl+N: info\tF5: export .desktop\tF6: export .service' \
          --bind "ctrl-r:accept" \
          --expect=enter,ctrl-r,ctrl-i,ctrl-n,ctrl-b,f5,f6 )

  local -a app_line appname apptitle
  app_line=("${(@f)app}")
  key="${app_line[1]}"
  selected="${app_line[2]}"

  [[ -z "$selected" ]] && return 1
  fields=(${(z)selected})
  appname="${fields[1]}"
  apptitle="${fields[2, -1]}"

  case "$key" in
    ctrl-r) app $appname run; return ;;
    ctrl-b) app $appname run --background; return ;;
    ctrl-i) app $appname install; return ;;
    ctrl-n) app $appname info; return ;;
    # return 130 to match Ctrl-C behaviour
    f5)     apps_desktop_install "$appname" "$apptitle"; return 130 ;;
    f6)     apps_service_install "$appname" "$apptitle"; return 130 ;;
    *)      ;;
  esac

  [[ -z $appname ]] && return 1

  local action
  action=$(app $appname --list-actions | tac | fzf --prompt="Select action: ")
  
  [[ -z $action ]] && return 2

  echo $appname $action
}
```

### update

> [!NOTE]
> `evaluate` so aliases get properly set in the current shell

```sh evaluate
app list update
```

### cd
```sh evaluate
cd ${APPSREPO}
```

### default alias run
```sh evaluate
run_apps() {
  action=($(apps all))
  local exitcode=$?
  [[ "$exitcode" -gt 0 ]] && return $exitcode
  [[ -z $action ]] && return 3

  app $action
}

run_apps
```
