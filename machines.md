# Machines

### default alias run
```sh interactive
run_machines() {

  chosen_command=$(printf "%s\n" "${machine_commands[@]}" | fzf --prompt="Machine command> ")
  [[ -z "$chosen_command" ]] && return 1

  # need a running machine
  if [[ "$chosen_command" == "stop" || "$chosen_command" == "switch" || "$chosen_command" == "apps" || "$chosen_command" == "playbook" || "$chosen_command" == "copy-id" || "$chosen_command" == "shell" ]]; then
    targets=$(machine_running_targets)
    [[ -z "$targets" ]] && echo "No running VMs found." && return
    target_list=$(echo -e "$targets")
  # need a known target
  elif [[ "$chosen_command" == "remove" ]]; then
    targets=$(machine_targets)
    [[ -z "$targets" ]] && echo "No VMs found." && return
    target_list=$(echo -e "$targets")
  # any target
  else
    targets=$(machine_targets)
    target_list=""
    declare -A known_prefixes
    if [[ -n "$targets" ]]; then
      while IFS=$'\t' read -r prefix state; do
        target_list+="$prefix\t[$state]\n"
        known_prefixes["$prefix"]=1
      done <<< "$targets"
    fi
    for prefix in $(machine_prefixes); do
      if [[ -z "${known_prefixes[$prefix]}" ]]; then
        target_list+="$prefix\t[Create]\n"
      fi
    done
    target_list=$(echo -e "$target_list" | sed '/^$/d')
  fi

  target_list=$(echo -e "$target_list" | sed '/^$/d')
  chosen_target=$(echo -e "$target_list" | column -t -s $'\t' | fzf --prompt="Choose target> ")
  [[ -z "$chosen_target" ]] && return
  target_prefix=$(echo "$chosen_target" | awk '{print $1}')

  if [[ "$chosen_command" == "status" ]]; then
    chosen_command=$(printf "%s\n" "${devenv_commands[@]}" | fzf --prompt="Devenv command> ")
    [[ -z "$chosen_command" ]] && return                                                       
  fi

  chosen_deployment=""
  if [[ "$chosen_command" == "switch" ]]; then
    chosen_deployment=$(machine_deployments | fzf --prompt="Select> ")
    [[ -z "$chosen_deployment" ]] && return
  fi
  
  if [[ "$chosen_command" == "from" ]]; then
    freeform_name=$(echo | fzf --prompt="Enter machine name (or select)> " --print-query --phony | head -n1)
    [[ -z "$freeform_name" ]] && return 1
    [[ -z "$target_prefix" ]] && target_prefix=$(machine_prefixes | fzf --prompt="Select prefix> ")
    [[ -z "$target_prefix" ]] && return 1
    machine "$freeform_name" from "$target_prefix"
    return
  fi
  
  if [[ "$chosen_command" == "playbook" ]]; then
    playbook_file=$(find . -type f ! -path './.*/*' -name '*.yml' -o -name '*.yaml' | sed 's|^\./||' | fzf --prompt="Select playbook: " --exit-0)
    if [ -z "$playbook_file" ]; then
      echo "No playbook selected."
      return 1
    fi
    machine $target_prefix playbook "$playbook_file"
    return
  fi

  machine "$target_prefix" "$chosen_command" $chosen_deployment
  return
}

run_machines
```
