# ifconfig


### ip
```sh
apps ifconfig | jq -r '.ip'
```

### country
```sh
apps ifconfig | jq -r '.country'
```

### europe
```sh
apps ifconfig | jq -r '.country_eu'
```

### default run alias
```sh
curl -sL https://ifconfig.co/json
```
