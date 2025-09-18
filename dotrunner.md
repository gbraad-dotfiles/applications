# Dotfiles runners

### vars
```sh
REPO="gbraad-dotfiles/upstream"
REF="main"

RUNNERS_X64=(
  "ubuntu-latest"
  "warp-ubuntu-latest-x64-2x"
  "warp-ubuntu-latest-x64-2x-spot"
  "blacksmith-2vcpu-ubuntu-2404"
)
RUNNERS_ARM=(
  "ubuntu-latest-arm"
  "blacksmith-2vcpu-ubuntu-2404-arm"
)
```

### shared
```sh 
# Select a runner from provided array
select_runner() {
  local runners=("$@")
  printf "%s\n" "${runners[@]}" | fzf --prompt="Runner> "
}

# Select a prefix from a prefix function
select_prefix() {
  local prefix_func="$1"
  "$prefix_func" | fzf --prompt="Prefix> "
}

# Dispatch the workflow with optional inputs
dispatch_workflow() {
  local workflow_file="$1"
  shift

  # Use default branch if REF not set
  local ref="${REF:-main}"

  # Build --field argument list
  local fields=()
  while [[ $# -gt 1 ]]; do
    local k="$1"
    local v="$2"
    fields+=(--field "$k=$v")
    shift 2
  done

  # You can add --ref "$ref" if you want to specify the branch/ref
  gh --repo gbraad-dotfiles/upstream workflow run "$workflow_file" --ref "$ref" "${fields[@]}"
  echo "Dispatched via gh"
}
```

### status
```sh
status_dotrunner() {
  gh --repo gbraad-dotfiles/upstream run list
}

status_dotrunner
```

### runner
```sh
run_runner_workflow() {
  local runner
  runner=$(select_runner "${RUNNERS_X64[@]}" "${RUNNERS_ARM[@]}")
  [[ -z "$runner" ]] && { echo "No runner selected."; return 1; }
  dispatch_workflow "callable-runner.yml" "runs-on" "$runner"
}

run_runner_workflow
```

### devenv
```sh
run_devenv_workflow() {
  local runner prefix
  runner=$(select_runner "${RUNNERS_X64[@]}" "${RUNNERS_ARM[@]}")
  [[ -z "$runner" ]] && { echo "No runner selected."; return 1; }
  prefix=$(select_prefix devenv_prefixes)
  [[ -z "$prefix" ]] && { echo "No prefix selected."; return 1; }
  dispatch_workflow "callable-devenv.yml" "runs-on" "$runner" "prefix_name" "$prefix"
}

run_devenv_workflow
```

### rshell rscreen
```sh
run_rshell_workflow() {
  local runner prefix
  runner=$(select_runner "${RUNNERS_X64[@]}" "${RUNNERS_ARM[@]}")
  [[ -z "$runner" ]] && { echo "No runner selected."; return 1; }
  dispatch_workflow "callable-rscreen.yml" "runs-on" "$runner"
}

run_rshell_workflow
```

### machine
```sh
run_machine_workflow() {
  local runner prefix
  runner=$(select_runner "${RUNNERS_X64[@]}")
  [[ -z "$runner" ]] && { echo "No runner selected."; return 1; }
  prefix=$(select_prefix machine_prefixes)
  [[ -z "$prefix" ]] && { echo "No prefix selected."; return 1; }
  dispatch_workflow "callable-machine.yml" "runs-on" "$runner" "prefix_name" "$prefix"
}

run_machine_workflow
```

### connect
```sh evaluate
app tailshell runner connect
```

### default run alias
```sh
select_workflow_type() {
  local options=("runner" "devenv" "machine" "rshell" "connect" "status")
  local selected
  selected=$(printf "%s\n" "${options[@]}" | fzf --prompt="Workflow type> ")
  case "$selected" in
    runner)  app dotrunner runner ;;
    devenv)  app dotrunner devenv ;;
    rshell)  app dotrunner rshell ;;
    machine) app dotrunner machine ;;
    connect) app dotrunner connect ;;
    status)  app dotrunner status ;;
    *) echo "No workflow type selected."; return 1 ;;
  esac
}

select_workflow_type
```
