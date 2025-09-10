# OCI registry client

### info

### install
```sh
VERSION="1.2.0"
local TEMP_DIR=$(mktemp -d)
curl -fsSL "https://github.com/oras-project/oras/releases/download/v${VERSION}/oras_${VERSION}_linux_amd64.tar.gz" -o ${TEMP_DIR}/oras.tar.gz
tar -zxf ${TEMP_DIR}/oras.tar.gz -C ${TEMP_DIR}/
mv ${TEMP_DIR}/oras ${LOCALBIN}
rm -rf ${TEMP_DIR}/
```
