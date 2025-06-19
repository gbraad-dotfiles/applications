# Visual Studio Code


## info

**Visual Studio Code** (VS Code) is a free, open-source source-code editor developed by Microsoft. It is available for Windows, macOS, and Linux. VS Code features support for debugging, embedded Git control, syntax highlighting, intelligent code completion, snippets, and code refactoring. Extensions are available for additional languages, debuggers, and tools.

- Homepage: [https://code.visualstudio.com](https://code.visualstudio.com)
- GitHub: [microsoft/vscode](https://github.com/microsoft/vscode)
- Snap: `snap install code --classic`
- Flatpak: `flatpak install flathub com.visualstudio.code`
- Arch (AUR): `yay -S visual-studio-code-bin`
- Ubuntu/Debian: [Official .deb download](https://code.visualstudio.com/Download)
- Fedora/RHEL: [Official .rpm download](https://code.visualstudio.com/Download)


## apt-install
```sh
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg
echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" |sudo tee /etc/apt/sources.list.d/vscode.list > /dev/null
rm -f packages.microsoft.gpg
sudo apt update
sudo apt install -y code
```

## apt-remove
```sh
sudo apt remove -y code
sudo rm /etc/apt/sources.list.d/vscode.list
```

## dnf-install
```sh
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\nautorefresh=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" | sudo tee /etc/yum.repos.d/vscode.repo > /dev/null
sudo dnf install -y code
```

## dnf-remove
```sh
sudo dnf remove -y code
sudo rm /etc/yum.repos.d/vscode.repo
```

## cli-install
```sh
_installcode
```

## cli-remove
```sh
rm -rf -f ${_codepath}/code
```

## run
```sh
code
```

## user-serveweb
```sh
mkdir -p ~/.config/systemd/user/
curl -fsSL  https://raw.githubusercontent.com/gbraad-vscode/code-systemd/refs/heads/main/user/code-serveweb.service \
  -o ~/.config/systemd/user/code-serveweb.service
curl -fsSL  https://raw.githubusercontent.com/gbraad-vscode/code-systemd/refs/heads/main/user/code-tunnel.service   \
  -o ~/.config/systemd/user/code-tunnel.service
systemctl --user daemon-reload
systemctl --user enable --now code-serveweb
```

## system-serveweb
```sh
sudo curl -fsSL https://raw.githubusercontent.com/gbraad-vscode/code-systemd/refs/heads/main/system/code-serveweb%40.service \
  -o /etc/systemd/system/code-serveweb@.service
sudo curl -fsSL https://raw.githubusercontent.com/gbraad-vscode/code-systemd/refs/heads/main/system/code-tunnel%40.service   \
  -o /etc/systemd/system/code-tunnel@.service
sudo systemctl daemon-reload
sudo systemctl enable --now code-serveweb@${USER}
```
