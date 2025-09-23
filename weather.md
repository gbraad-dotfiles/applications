# Weather

### info
This displays a weather forecast

### ams amsterdam
```sh
app ${APPNAME} query --arg CITY="Amsterdam"
```

### apd apeldoorn
```sh
app ${APPNAME} query --arg CITY="Apeldoorn"
```

### pek peking beijing
```sh
app ${APPNAME} query --arg CITY="Beijing"
```

### query
```sh
curl -sL https://wttr.in/${CITY}
```

### default run alias
```sh
CITY=$(app ${APPNAME} --list-actions | grep -vE '^(query)$' | fzf)
if [[ -n ${CITY} ]]; then
  app ${APPNAME} query --arg CITY=${CITY}
fi
```
