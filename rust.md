# Rust

### user-install
```sh evaluate
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

### fedora-install dnf-install
This command will install the `rustc` compiler, the standard library, gdb support, the `rustdoc` documentation generator and the `cargo` package manager.

```sh
sudo dnf install rust cargo rustup
```

> [!NOTE]
> This also installs `rustup-init` to start the Rust installation.

### `rustup`
```sh evaluate
rustup-init
```
