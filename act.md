# Act

## shared
```sh
ACT_VERSION="v0.2.81"
ACT_BASE_URL="https://github.com/nektos/act/releases/download/${ACT_VERSION}"
```


## install
```sh interactive
ARCH=$(uname -m)
case "$ARCH" in
    x86_64)  ACT_FILE="act_Linux_x86_64.tar.gz";;
    aarch64) ACT_FILE="act_Linux_arm64.tar.gz";;
    armv7l)  ACT_FILE="act_Linux_armv7.tar.gz";;
    i686)    ACT_FILE="act_Linux_i386.tar.gz";;
    *)
        echo "Unsupported architecture: $ARCH"
        exit 1
        ;;
esac

curl -sL "$ACT_BASE_URL/$ACT_FILE" -o /tmp/$ACT_FILE
tar -xzf "/tmp/$ACT_FILE" -C ${LOCALBIN}
chmod +x ${LOCALBIN}/act
rm -f "$ACT_FILE"
```