# Development Environments

## shared
```sh
devenv_targets() {
  devenv | awk -F' - ' '{sub(/sys$/, "", $1); print $1 "\t" $2}'
}

devenv_running_targets() {
  devenv | awk -F' - ' '$2 ~ /^Up/ {sub(/sys$/, "", $1); print $1 "\t" $2}'
}
```

## default alias run
```sh interactive
run_devenvs() {

  chosen_command=$(printf "%s\n" "${devenv_commands[@]}" | fzf --prompt="Devenv command> ")
  [[ -z "$chosen_command" ]] && return

  if [[ "$chosen_command" == "status" ]]; then
    devenv_targets
    return
  fi

  if [[ "$chosen_command" == "from" ]]; then
    freeform_name=$(echo | fzf --prompt="Enter environment name (or select)> " --print-query --phony | head -n1)
    [[ -z "$freeform_name" ]] && return
    chosen_prefix=$(devenv_prefixes | fzf --prompt="Select prefix> ")
    [[ -z "$chosen_prefix" ]] && return
    devenv "$freeform_name" from "$chosen_prefix"
    return
  fi

  if [[ "$chosen_command" == "stop" ]]; then
    targets=$(devenv_running_targets)
    [[ -z "$targets" ]] && echo "No running environments found." && return
    target_list="$targets"
  elif [[ "$chosen_command" == "remove" ]]; then
    targets=$(devenv_targets)
    [[ -z "$targets" ]] && echo "No environments found." && return
    target_list="$targets"
  else
    targets=$(devenv_targets)
    target_list=""
    declare -A known_prefixes
    if [[ -n "$targets" ]]; then
      while IFS=$'\t' read -r prefix state; do
        target_list+="$prefix\t[$state]\n"
        known_prefixes["$prefix"]=1
      done <<< "$targets"
    fi
    for prefix in $(devenv_prefixes); do
      if [[ -z "${known_prefixes[$prefix]}" ]]; then
        target_list+="$prefix\t[Create]\n"
      fi
    done
  fi

  target_list=$(echo -e "$target_list" | sed '/^$/d')
  chosen_target=$(echo -e "$target_list" | column -t -s $'\t' | fzf --prompt="Choose target> ")
  [[ -z "$chosen_target" ]] && return

  target_prefix=$(echo "$chosen_target" | awk '{print $1}')

  devenv "$target_prefix" "$chosen_command"
  return
}

run_devenvs
```
