# Authorized keys

### default alias run
```sh
mkdir -p ~/.ssh/
curl https://github.com/gbraad.keys | tee -a ~/.ssh/authorized_keys
curl https://dotfiles.gbraad.nl/public.keys | tee -a ~/.ssh/authorized_keys
```
