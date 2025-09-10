# GitHub CLI

### info

### fedora-install
```sh
sudo dnf install dnf5-plugins
sudo dnf config-manager addrepo --from-repofile=https://cli.github.com/packages/rpm/gh-cli.repo
sudo dnf install -y gh --repo gh-cli
```

### centos-install
```sh
sudo dnf install 'dnf-command(config-manager)'
sudo dnf config-manager --add-repo https://cli.github.com/packages/rpm/gh-cli.repo
sudo dnf install -y gh --repo gh-cli
```

### apt-install
```sh
sudo mkdir -p -m 755 /etc/apt/keyrings \
out=$(mktemp) && wget -nv -O$out https://cli.github.com/packages/githubcli-archive-keyring.gpg \
cat $out | sudo tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null \
sudo chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
sudo apt update \
sudo apt install gh -y
```

### bluefin-install brew-install
```sh
brew install gh
```

### secret
```sh
mkdir -p ${HOME}/.config/gh
secrets file ghcli ${HOME}/.config/gh/hosts.yml
```
