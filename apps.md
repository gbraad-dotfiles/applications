# Applications launcher

### info


### default alias run
```sh
run_apps() {
  local applist
  apps_list=$(app --list-apps)

  app=$(echo -e "${apps_list[@]}" | column -t -s $'\t' | \
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
    ctrl-r) app $appname run --evaluate ;;
    ctrl-b) app $appname run --background ;;
    ctrl-i) app $appname install ;;
    ctrl-n) app $appname info ;;
    # return 130 to match Ctrl-C behaviour
    f5)     apps-desktop-install "$appname" "$apptitle"; return 130 ;;
    f6)     apps-service-install "$appname" "$apptitle"; return 130 ;;
    *)      app $appname ;;
  esac
}

run_apps
```
