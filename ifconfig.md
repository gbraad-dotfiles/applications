# ifconfig


## ip
```sh
apps ifconfig | jq -r '.ip'
```

## country
```sh
apps ifconfig | jq -r '.country'
```

## default run alias
```sh interactive
curl -sL https://ifconfig.co/json
```
