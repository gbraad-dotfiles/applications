# Dependencies for dotfiles

### info

### vars
```sh
app powerline check
powerline_available=$?
app fzf check
fzf_available=$?
```

### check
```sh
[ $powerline_available -ne 0 ] || [ $fzf_available -ne 0 ]
```

### install
```
if [ $powerline_available -ne 0 ]; then
    app powerline install pip
fi

if [ $fzf_available -ne 0 ]; then
    app fzf install
fi
```
