# Virtualization for Fedora

## info
  - libvirt

## dnf-install
```sh
sudo dnf group install -y virtualization
```

## apt-install
```sh
sudo apt-get update
sudo apt install -y qemu-kvm libvirt-daemon libvirt-daemon-system virtiofsd virt-manager
sudo usermod -a -G libvirt $USER
```

## default install
