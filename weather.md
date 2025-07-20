# Weather

## info

## vars
```sh
APPNAME="weather"
CITIES=("Beijing" "Amsterdam" "Apeldoorn")
```

## ams amsterdam
```sh
apps weather query --arg CITY="Amsterdam"
```

## apd apeldoorn
```sh
apps weather query --arg CITY="Apeldoorn"
```

## default pek peking beijing run
```sh
apps weather query --arg CITY="Beijing"
```

## query
```sh
curl -sL https://wttr.in/${CITY}
```

## default run alias
```sh interactive
CITY=$(printf "%s\n" "${CITIES[@]}" | fzf)
if [[ -n ${CITY} ]]; then
  apps weather query --arg CITY=${CITY}
fi
unset CITY CITIES
```
