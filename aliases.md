# List of aliases

### shared
```sh
clean_args=()
shift 1   # remove filename

while [ "$#" -gt 0 ]; do
  case "$1" in
    --arg)
      shift 2
      ;;
    --arg*)
      shift
      ;;
    *)
      clean_args+=("$1")
      shift
      ;;
  esac
done

set -- "${clean_args[@]}"
```

### test
```sh
echo $@
echo $FOO
```

### default run
```sh
echo "Defined actions:"
app ${APPNAME} --list-actions
```
