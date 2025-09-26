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
#apps="$(app list services)"
#apps_fuzzy_pick <<< "$apps"
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
          --header=$'Enter: select\tCtrl+R: run\tCtrl+B: run bg\tCtrl+S: screen\tCtrl+I: install\tCtrl+J: info\tF5: export .desktop\tF6: export .service\tCtrl+E: edit\tCtrl+N: new\tCtrl+K: config'\
          --bind "ctrl-r:accept" \
          --expect=enter,ctrl-r,ctrl-s,ctrl-i,ctrl-j,ctrl-n,ctrl-b,ctrl-e,ctrl-k,f5,f6 )

  app_line=("${(@f)app}")
  key="${app_line[1]}"
  selected="${app_line[2]}"
  fields=(${(z)selected})
  appname="${fields[1]}"
  apptitle="${fields[2,-1]}"

  case "$key" in
    ctrl-r) echo "${appname} run"; return ;;
    ctrl-s) echo "${appname} run --screen"; return ;;
    ctrl-b) echo "${appname} run --background"; return ;;
    ctrl-i) echo "${appname} install"; return ;;
    ctrl-j) echo "${appname} info"; return ;;
    ctrl-n)
      new_appname=$(fzf --prompt="New app name: " --phony --print-query --bind 'enter:accept')
      new_appname=$(echo "$new_appname" | head -n1)
      if [[ -n "$new_appname" ]]; then
        if [[ "$new_appname" == */* ]]; then
          dirpart="${new_appname%/*}"
          mkdir -p -- "$dirpart"
        fi
        echo "${new_appname} --edit"
      fi
      return
      ;;
    ctrl-e) echo "${appname} --edit"; return ;;
    ctrl-k) echo "${appname} --config"; return ;;
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

### resident
```sh evaluate
while true; do
  app ${APPNAME}
done
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
