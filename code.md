# Visual Studio Code


## info

**Visual Studio Code** (VS Code) is a free, open-source source-code editor developed by Microsoft. It is available for Windows, macOS, and Linux. VS Code features support for debugging, embedded Git control, syntax highlighting, intelligent code completion, snippets, and code refactoring. Extensions are available for additional languages, debuggers, and tools.

- Homepage: [https://code.visualstudio.com](https://code.visualstudio.com)
- GitHub: [microsoft/vscode](https://github.com/microsoft/vscode)


## vars
```sh
host=0.0.0.0
port=8000
```

## shared
```sh
get_download_arch() {
    local arch download_target
    arch=$(uname -m)

    case "$arch" in
        x86_64)
            download_target="x64"
            ;;
        i386 | i686)
            download_target="x86"
            ;;
        armv7l)
            download_target="arm32"
            ;;
        aarch64)
            download_target="arm64"
            ;;
        *)
            echo "Unsupported architecture: $arch" >&2
            return 1
            ;;
    esac
    echo "$download_target"
}
```

## apt-install
```sh
curl -SL https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > /tmp/packages.microsoft.gpg
sudo install -D -o root -g root -m 644 /tmp/packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg
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

## flatpak-install
```sh
flatpak install flathub com.visualstudio.code
```

## flatpak-run
```sh
flatpak run com.visualstudio.code
```

## cli-install
```sh
if apps code check cli; then
   echo "Already installed"
   exit 0
fi
download_target=$(get_download_arch)
tempfile=$(mktemp)
curl -fsSL "https://code.visualstudio.com/sha/download?build=stable&os=cli-alpine-${download_target}" -o ${tempfile}
tar -zxvf ${tempfile} -C ${_codepath} > /dev/null 2>&1
rm -f ${tempfile}
echo "Installed code-cli for: ${download_target}"
```

## cli-remove
```sh
rm -f ${LOCALBIN}/code
```

## cli-check
```sh
[ -x ${LOCALBIN}/code ]
```

## user-install
```sh
if apps code check user; then
   echo "Already installed"
   exit 0
fi
download_target=$(get_download_arch)
mkdir -p ${APPSHOME}/code
curl -fL "https://code.visualstudio.com/sha/download?build=stable&os=linux-${download_target}" -o /tmp/vscode.tar.gz
tar -xzf /tmp/vscode.tar.gz --strip-components=1 -C ${APPSHOME}/code
rm /tmp/vscode.tar.gz
#ln -s ${LOCALBIN}/code ${APPSHOME}/code/code
echo "Installed code for: ${download_target}"
```

## user-remove
```sh
rm -rf ${APPSHOME}/code
```

## user-run
```sh
${APPSHOME}/code/code
```

## user-check
```sh
[ -x ${APPSHOME}/code/code ]
```

## run
```sh
if apps code check user; then
   apps code run user
else
   code
fi
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

## serveweb
```sh
code serve-web --without-connection-token --host ${host} --port ${port}
```

## tunnel
```sh
if [ -z "${HOSTNAME}" ]; then
    echo "HOSTNAME not set"
    return 1
fi

code tunnel --accept-server-license-terms --name ${HOSTNAME}
```

## tmux-serveweb
```sh
screen code serveweb
```

## tmux-tunnel
```sh
screen apps code tunnel
```

