# Machinefile

## info

## vars
```sh
VERSION="0.9.0"
DOWNLOAD_BASEURL="https://github.com/gbraad-redhat/Machinefile/releases/download/${VERSION}"
```

## run
```sh
machinefile
```

## install
```sh
arch=$(uname -m)
 if [[ "$arch" == "x86_64" ]]; then
   DOWNLOAD_URL="${DOWNLOAD_BASEURL}/linux-amd64.tar.gz"
elif [[ "$arch" == "aarch64" ]]; then
   DOWNLOAD_URL="${DOWNLOAD_BASEURL}/linux-arm64.tar.gz"
else
  echo "::error::Unsupported architecture: $arch. Only x86_64 and aarch64 are supported."
  return 1
fi

echo "Downloading Machinefile executor from $DOWNLOAD_URL"
mkdir -p /tmp/machinefile-executor
curl -sSL $DOWNLOAD_URL | tar -xz -C /tmp/machinefile-executor
chmod +x /tmp/machinefile-executor/machinefile
echo "Downloaded and extracted Machinefile executor binary"

mv /tmp/machinefile-executor/machinefile ~/.local/bin/
rm -rf /tmp/machinefile-executor
```

