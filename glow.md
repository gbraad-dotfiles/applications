# Glow


### info

**Glow** is a command-line tool for rendering Markdown files with syntax highlighting in the terminal. It is designed to provide a visually pleasing Markdown reading experience directly from your shell, supporting local files, GitHub repositories, and markdown piped from standard input.

- Homepage: [https://github.com/charmbracelet/glow](https://github.com/charmbracelet/glow)
- Documentation: [https://github.com/charmbracelet/glow#readme](https://github.com/charmbracelet/glow#readme)
- Prebuilt binaries: [Releases page](https://github.com/charmbracelet/glow/releases)
- Features:
  - Render markdown files in the terminal with rich formatting
  - Browse local and remote markdown files
  - Integration with GitHub for browsing starred and saved repositories
  - Customizable styles and themes


### brew-intsall
```sh
brew install glow
```

### dnf-install
```sh
echo '[charm]
name=Charm
baseurl=https://repo.charm.sh/yum/
enabled=1
gpgcheck=1
gpgkey=https://repo.charm.sh/yum/gpg.key' | sudo tee /etc/yum.repos.d/charm.repo
sudo dnf install -y glow
```

### go-install
```sh
go install github.com/charmbracelet/glow@latest
```

### termux-install
```sh
pkg install glow
```
