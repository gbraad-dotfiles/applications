# Dotfiles runners

## vars
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

## shared
```sh 
# Extract machine prefixes from dotini machine --list
machine_prefixes() {
  local key="disks"
  local output prefixes
  output=($(dotini machine --list | grep "^${key}\." || true))
  prefixes=(${output//${key}./})
  prefixes=(${prefixes//=*/})
  printf "%s\n" "${prefixes[@]}"
}

# Extract devenv prefixes from dotini devenv --list
devenv_prefixes() {
  local key="images"
  local output prefixes
  output=($(dotini devenv --list | grep "^${key}\." || true))
  prefixes=(${output//${key}./})
  prefixes=(${prefixes//=*/})
  printf "%s\n" "${prefixes[@]}"
}

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
  if [[ -z "$GH_TOKEN" ]]; then
    secrets var gh_token
  fi

  if [[ -z "$GH_TOKEN" ]]; then
    echo "GH_TOKEN is not set and could not be retrieved from secrets. Exiting."
  fi

  local workflow_file="$1"
  shift
  local data="{\"ref\":\"$REF\",\"inputs\":{"
  local first=1
  while [[ $# -gt 1 ]]; do
    local k="$1"
    local v="$2"
    if [[ $first -eq 0 ]]; then data+=", "; fi
    data+="\"$k\":\"$v\""
    first=0
    shift 2
  done
  data+="}}"
  curl -X POST \
    -H "Accept: application/vnd.github+json" \
    -H "Authorization: Bearer $GH_TOKEN" \
    "https://api.github.com/repos/$REPO/actions/workflows/$workflow_file/dispatches" \
    -d "$data"
  echo "Dispatched"
}
```

## runner
```sh interactive
run_runner_workflow() {
  local runner
  runner=$(select_runner "${RUNNERS_X64[@]}" "${RUNNERS_ARM[@]}")
  [[ -z "$runner" ]] && { echo "No runner selected."; return 1; }
  dispatch_workflow "callable-runner.yml" "runs-on" "$runner"
}

run_runner_workflow
```

## devenv
```sh interactive
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

## rshell rscreen
```sh interactive
run_rshell_workflow() {
  local runner prefix
  runner=$(select_runner "${RUNNERS_X64[@]}" "${RUNNERS_ARM[@]}")
  [[ -z "$runner" ]] && { echo "No runner selected."; return 1; }
  dispatch_workflow "callable-rscreen.yml" "runs-on" "$runner"
}

run_rshell_workflow
```

## machine
```sh interactive
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

## default run alias
```sh interactive
select_workflow_type() {
  local options=("runner" "devenv" "machine" "rshell" "connect")
  local selected
  selected=$(printf "%s\n" "${options[@]}" | fzf --prompt="Workflow type> ")
  case "$selected" in
    runner)  apps dotrunner runner ;;
    devenv)  apps dotrunner devenv ;;
    rshell)  apps dotrunner rshell ;;
    machine) apps dotrunner machine ;;
    connect) apps dotshell runner connect ;;
    *) echo "No workflow type selected."; return 1 ;;
  esac
}

select_workflow_type
```
