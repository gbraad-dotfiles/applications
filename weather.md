# Weather

### info

### vars
```sh
APPNAME="weather"
CITIES=("Beijing" "Amsterdam" "Apeldoorn")
```

### ams amsterdam ams-alias amsterdam-alias
```sh
app weather query --arg CITY="Amsterdam"
```

### apd apeldoorn apd-alias apeldoorn-alias
```sh
app weather query --arg CITY="Apeldoorn"
```

### pek peking beijing pek-alias peking-alias beijing-alias
```sh
app weather query --arg CITY="Beijing"
```

### query
```sh
curl -sL https://wttr.in/${CITY}
```

### default run alias
```sh
CITY=$(printf "%s\n" "${CITIES[@]}" | fzf)
if [[ -n ${CITY} ]]; then
  app weather query --arg CITY=${CITY}
fi
unset CITY CITIES
```
