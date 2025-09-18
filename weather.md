# Weather

### info

### vars
```sh
APPNAME="weather"
CITIES=("Beijing" "Amsterdam" "Apeldoorn")
```

### ams amsterdam ams-alias amsterdam-alias
```sh
apps weather query --arg CITY="Amsterdam"
```

### apd apeldoorn apd-alias apeldoorn-alias
```sh
apps weather query --arg CITY="Apeldoorn"
```

### pek peking beijing pek-alias peking-alias beijing-alias
```sh
apps weather query --arg CITY="Beijing"
```

### query
```sh
curl -sL https://wttr.in/${CITY}
```

### default run alias
```sh
CITY=$(printf "%s\n" "${CITIES[@]}" | fzf)
if [[ -n ${CITY} ]]; then
  apps weather query --arg CITY=${CITY}
fi
unset CITY CITIES
```
