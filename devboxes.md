# Development Boxes

### default alias run
```sh interactive
run_devboxes() {

  chosen_command=$(printf "%s\n" "${devbox_commands[@]}" | fzf --prompt="Devbox command> ")
  [[ -z "$chosen_command" ]] && return

  if [[ "$chosen_command" == "stop" ]]; then
    targets=$(devbox_running_targets)
    [[ -z "$targets" ]] && echo "No running environments found." && return
    target_list="$targets"
  elif [[ "$chosen_command" == "remove" ]]; then
    targets=$(devbox_targets)
    [[ -z "$targets" ]] && echo "No environments found." && return
    target_list="$targets"
  elif [[ "$chosen_command" == "from-devenv" ]]; then
    targets=$(devenv_prefixes)
    [[ -z "$targets" ]] && echo "No environments found." && return
    target_list="$targets"
  else
    targets=$(devbox_targets)
    target_list=""
    declare -A known_prefixes
    if [[ -n "$targets" ]]; then
      while IFS=$'\t' read -r prefix state; do
        target_list+="$prefix\t[$state]\n"
        known_prefixes["$prefix"]=1
      done <<< "$targets"
    fi
    for prefix in $(devbox_prefixes); do
      if [[ -z "${known_prefixes[$prefix]}" ]]; then
        target_list+="$prefix\t[Create]\n"
      fi
    done
  fi

  target_list=$(echo -e "$target_list" | sed '/^$/d')
  chosen_target=$(echo -e "$target_list" | column -t -s $'\t' | fzf --prompt="Choose target> ")
  [[ -z "$chosen_target" ]] && return

  target_prefix=$(echo "$chosen_target" | awk '{print $1}')

  # status shown; choose command
  if [[ "$chosen_command" == "status" ]]; then
    chosen_command=$(printf "%s\n" "${devbox_commands[@]}" | fzf --prompt="Devbox command> ")
    [[ -z "$chosen_command" ]] && return                                                     
  fi

  if [[ "$chosen_command" == "from" ]]; then
    freeform_name=$(echo | fzf --prompt="Enter environment name (or select)> " --print-query --phony | head -n1)
    [[ -z "$freeform_name" ]] && return
    [[ -z "$target_prefix" ]] && target_prefix=$(devbox_prefixes | fzf --prompt="Select prefix> ")
    [[ -z "$target_prefix" ]] && return
    devbox "$freeform_name" from "$target_prefix"
    return
  elif [[ "$chosen_command" == "from-devenv" ]]; then
    freeform_name=$(echo | fzf --prompt="Enter environment name (or select)> " --print-query --phony | head -n1)
    [[ -z "$freeform_name" ]] && return 
    devbox "$freeform_name" from-devenv "$target_prefix"
    return
  fi

  if [[ "$chosen_command" == "playbook" ]]; then
    playbook_file=$(find . -type f ! -path './.*/*' -name '*.yml' -o -name '*.yaml' | sed 's|^\./||' | fzf --prompt="Select playbook: " --exit-0)
    if [ -z "$playbook_file" ]; then
      echo "No playbook selected."
      return 1
    fi
    devbox $target_prefix playbook "$playbook_file"
    return
  fi

  

  devbox "$target_prefix" "$chosen_command"
  return
}

run_devboxes
```
