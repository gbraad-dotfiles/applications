# System install

## info

## default fedora-install
```sh
sudo dnf install -y \
    mc vim tmux screen links lynx z

#powerline
sudo dnf install -y \
    powerline vim-powerline tmux-powerline

# fonts
sudo dnf install -y \
    adobe-source-han-mono-fonts \
    adobe-source-code-pro-fonts \
    adobe-source-sans-pro-fonts \
    adobe-source-serif-pro-fonts \
    powerline-fonts

# optiona
sudo dnf install -y \
    python3-pygments \
    python3-pip
pip install pygments-style-tomorrownightbright

# vim
sudo dnf install -y \
    vim-syntastic-asciidoc \
    vim-syntastic-html \
    vim-syntastic-css \
    vim-syntastic-vim \
    vim-syntastic-yaml \
    vim-syntastic-json \
    vim-syntastic-go \
    vim-syntastic-zsh \
    vim-airline \
    vim-go \
    vim-ctrlp \
    vim-gitgutter \
    vim-fugitive \
    vim-nerdtree

# ranger
sudo dnf install -y \
    ranger \
    highlight

# graphical
sudo dnf install -y \
    i3 \
    python3-i3ipc \
    feh

sudo dnf install -y \
    trash-cli \
    fzf \
    vgrep \
    fd-find \
    ripgrep
```
