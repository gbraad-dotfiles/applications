# ifconfig


### ip
```sh
app ifconfig | jq -r '.ip'
```

### country
```sh
app ifconfig | jq -r '.country'
```

### europe
```sh
app ifconfig | jq -r '.country_eu'
```

### default run alias
```sh
curl -sL https://ifconfig.co/json
```
