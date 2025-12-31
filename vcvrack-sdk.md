# VCV-Rack SDK

### activate default
```sh evaluate
if [ -d "${APPSHOME}/VCVRack/Rack-SDK" ]; then 
  export RACK_DIR=${APPSHOME}/VCVRack/Rack-SDK
else
  echo "Not installed"
fi
```

### install alias
```sh
mkdir -p ${APPSHOME}/VCVRack/
cd ${APPSHOME}/VCVRack/
wget https://vcvrack.com/downloads/Rack-SDK-2.5.2-lin-x64.zip
unzip Rack-SDK-2.5.2-lin-x64.zip 
wget https://vcvrack.com/downloads/Rack-SDK-2.5.2-win-x64.zip
unzip Rack-SDK-2.5.2-win-x64.zip -d Rack-SDK-win-x64
```

