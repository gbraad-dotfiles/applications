# Notebooks

### vars
```sh
NBREPO="https://github.com/gbraad-dotfiles/notebooks"
NBPATH="${HOME}/Projects/gbraad-dotfiles/notebooks"
```

### shared
```sh
notebooks_list_names() {
  local notebookpath=$1
  find -L "$notebookpath" \( -name "*.md" -o -name "*.ipynb" \) -type f \
    | sed -E "s|^$notebookpath/||" | sed -E 's/\.(md|ipynb)$//' | sort | uniq \
    | while read relname; do
        out=""
        [[ -f "$notebookpath/$relname.md" ]] && out="$relname.md"
        [[ -f "$notebookpath/$relname.ipynb" ]] && out="$out $relname.ipynb"
        [[ -n "$out" ]] && echo $out
      done
}

notebooks_fuzzy_pick() {
  local dir=$1
  local list=$(notebooks_list_names $dir)
  local nbselect
  nbselect=$(printf "%s\n" "$list" | \
    fzf --prompt="Select notebook: " \
        --header=$'Enter: select\tCtrl+R: execute\tCtrl+S: sync\tCtrl+E: edit' \
        --bind "ctrl-r:accept" \
        --expect=enter,ctrl-r,ctrl-s,ctrl-e)

  local -a nb_line
  nb_line=("${(@f)nbselect}")
  local key="${nb_line[1]}"
  local selected="${nb_line[2]}"

  [[ -z "$selected" ]] && return 1
  local -a fields
  fields=(${(z)selected})
  local nbfile="${fields[1]}"
  nbfile="${dir}/${nbfile}"

  case "$key" in
    ctrl-r) echo "${nbfile} execute"; return ;;
    ctrl-s) echo "${nbfile} sync"; return ;;
    ctrl-e) echo "${nbfile} edit"; return ;;
    *)      ;;
  esac

  echo ${nbfile} execute
}
```

### exists
```sh
[ -d ${NBPATH} ]
```

### cd
```sh evaluate
cd ${NBPATH}
```

### checkout
```sh
git clone ${NBREPO} ${NBPATH}
```

### update
```sh
cd ${NBPATH}
git pull
```

### here
```sh evaluate
app ${APPNAME} run --arg NBWHERE=$(pwd)
```

### resident
"Terminate, Stay Resident"

```sh evaluate
while true; do
  app ${APPNAME}
done
```

### resident-here
```sh evaluate
while true; do
  app ${APPNAME} here
done
```

### default alias run
```sh evaluate
run_notebooks() {
  if [ -n "$1" ] && [ -d "$1" ]; then
    dir=$1
  else
    dir=${NBPATH}
    if ! app ${APPNAME} exists ; then
      app ${APPNAME} checkout
    fi
  fi

  local result picked_notebook picked_command
  result=($(notebooks_fuzzy_pick "$dir"))

  local exitcode=$?
  picked_notebook=${result[1]}
  picked_command=("${result[@]:1}")

  [[ "$exitcode" -gt 0 ]] && return $exitcode
  [[ -z $picked_command ]] && return 3

  notebook $picked_notebook $picked_command  
}

run_notebooks $NBWHERE
unset NBWHERE
```

