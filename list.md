# list

## update
```sh
cd ${_appsdefpath}
git pull
cd -
```

## switch
```sh
cd ${_appsdefpath}
git remote remove origin
git remote add origin git@github.com:gbraad-dotfiles/applications
git fetch
git branch --set-upstream-to=origin/main main
cd -
```
