# VCV Rack

### config
```ini
[vcvrack]
   version="RackFree-2.6.6-lin-x64.zip"
   location="Rack2Free"
```

### install
```
mkdir -p ${APPSHOME}/VCVRack
cd ${APPSHOME}/VCVRack
wget https://vcvrack.com/downloads/${VCVRACK_VERSION}
unzip ${VCVRACK_VERSION}
```

### default desktop-run alias run
```sh
cd ${APPSHOME}/VCVRack/${VCVRACK_LOCATION}
./Rack
```
