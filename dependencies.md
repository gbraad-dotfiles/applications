# Dependencies for dotfiles

### info

### vars
```sh
apps powerline check
powerline_available=$?
apps fzf check
fzf_available=$?
```

### check
```sh
[ $powerline_available -ne 0 ] || [ $fzf_available -ne 0 ]
```

### install
```
if [ $powerline_available -ne 0 ]; then
    apps powerline install pip
fi

if [ $fzf_available -ne 0 ]; then
    apps fzf install
fi
```
