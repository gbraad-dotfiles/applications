# SunVox

## info

## vars
```sh
APPNAME=sunvox
```

## install
```sh
mkdir -p ${APPSHOME}/${APPNAME}
curl -l https://warmplace.ru/soft/sunvox/sunvox-2.1.2b.zip -o /tmp/sunvox.zip
unzip /tmp/sunvox.zip -d ${APPSHOME}/
rm -f /tmp/sunvox.zip
```

## check
```sh
[ -x ${APPSHOME}/${APPNAME}/sunvox/linux_x86_64/sunvox ]
```

## default run run-desktop
```sh
${APPSHOME}/${APPNAME}/sunvox/linux_x86_64/sunvox
```
